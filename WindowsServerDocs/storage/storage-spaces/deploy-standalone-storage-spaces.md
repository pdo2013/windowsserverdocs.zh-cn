---
title: 在独立服务器上部署存储空间
description: 介绍如何在独立的基于 Windows Server 2012 的服务器上部署存储空间。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-spaces
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 40265e767ac9aca05386c0893def259aca3a5633
ms.sourcegitcommit: e73fbe1046a8bd2bf4f24ccffc11465ad8dfab1d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/07/2019
ms.locfileid: "8992530"
---
# 在独立服务器上部署存储空间

>适用于： Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

本主题介绍了如何在独立服务器上部署存储空间。 有关如何创建群集的存储空间的信息，请参阅[部署 Windows Server 2012 R2 上的一个存储空间群集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>)。

若要创建的存储空间，你必须首先创建一个或多个存储池。 存储池是物理磁盘的集合。 存储池，使存储聚合、 弹性容量扩展和委派的管理。

从存储池中，你可以创建一个或多个虚拟磁盘。 这些虚拟磁盘也称为*存储空间*。 存储空间此时将显示为 Windows 操作系统中，如你可以从中创建的常规磁盘格式化的卷。 创建虚拟磁盘通过文件和存储服务的用户界面时，你可以配置的复原类型 (简单，镜像或奇偶校验)，预配类型 （精简或固定） 和大小。 通过 Windows PowerShell 中，你可以设置其他参数，如和列数，则交错值，若要使用的是池中的物理磁盘。 有关这些其他参数的信息，请参阅[新 VirtualDisk](https://docs.microsoft.com/powershell/module/storage/new-virtualdisk?view=win10-ps)和[什么是列和存储空间是如何确定多少用于？](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx%23what_are_columns_and_how_does_storage_spaces_decide_how_many_to_use)在存储空间经常常见问题 (FAQ)。

>[!NOTE]
>存储空间不能用于托管 Windows 操作系统。

从虚拟磁盘，你可以创建一个或多个卷。 创建卷时，你可以配置的大小、 驱动器号或文件夹、 文件系统 （NTFS 文件系统或复原文件系统 (ReFS)）、 分配单元大小和一个可选的卷的标签。

下图说明了存储空间工作流。

![存储空间工作流](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**图 1： 的存储空间工作流**

>[!NOTE]
>本主题包括示例 Windows PowerShell cmdlet，你可以使用自动执行某些描述的过程。 有关详细信息，请参阅[PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6)。

## 先决条件

若要在独立的 Windows Server 2012−based 服务器上使用存储空间，请确保你想要使用的物理磁盘满足以下先决条件。

> [!IMPORTANT]
> 如果你想要了解如何在故障转移群集上部署存储空间，请参阅[部署 Windows Server 2012 R2 上的一个存储空间群集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>)。 故障转移群集部署具有不同的先决条件，如受支持的磁盘总线类型、 受支持的复原类型和磁盘所需的最小数。

|区域|要求|注释|
|---|---|---|
|磁盘总线类型|串行附加 SCSI (SAS)<br>串行高级技术连接 (SATA)<br>-iSCSI 和光纤通道控制器。 |你还可以使用 USB 驱动器。 但是，并非最佳的服务器环境中使用 USB 驱动器。<br>存储空间支持在 iSCSI 和光纤通道 (FC) 控制器上，只要在其顶部创建的虚拟磁盘的非复原 （简单使用任意数量的列）。<br>|
|磁盘配置|的必须至少 4 GB 物理磁盘。<br>-磁盘必须为空，并不格式。 不要创建卷。||
|HBA 注意事项|的建议简单的主机总线适配器 (Hba) 不支持 RAID 功能<br>-如果能够 RAID Hba 必须处于非 RAID 模式与所有 RAID 功能已禁用<br>-适配器不得抽象物理磁盘，缓存数据，或甚至会遮盖所有连接的设备。 This includes enclosure services that are provided by attached just-a-bunch-of-disks (JBOD) devices. |存储空间是仅兼容 Hba 可以完全禁用所有 RAID 功能。|
|JBOD enclosures|- JBOD enclosures are optional<br>建议使用存储空间认证机箱列出 Windows Server 目录<br>- If you're using a JBOD enclosure, verify with your storage vendor that the enclosure supports Storage Spaces to ensure full functionality<br>- To determine whether the JBOD enclosure supports enclosure and slot identification, run the following Windows PowerShell cmdlet:<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>如果**EnclosureNumber**和**SlotNumber**字段包含值，则机箱支持这些功能。||

若要规划物理磁盘和独立服务器部署所需的复原类型的数量，请使用以下指南。

|复原类型|磁盘要求|使用时间|
|---|---|---|
|**简单**<br><br>-在物理磁盘上分布数据<br>-最大磁盘容量并提高吞吐量<br>-（不保护从磁盘故障） 没有复原<br><br><br><br><br><br><br>|需要至少一个物理磁盘。|不要使用主机无法恢复数据。 简单空间不能防止磁盘失败。<br><br>使用主机临时或轻松地重新创建数据，以降低成本。<br><br>适合高性能工作负荷复原不是必需的或已提供的应用程序的位置。|
|**镜像**<br><br>-跨不同的一组物理磁盘存储数据的两个或三个副本<br>-提高可靠性，但可减少容量。 重复出现每次写入。 镜像空间也在多个物理驱动器分布数据。<br>大数据吞吐量和更低的访问权限延迟比奇偶校验<br>-使用更新跟踪 (DRT) 来跟踪修改池中的磁盘区域。 当系统恢复从非计划停机关闭和空格联机时，DRT 会使磁盘池中彼此保持一致。|需要至少两个物理磁盘，以防止单个磁盘失败。<br><br>需要至少五个物理磁盘，以防止两个同时磁盘失败。|大多数部署使用。 例如，镜像空间适合于一般用途的文件共享或虚拟硬盘 (VHD) 库。|
|**奇偶校验**<br><br>-在物理磁盘上分布数据和奇偶校验的信息<br>-但时，它为简单的空间，相比某种程度上减少容量提高可靠性<br>-通过日志记录增加复原。 这有助于防止数据损坏，如果未计划的关机发生。|需要至少三个物理磁盘，以防止单个磁盘失败。|用于工作负荷，高度顺序，如存档或备份。|

## 步骤 1： 创建存储池

必须提供第一个物理磁盘组到一个或多个存储池。

1. 在服务器管理器导航窗格中，选择**文件和存储服务**。

2. 在导航窗格中，选择**存储池**页面。
    
    默认情况下，可用磁盘包含名为*原始*池池中。 如果没有原始池列出下**存储池**，这表示存储不符合存储空间的要求。 请确保磁盘满足先决条件部分所述的要求。
    
    >[!TIP]
    >如果你选择**原始**的存储池，可用物理磁盘均在**物理磁盘**。

3. 在**存储池**，选择**任务**列表中，，然后选择**新的存储池**。 新的存储池向导将打开。

4. 在**开始之前**页面上，选择**下一步**。

5. 在**指定的存储池名称和子系统**页上，输入的名称和可选说明存储池，选择所需的可用物理磁盘的组，然后选择**下一步**。

6. 在**选择存储池的物理磁盘**页上，执行下面，，然后选择**下一步**:
    
    1. 选择你想要包含在存储池中每个物理磁盘旁边的复选框。
    
    2. 如果你想要指定一个或多个磁盘作为热备用，**分配**，在选择的下拉箭头，然后选择**热备用**。

7. 在**确认选择**页上，验证设置正确，，然后选择**创建**。

8. 在**查看结果**页上，验证所有任务已完成，然后选择**关闭**。
    
    >[!NOTE]
    >（可选） 若要直接继续下一步，可以选择**创建虚拟磁盘关闭向导时**复选框。

9. 在**存储池**，验证列出了新的存储池。

### Windows PowerShell 创建存储池的等效命令

以下 Windows PowerShell cmdlet 执行上述过程相同的功能。 在同一行，输入每个 cmdlet，即使它们可能会显示自动换行跨多个行下面由于格式约束。

下面的示例演示的物理磁盘处于可用原始池。

```PowerShell
Get-StoragePool -IsPrimordial $true | Get-PhysicalDisk | Where-Object CanPool -eq $True
```

下面的示例创建新的存储池名为*StoragePool1*使用所有可用的磁盘。

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk –CanPool $True)
```

下面的示例创建新的存储池， *StoragePool1*，使用四个可用磁盘。

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk PhysicalDisk1, PhysicalDisk2, PhysicalDisk3, PhysicalDisk4)
```

以下示例序列 cmdlet 显示了如何将可用物理磁盘*PhysicalDisk5*为热备用添加到存储池*StoragePool1*。

```PowerShell
$PDToAdd = Get-PhysicalDisk –FriendlyName PhysicalDisk5
Add-PhysicalDisk –StoragePoolFriendlyName StoragePool1 –PhysicalDisks $PDToAdd –Usage HotSpare
```

## 步骤 2： 创建虚拟磁盘

接下来，你必须从存储池中创建一个或多个虚拟磁盘。 创建虚拟磁盘时，你可以选择数据在物理磁盘的布局方式。 这将影响可靠性和性能。 你还可以选择是否要创建或修复的精简配置的磁盘。

1. 如果尚未打开，请在**存储池**页面在服务器管理器中，新的虚拟磁盘向导下**存储池**，确保所需的存储池处于选中状态。

2. 在**虚拟磁盘**，选择**任务**列表中，，然后选择**新建虚拟磁盘**。 新建虚拟磁盘向导将打开。

3. 在**开始之前**页面上，选择**下一步**。

4. 在**选择存储池**页上，选择所需的存储池，然后选择**下一步**。

5. 在**指定的虚拟磁盘名称**页上，输入的名称和可选描述，然后选择**下一步**。

6. 在**选择存储布局**页上，选择所需的布局，然后选择**下一步**。
    
    >[!NOTE]
    >如果你选择你没有足够的物理磁盘的布局，你将收到一条错误消息，当你选择**下一步**。 有关使用与磁盘要求的布局的信息，请参阅[先决条件](#prerequisites)）。

7. 如果你选择**镜像**作为存储布局中，并且池中有五个或多个磁盘，将显示**配置复原设置**页面。 选择以下选项之一：
    
      - **双向镜像**
      - **三向镜像**

8. 在**指定的预配类型**页上，选择以下选项之一，然后选择**下一步**。
    
      - **精简**
        
        使用精简预配，根据需要分配空间。 此优化可用的存储的使用情况。 但是，因为这使你能够过度分配存储，你必须仔细监视的可用磁盘空间。
    
      - **固定**
        
        使用固定的预配，创建一个虚拟磁盘时立即分配的存储容量。 因此，从存储池中，它等于虚拟磁盘大小固定预配的使用空间。
    
    >[!TIP]
    >如果具有存储空间，可以在相同的存储池创建两个和固定的精简配置的虚拟磁盘。 例如，你可以使用精简配置的虚拟磁盘托管数据库和固定的预配虚拟磁盘托管相关联的日志文件。

9. 在**指定的虚拟磁盘大小**页上，请执行以下操作：
    
    如果你选择精简预配上一步中，在**虚拟磁盘大小**框中，输入虚拟磁盘大小，选择的单位 （**MB**、 **GB**或**TB**），然后选择**下一步**。
    
    如果你选择固定预配上一步中，选择以下选项之一：
    
      - **指定大小**
        
        若要指定大小，在**虚拟磁盘大小**框中，输入的值，然后选择 （**MB**、 **GB**或**TB**） 的单位。
        
        如果你使用简单以外的存储布局，虚拟磁盘使用比你指定大小的多个可用空间。 若要避免可能出现的错误卷的大小超出了存储池可用空间，你可以选择**创建最大的虚拟磁盘可能，达到指定大小**复选框。
    
      - **最大大小**
        
        选择此选项以创建虚拟磁盘使用的存储池最大容量。

10. 在**确认选择**页上，验证设置正确，，然后选择**创建**。

11. 在**查看结果**页上，验证所有任务已完成，然后选择**关闭**。
    
    >[!TIP]
    >默认情况下，选择**创建卷时关闭向导**复选框。 这使您直接进入下一步。

### Windows PowerShell 创建虚拟磁盘的等效命令

以下 Windows PowerShell cmdlet 执行上述过程相同的功能。 在同一行，输入每个 cmdlet，即使它们可能会显示自动换行跨多个行下面由于格式约束。

下面的示例创建名为*VirtualDisk1*上存储池名为*StoragePool1*50 GB 虚拟磁盘。

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

下面的示例创建名为*VirtualDisk1*上存储池名为*StoragePool1*镜像虚拟磁盘。 该磁盘使用存储池的最大存储容量。

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –ResiliencySettingName Mirror –UseMaximumSize
```

下面的示例创建名为*VirtualDisk1*上的存储池，名为*StoragePool1*50 GB 虚拟磁盘。 磁盘使用精简预配类型。

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB) –ProvisioningType Thin
```

下面的示例创建一个名为*VirtualDisk1*名为*StoragePool1*存储池上的虚拟磁盘。 虚拟磁盘使用三向镜像，并且是固定的大小的 20 GB。

>[!NOTE]
>在此 cmdlet 来进行工作的存储池必须至少包含五台物理磁盘。 （这不包括任何分配为热备用的磁盘）。

```PowerShell
New-VirtualDisk -StoragePoolFriendlyName StoragePool1 -FriendlyName VirtualDisk1 -ResiliencySettingName Mirror -NumberOfDataCopies 3 -Size 20GB -ProvisioningType Fixed
```

## 步骤 3： 创建卷

接下来，你必须从虚拟磁盘创建卷。 你可以分配一个可选的驱动器号或文件夹，然后格式具有文件系统的卷。

1. 如果尚未打开，请在**存储池**页面服务器管理器中，在**虚拟磁盘**，新建卷向导右键单击所需的虚拟磁盘，然后选择**新卷**。
    
    新建卷向导将打开。

2. 在**开始之前**页面上，选择**下一步**。

3. 在**选择服务器和磁盘**页上，执行下，，然后选择**下一步**。
    
    1. 在**服务器**区域中，选择你想要预配卷的服务器。
    
    2. 在**磁盘**区域中，选择你想要创建卷的虚拟磁盘。

4. 在**指定卷的大小**页上，输入卷的大小，指定的单位 （**MB**、 **GB**或**TB**），然后选择**下一步**。

5. 在**将分配给驱动器号或文件夹**页上，配置所需的选项，然后选择**下一步**。

6. 在**选择文件系统设置**页上，执行以下，，然后选择**下一步**。
    
    1. 在**文件系统**列表中，选择**NTFS**或**ReFS**。
    
    2. 在**分配单元大小**列表中，**默认**保留的设置或设置分配单元大小。
        
        >[!NOTE]
        >有关分配单元大小的详细信息，请参阅[默认 NTFS、 FAT 和 exFAT 的群集大小](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat)。

    
    3. （可选） 在**卷标签**框中，输入卷标签名称，例如**HR 数据**。

7. 在**确认选择**页上，验证设置正确，，然后选择**创建**。

8. 在**查看结果**页上，验证所有任务已完成，然后选择**关闭**。

9. 若要验证卷已创建，在服务器管理器中，选择**卷**页。 创建它的服务器下面列出了该卷。 你还可以验证该卷是在 Windows 资源管理器。

### Windows PowerShell 的等效命令创建卷

以下 Windows PowerShell cmdlet 的功能与前面的过程。 在单行输入命令。

下面的示例初始化虚拟磁盘*VirtualDisk1*磁盘、 创建分区分配的驱动器号，然后格式具有默认 NTFS 文件系统的卷。

```PowerShell
Get-VirtualDisk –FriendlyName VirtualDisk1 | Get-Disk | Initialize-Disk –Passthru | New-Partition –AssignDriveLetter –UseMaximumSize | Format-Volume
```

## 其他信息

- [存储空间](overview.md)
- [Windows PowerShell 中的存储 Cmdlet](https://docs.microsoft.com/powershell/module/storage/index?view=win10-ps)
- [部署群集的存储空间](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11))
- [存储空间常见问题 (FAQ)](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)
