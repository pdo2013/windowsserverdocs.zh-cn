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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868268"
---
# <a name="deploy-storage-spaces-on-a-stand-alone-server"></a>在独立服务器上部署存储空间

>适用于：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

本主题介绍如何在独立服务器上部署存储空间。 有关如何创建群集的存储空间的信息，请参阅[部署 Windows Server 2012 R2 上的存储空间群集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>)。

若要创建存储空间，必须首先创建一个或多个存储池。 存储池是物理磁盘的集合。 存储池允许存储聚合、弹性容量扩展以及委派的管理。

从存储池中，你可以创建一个或多个虚拟磁盘。 这些虚拟磁盘也称为*存储空间*。 存储空间在 Windows 操作系统中将显示为一个常规磁盘，你可以从中创建格式化的卷。 在你通过文件和存储服务的用户界面创建虚拟磁盘时，你可以配置复原类型（简单、镜像或奇偶校验）、设置类型（精简或固定）以及大小。 通过 Windows PowerShell，你可以设置附加参数，如列数、交错值，以及要使用池中的哪个物理磁盘。 有关这些附加参数的信息，请参阅“存储空间常见问题 (FAQ)”中的 [New-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/new-virtualdisk?view=win10-ps) 和[什么是列以及存储空间如何决定使用多少列？](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx%23what_are_columns_and_how_does_storage_spaces_decide_how_many_to_use)。

>[!NOTE]
>不能使用存储空间来托管 Windows 操作系统。

你可以从虚拟磁盘创建一个或多个卷。 在创建卷时，可以配置大小、 驱动器号或文件夹、 文件系统 （NTFS 文件系统或复原文件系统 (ReFS)），分配单元大小和可选的卷标。

下图说明了存储空间工作流。

![存储空间工作流](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**图 1：存储空间工作流**

>[!NOTE]
>此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅[PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6)。

## <a name="prerequisites"></a>先决条件

若要在独立的 Windows Server 2012−based 服务器上使用存储空间，请确保你想要使用的物理磁盘满足以下先决条件。

> [!IMPORTANT]
> 如果你想要了解如何在故障转移群集上部署存储空间，请参阅[部署 Windows Server 2012 R2 上的存储空间群集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>)。 故障转移群集部署具有不同的先决条件，如受支持的磁盘总线类型、 受支持的复原类型和所需的最小数量的磁盘。

|区域|要求|说明|
|---|---|---|
|磁盘总线类型|串行连接 SCSI (SAS)<br>串行高级技术附件 (SATA)<br>-iSCSI 和光纤通道控制器。 |你还可以使用 USB 驱动器。 但是，并非最佳选择要在服务器环境中使用 USB 驱动器。<br>存储空间支持 iSCSI 和光纤通道 (FC) 控制器上，只要在它们上面创建的虚拟磁盘的非可复原 （简单与任意数量的列）。<br>|
|磁盘配置|的必须至少 4 GB 物理磁盘。<br>磁盘必须为空，并且未格式化。 不要创建卷。||
|HBA 注意事项|-简单的主机总线适配器 (Hba) 不支持 RAID 功能的建议<br>-如果支持 RAID Hba 必须为非 RAID 模式与禁用所有 RAID 功能<br>-适配器不得提取物理磁盘，缓存数据或遮盖任何连接的设备。 这包括通过附加的简单磁盘捆绑 (JBOD) 设备所提供的存储设备服务。 |存储空间仅与可以完全禁用所有 RAID 功能的 HBA 兼容。|
|JBOD 存储设备|-是可选的 jbod 存储设备<br>-建议使用存储空间认证的 Windows Server 目录上列出的机箱<br>-如果你使用 jbod 存储设备，验证向存储供应商存储设备支持存储空间，以确保完整的功能<br>-若要确定该 jbod 存储设备是否支持存储模块和插槽标识，请运行以下 Windows PowerShell cmdlet:<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>如果**EnclosureNumber**并**SlotNumber**字段包含的值，然后存储设备支持这些功能。||

若要计划物理磁盘数和独立服务器部署所需的复原类型，请使用以下指南。

|复原类型|磁盘要求|何时使用|
|---|---|---|
|**Simple**<br><br>的跨物理磁盘条带化数据<br>-最大化磁盘容量并提高吞吐量<br>-无复原能力 （不能防止磁盘故障）<br><br><br><br><br><br><br>|需要至少一个物理磁盘。|不要用于托管不可替代的数据。 简单的空间不能防止磁盘故障。<br><br>以较低的成本托管临时的或容易重新创建的数据。<br><br>适用于高性能工作负荷复原能力不是必需的或已提供的应用程序的位置。|
|**Mirror**<br><br>-将数据的两个或三个副本存储到一组物理磁盘<br>-提高可靠性，但减少容量。 每次写入时出现重复。 镜像空间还在多个物理驱动器上条带化数据。<br>-更大的数据吞吐量和访问延迟低于奇偶校验<br>-使用脏区域跟踪 (DRT) 来跟踪池中的磁盘修改。 当系统从计划外的关机恢复并且空间重新联机时，DRT 使磁盘中的池彼此保持一致。|需要至少两个物理磁盘，以防止单个磁盘故障。<br><br>需要至少五个物理磁盘，以防止两个磁盘同时发生故障。|在大多数部署中使用。 例如，镜像空间适合一般用途的文件共享或虚拟硬盘 (VHD) 库。|
|**奇偶校验**<br><br>-条带化数据和奇偶校验信息跨物理磁盘<br>-提高可靠性，相比简单空间，但一定程度上降低了容量<br>-通过日志记录增加复原能力。 这有助于防止在发生计划外关机时损坏数据。|要求至少三个物理磁盘，以防止单个磁盘故障。|用于具有高度序列化的工作负载，如存档或备份。|

## <a name="step-1-create-a-storage-pool"></a>第 1 步：创建存储池

你必须首先将可用的物理磁盘分组到一个或多个存储池中。

1. 在服务器管理器导航窗格中，选择**文件和存储服务**。

2. 在导航窗格中，选择**存储池**页。
    
    默认情况下，可用磁盘包括在一个名为*原始*池的池中。 如果在“存储池”下没有列出原始池，这表示存储不符合存储空间的要求。 确保磁盘满足“先决条件”部分所述的要求。
    
    >[!TIP]
    >如果选择“原始”存储池，则可用的物理磁盘在“物理磁盘”下列出。

3. 下**存储池**，选择**任务**列表，并选择**新的存储池**。 新建存储池向导将打开。

4. 上**在开始之前**页上，选择**下一步**。

5. 上**指定存储池名称和子系统**页上，输入名称和存储池的可选描述，选择你想要使用，然后选择的可用物理磁盘的组**下一步**。

6. 上**选择存储池的物理磁盘**页上，执行以下操作，并选择**下一步**:
    
    1. 选中你想要包括在存储池中的每个物理磁盘旁边的复选框。
    
    2. 如果你想要在将一个或多个磁盘指定为热备用**分配**，选择下拉箭头，然后选择**热备用**。

7. 上**确认选择**页上，验证设置是否正确，然后选择**创建**。

8. 上**查看结果**页上，验证所有任务完成，然后选择**关闭**。
    
    >[!NOTE]
    >或者，若要直接继续下一步，可以选中“此向导关闭时创建虚拟磁盘”复选框。

9. 在“存储池”下，验证是否列出了新的存储池。

### <a name="windows-powershell-equivalent-commands-for-creating-storage-pools"></a>创建存储池的 Windows PowerShell 等效命令

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

下面的示例说明哪些物理磁盘在原始池中可用。

```PowerShell
Get-StoragePool -IsPrimordial $true | Get-PhysicalDisk | Where-Object CanPool -eq $True
```

下面的示例创建名为新的存储池*StoragePool1* ，它使用所有可用的磁盘。

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk –CanPool $True)
```

下面的示例创建一个新的存储池*StoragePool1*，使用四个可用的磁盘。

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk PhysicalDisk1, PhysicalDisk2, PhysicalDisk3, PhysicalDisk4)
```

下面的 cmdlet 序列示例说明如何将一个可用的物理磁盘 *PhysicalDisk5* 作为热备用添加到存储池 *StoragePool1*。

```PowerShell
$PDToAdd = Get-PhysicalDisk –FriendlyName PhysicalDisk5
Add-PhysicalDisk –StoragePoolFriendlyName StoragePool1 –PhysicalDisks $PDToAdd –Usage HotSpare
```

## <a name="step-2-create-a-virtual-disk"></a>步骤 2：创建虚拟磁盘

接下来，你必须从存储池中创建一个或多个虚拟磁盘。 在创建虚拟磁盘时，你可以选择在不同的物理磁盘上如何布局数据。 这会影响可靠性和性能。 你还可以选择是创建精简还是固定的设置磁盘。

1. 如果“新建虚拟磁盘”向导尚未打开，则在服务器管理器中的“存储池”页面上的“存储池”下，确保已选中所需的存储池。

2. 下**虚拟磁盘**，选择**任务**列表，并选择**新的虚拟磁盘**。 将打开新的虚拟磁盘向导。

3. 上**在开始之前**页上，选择**下一步**。

4. 上**选择存储池**页上，选择所需的存储池，并选择**下一步**。

5. 上**指定虚拟磁盘名称**页上，输入名称和可选描述，然后选择**下一步**。

6. 上**选择存储布局**页上，选择所需的布局，然后选择**下一步**。
    
    >[!NOTE]
    >如果选择没有足够的物理磁盘布局，在选择时，将收到一条错误消息**下一步**。 有关使用和磁盘要求的布局信息，请参阅[先决条件](#prerequisites))。

7. 如果所选**镜像**存储布局，还有五个或多个磁盘在池中，如**配置复原设置**页将出现。 选择以下选项之一：
    
      - “双向镜像”
      - “三向镜像”

8. 上**指定预配类型**页上，选择以下选项之一，然后选择**下一步**。
    
      - “精简”
        
        若使用精简设置，将根据需要分配空间。 这将优化可用存储空间的使用情况。 但是，由于这样使你能够过量分配存储，因此必须认真监视有多少可用磁盘空间量。
    
      - “固定”
        
        若使用固定的设置，则在创建虚拟磁盘时，将立即分配存储容量。 因此，固定的设置从存储池中使用等于虚拟磁盘大小的空间。
    
    >[!TIP]
    >使用存储空间时，你可以在相同的存储池中同时创建精简和固定设置的虚拟磁盘。 例如，你可以使用精简设置的虚拟磁盘来托管一个数据库，以及使用固定设置虚拟磁盘来托管相关联的日志文件。

9. 在“指定虚拟磁盘的大小”页面上，执行以下操作：
    
    如果你选择了精简设置在上一步骤中，在**虚拟磁盘大小**框中，输入虚拟磁盘大小，选择单位 (**MB**， **GB**，或**TB**)，然后选择**下一步**。
    
    如果你选择了固定设置上一步中，选择以下项之一：
    
      - “指定大小”
        
        若要指定大小，请输入中的值**虚拟磁盘大小**，然后选择单位 (**MB**， **GB**，或者**TB**)。
        
        如果你使用一个简单布局以外的存储布局，则该虚拟磁盘使用比指定大小更多的可用空间。 若要避免可能出现的错误（其中卷的大小超过存储池的可用空间），可以选中“创建可能的最大虚拟磁盘，直到达到指定的大小”复选框。
    
      - “最大大小”
        
        选择此选项可以创建使用存储池的最大容量的虚拟磁盘。

10. 上**确认选择**页上，验证设置是否正确，然后选择**创建**。

11. 上**查看结果**页上，验证所有任务完成，然后选择**关闭**。
    
    >[!TIP]
    >默认情况下，“此向导关闭时创建卷”复选框处于选中状态。 这使你可以直接进入下一步。

### <a name="windows-powershell-equivalent-commands-for-creating-virtual-disks"></a>用于创建虚拟磁盘的 Windows PowerShell 等效命令

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

下面的示例创建名为的 50 GB 虚拟磁盘*VirtualDisk1*存储池上名为*StoragePool1*。

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

下面的示例创建名为的镜像虚拟磁盘*VirtualDisk1*存储池上名为*StoragePool1*。 该磁盘使用存储池的最大存储容量。

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –ResiliencySettingName Mirror –UseMaximumSize
```

下面的示例创建名为的 50 GB 虚拟磁盘*VirtualDisk1*名为的存储池上*StoragePool1*。 该磁盘使用精简设置类型。

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB) –ProvisioningType Thin
```

下面的示例创建名为的虚拟磁盘*VirtualDisk1*存储池上名为*StoragePool1*。 虚拟磁盘使用三向镜像，并且大小固定为 20 GB。

>[!NOTE]
>为了使此 cmdlet 起作用，在该存储池中你必须至少有五个物理磁盘。 （这不包括任何分配为热备用的磁盘。）

```PowerShell
New-VirtualDisk -StoragePoolFriendlyName StoragePool1 -FriendlyName VirtualDisk1 -ResiliencySettingName Mirror -NumberOfDataCopies 3 -Size 20GB -ProvisioningType Fixed
```

## <a name="step-3-create-a-volume"></a>步骤 3:创建卷

接下来，你必须从虚拟磁盘创建卷。 你可以分配可选的驱动器号或文件夹，然后使用文件系统格式化该卷。

1. 如果尚未打开，请在新建卷向导**存储池**页面上在服务器管理器中，在**的虚拟磁盘**，右键单击所需的虚拟磁盘，并选择**新卷**.
    
    此时将打开“新建卷”向导。

2. 上**在开始之前**页上，选择**下一步**。

3. 上**选择的服务器和磁盘**页上，执行以下操作，并选择**下一步**。
    
    1. 在中**Server**区域中，选择你想要预配卷的服务器。
    
    2. 在中**磁盘**区域中，选择你想要创建卷的虚拟磁盘。

4. 上**指定卷的大小**页上，输入卷的大小，指定的单位 (**MB**， **GB**，或者**TB**)，然后选择**下一步**。

5. 上**分配到驱动器号或文件夹**页上，配置所需的选项，并选择**下一步**。

6. 上**选择文件系统设置**页上，执行以下操作，并选择**下一步**。
    
    1. 在中**文件系统**列表中，选择**NTFS**或**ReFS**。
    
    2. 在“分配单元大小”列表中，将设置保留为“默认值”或设置分配单位大小。
        
        >[!NOTE]
        >有关分配单元大小的详细信息，请参阅 [NTFS、FAT 和 exFAT 的默认群集大小](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat)。

    
    3. （可选） 在**卷标**框中，输入卷标名称，例如**HR 数据**。

7. 上**确认选择**页上，验证设置是否正确，然后选择**创建**。

8. 上**查看结果**页上，验证所有任务完成，然后选择**关闭**。

9. 若要验证是否已创建卷，在服务器管理器中，选择**卷**页。 在创建卷的服务器下列出该卷。 你还可以验证该卷是否在 Windows 资源管理器中。

### <a name="windows-powershell-equivalent-commands-for-creating-volumes"></a>Windows PowerShell 等效命令用于创建卷

以下 Windows PowerShell cmdlet 执行与前面的过程相同的功能。 请在单一行中输入命令。

下面的示例将磁盘初始化为虚拟磁盘 *VirtualDisk1*、使用分配的驱动器号创建分区，然后使用默认 NTFS 文件系统格式化卷。

```PowerShell
Get-VirtualDisk –FriendlyName VirtualDisk1 | Get-Disk | Initialize-Disk –Passthru | New-Partition –AssignDriveLetter –UseMaximumSize | Format-Volume
```

## <a name="additional-information"></a>其他信息

- [存储空间](overview.md)
- [Windows PowerShell 中的存储 Cmdlet](https://docs.microsoft.com/powershell/module/storage/index?view=win10-ps)
- [部署群集的存储空间](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11))
- [存储空间常见问题 (FAQ)](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)
