---
title: 部署存储空间直通
ms.prod: windows-server-threshold
manager: eldenc
ms.author: stevenek
ms.technology: storage-spaces
ms.topic: get-started-article
ms.assetid: 20fee213-8ba5-4cd3-87a6-e77359e82bc0
author: stevenek
ms.date: 8/16/2018
description: 将部署使用存储空间直通在 Windows Server 中软件定义存储为超聚合基础结构或聚合 （也称为非聚合） 的基础结构的分步说明。
ms.localizationpriority: medium
ms.openlocfilehash: 55cfa0e066506d7174f9e5b1e61cc0aa290706d7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865408"
---
# <a name="deploy-storage-spaces-direct"></a>部署存储空间直通

> 适用于：Windows Server 2019、Windows Server 2016

本主题提供了分步说明部署[存储空间直通](storage-spaces-direct-overview.md)。

> [!Tip]
> 为希望获得 Hyper-Converged 基础结构？ Microsoft 建议这些[Windows Server 软件定义](https://microsoft.com/wssd)从我们的合作伙伴解决方案。 它们是设计、 组装和根据我们的参考体系结构，以确保兼容性和可靠性，因此你和快速运行验证。

> [!Tip]
> 可以使用 HYPER-V 虚拟机，包括在 Microsoft Azure 中，以便[无需任何硬件评估存储空间直通](storage-spaces-direct-in-vm.md)。 您可能还想要查看便利[Windows Server 快速实验室部署脚本](https://aka.ms/wslab)，我们用于培训目的。

## <a name="before-you-start"></a>开始之前

审阅[存储空间直通的硬件要求](Storage-Spaces-Direct-Hardware-Requirements.md)通篇此文档来熟悉总体方法以及与一些步骤相关的重要说明。

收集以下信息：

- **部署选项。** 存储空间直通支持[两个部署选项： 超聚合和聚合](storage-spaces-direct-overview.md#deployment-options)，也称为非聚合。 熟悉各自来决定哪个最适合你的优点。 步骤 1-3 以下适用于这两个部署选项。 步骤 4 是仅部署所需的聚合。

- **服务器名称。** 熟悉你的组织命名策略的计算机、 文件、 路径和其他资源。 你将需要预配多台服务器，每个都有唯一的名称。

- **域的名称。** 熟悉有关域命名和域加入的组织的策略。  您将会将服务器加入到你的域，并将需要指定域的名称。 

- **RDMA 网络。** 有两种类型的 RDMA 协议： iWarp 和 RoCE。 请注意使用哪一个网络适配器，并且如果 RoCE，还请注意 （v1 或 v2） 的版本。 为 RoCE，另请注意机架顶部交换机的模型。

- **VLAN ID。** 如果有，请注意要用于管理操作系统的服务器上的网络适配器的 VLAN ID。 应该能够从网络管理员处获取此信息。

## <a name="step-1-deploy-windows-server"></a>第 1 步：部署 Windows Server

### <a name="step-11-install-the-operating-system"></a>步骤 1.1:安装操作系统。

第一步是将位于群集中每个服务器上安装 Windows Server。 存储空间直通需要 Windows Server 2016 Datacenter Edition。 带有桌面体验，可以使用服务器核心安装选项或服务器。

在安装 Windows Server 使用安装向导，您可以选择*Windows Server* （指服务器核心） 和*Windows Server （带有桌面体验的服务器）*，这相当*完整*安装选项在 Windows Server 2012 R2 中可用。 如果您没有作出选择，将获取的服务器核心安装选项。 有关详细信息，请参阅[Windows Server 2016 的安装选项](../../get-started/Windows-Server-2016.md)。

### <a name="step-12-connect-to-the-servers"></a>步骤 1.2:连接到服务器

本指南主要服务器核心安装选项和单独的管理系统，其必须具有从远程部署/管理：

- Windows Server 2016 的更新与它所管理的服务器
- 网络连接到它管理的服务器
- 已加入到同一个域或完全受信任的域
- Hyper-V 和故障转移群集的远程服务器管理工具 (RSAT) 和 PowerShell 模块。 RSAT 工具和 PowerShell 模块是可在 Windows Server 上，可以安装而无需安装其他功能。 此外可以安装[远程服务器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)Windows 10 管理 PC 上。

在管理系统上安装故障转移群集和 Hyper-V 管理工具。 这可以通过使用“**添加角色和功能**”向导的服务器管理器来完成。 在“**功能**”页上，选择“**远程服务器管理工具**”，然后选择要安装的工具。

输入 PS 会话并使用服务器名称或想要连接到的节点的 IP 地址。 您会提示输入密码后执行此命令时，输入设置 Windows 时指定的管理员密码。

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   下面是执行同一操作中，这在脚本中，更有用，以防需要多次执行此操作的方法的示例：

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> 如果你要从管理系统远程部署，可能会收到如下错误*WinRM 无法处理该请求。* 若要解决此问题，请使用 Windows PowerShell 将每个服务器添加到你管理的计算机上的受信任主机列表：  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> 注意： 受信任的主机列表支持通配符，如`Server*`。
>
> 若要查看受信任主机列表，请键入`Get-Item WSMAN:\Localhost\Client\TrustedHosts`。  
>   
> 若要清空该列表，请键入`Clear-Item WSMAN:\Localhost\Client\TrustedHost`。  

### <a name="step-13-join-the-domain-and-add-domain-accounts"></a>步骤 1.3:加入域并将添加域帐户

到目前为止已使用本地管理员帐户配置单个服务器`<ComputerName>\Administrator`。

若要管理存储空间直通，将需要将服务器加入到域并使用每个服务器上的管理员组中的 Active Directory 域服务域帐户。

从管理系统中，使用管理员权限打开 PowerShell 控制台。 使用`Enter-PSSession`连接到每台服务器并运行以下 cmdlet，从而替换你自己的计算机名称、 域名和域凭据：

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

如果你的存储管理员帐户不是 Domain Admins 组的成员，将你的存储管理员帐户添加到每个节点的上的本地管理员组或更好的做法，添加用于存储管理员的组。 可以使用以下命令 (或编写一个 Windows PowerShell 函数来执行此操作-请参阅[使用 PowerShell 将域用户添加到本地组](http://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx)的详细信息):

```
Net localgroup Administrators <Domain\Account> /add
```

### <a name="step-14-install-roles-and-features"></a>步骤 1.4:安装角色和功能

下一步是在每个服务器上安装服务器角色。 您可以执行此操作通过使用[Windows Admin Center](../../manage/windows-admin-center/use/manage-servers.md)，[服务器管理器](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md))，或 PowerShell。 下面是要安装的角色：

- 故障转移群集
- Hyper-V
- 文件服务器 （如果您想要托管任何文件共享，例如，针对聚合部署）
- Data-Center-Bridging（如果正在使用 RoCEv2，而不是 iWARP 网络适配器）
- RSAT-Clustering-PowerShell
- Hyper-V-PowerShell

若要通过 PowerShell 安装，请使用[Install-windowsfeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) cmdlet。 可以如下一台服务器上使用它：

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

若要为同一时间在群集中的所有服务器上运行命令，使用此小段脚本，修改脚本，以满足您的环境的开头的变量的列表。

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"
$FeatureList = "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"

# This part runs the Install-WindowsFeature cmdlet on all servers in $ServerList, passing the list of features into the scriptblock with the "Using" scope modifier so you don't have to hard-code them here.
Invoke-Command ($ServerList) {
    Install-WindowsFeature -Name $Using:Featurelist
}
```

## <a name="step-2-configure-the-network"></a>步骤 2：配置网络

如果要部署存储空间直通中的虚拟机，跳过此部分。

存储空间直通需要高带宽、 低延迟网络在群集中的服务器之间。 至少为 10 GbE 网络是必需的建议远程直接内存访问 (RDMA)。 只要具有 Windows Server 2016 徽标，但 iWARP 通常易于设置，可以使用 iWARP 或 RoCE。

> [!Important]
> 具体取决于自己的网络设备，尤其是在使用 RoCE v2，可能需要的机架顶部交换机的某些配置。 务必确保可靠性和性能的存储空间直通是正确的交换机配置。

Windows Server 2016 引入了切换嵌入组合 (SET) 中的 HYPER-V 虚拟交换机。 这样，相同的物理 NIC 端口，以使用 RDMA，减少所需的物理 NIC 端口同时用于所有网络流量。 切换嵌入组合建议的存储空间直通。

若要设置的存储空间直通的网络的说明，请参阅[Windows Server 2016 聚合 NIC 和来宾 RDMA 部署指南](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx)。

## <a name="step-3-configure-storage-spaces-direct"></a>步骤 3:配置存储空间直通

以下步骤将在一个与配置的服务器版本相同的管理系统上完成。 以下步骤应不是运行使用远程 PowerShell 会话中，但改为在本地 PowerShell 会话中运行具有管理权限的管理系统上。

### <a name="step-31-clean-drives"></a>步骤 3.1:清洗驱动器

启用存储空间直通之前，请确保你的驱动器为空： 没有旧分区或其他数据。 运行以下脚本，替换您的计算机名称，若要删除任何旧的分区或其他数据。

> [!Warning]
> 此脚本将永久删除操作系统启动驱动器以外的任何驱动器上的任何数据 ！

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"

Invoke-Command ($ServerList) {
    Update-StorageProviderCache
    Get-StoragePool | ? IsPrimordial -eq $false | Set-StoragePool -IsReadOnly:$false -ErrorAction SilentlyContinue
    Get-StoragePool | ? IsPrimordial -eq $false | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$false -ErrorAction SilentlyContinue
    Get-StoragePool | ? IsPrimordial -eq $false | Remove-StoragePool -Confirm:$false -ErrorAction SilentlyContinue
    Get-PhysicalDisk | Reset-PhysicalDisk -ErrorAction SilentlyContinue
    Get-Disk | ? Number -ne $null | ? IsBoot -ne $true | ? IsSystem -ne $true | ? PartitionStyle -ne RAW | % {
        $_ | Set-Disk -isoffline:$false
        $_ | Set-Disk -isreadonly:$false
        $_ | Clear-Disk -RemoveData -RemoveOEM -Confirm:$false
        $_ | Set-Disk -isreadonly:$true
        $_ | Set-Disk -isoffline:$true
    }
    Get-Disk | Where Number -Ne $Null | Where IsBoot -Ne $True | Where IsSystem -Ne $True | Where PartitionStyle -Eq RAW | Group -NoElement -Property FriendlyName
} | Sort -Property PsComputerName, Count
```

输出将如下所示，其中**计数**是每个模型中的每个服务器的驱动器数：

```
Count Name                          PSComputerName
----- ----                          --------------
4     ATA SSDSC2BA800G4n            Server01
10    ATA ST4000NM0033              Server01
4     ATA SSDSC2BA800G4n            Server02
10    ATA ST4000NM0033              Server02
4     ATA SSDSC2BA800G4n            Server03
10    ATA ST4000NM0033              Server03
4     ATA SSDSC2BA800G4n            Server04
10    ATA ST4000NM0033              Server04
```

### <a name="step-32-validate-the-cluster"></a>步骤 3.2:验证群集

在此步骤中，你将运行群集验证工具，以确保正确配置服务器节点来创建使用存储空间直通的群集。 当群集验证 (`Test-Cluster`) 是运行创建群集之前，请运行测试，验证配置显示适合成功充当故障转移群集。 以下示例直接使用`-Include`指定参数，然后测试的特定类别。 这样可以确保验证中包含存储空间直通特定的测试。

使用以下 PowerShell 命令验证一组用作存储空间直通群集的服务器。

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### <a name="step-33-create-the-cluster"></a>步骤 3.3:创建群集

在此步骤中，将已经验证可用于在使用以下 PowerShell cmdlet 在前面步骤中创建群集的节点创建群集。

创建群集时，将看到警告，指出-"时出现的问题创建群集的角色可能会阻止其启动。 有关详细信息，请查看以下报告文件。” 可以放心地忽略此警告。 出现此警告是因为群集仲裁上没有可用的磁盘。 建议在创建群集后配置文件共享见证或云见证。

> [!Note]
> 如果服务器正在使用静态 IP 地址，通过添加以下参数和指定 IP 地址来修改以下命令以反映静态 IP 地址：–StaticAddress &lt;X.X.X.X&gt;。
> 在以下命令中，ClusterName 占位符应被唯一的且字符数等于或少于 15 个的 netbios 名称替换。
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

创建群集后，复制群集名称的 DNS 条目可能要花点时间。 时间取决于环境和 DNS 复制配置。 如果解析群集未成功，在大多数情况下，可以使用节点的计算机名称来完成，它是可用于取代群集名称的群集的活动成员。

### <a name="step-34-configure-a-cluster-witness"></a>步骤 3.4:配置群集见证

我们建议您配置在群集的见证服务器，以便具有三个或多个服务器群集可以承受两台服务器发生故障或处于脱机状态。 双服务器部署需要群集见证，否则进入脱机状态的任一服务器会导致另一个也变得不可用。 通过这些系统，可以使用文件共享作为见证或使用云见证。 

有关更多信息，请参见下列主题之一：

- [配置和管理仲裁](../../failover-clustering/manage-cluster-quorum.md)
- [部署故障转移群集的云见证](../../failover-clustering/deploy-cloud-witness.md)

### <a name="step-35-enable-storage-spaces-direct"></a>步骤 3.5:启用存储空间直通

创建群集后，使用`Enable-ClusterStorageSpacesDirect`PowerShell cmdlet，后者将存储系统置于存储空间直通模式和自动执行以下操作：

-   **创建池：** 创建具有一个名称，如"S2D on Cluster1"的单个大型池。

-   **配置存储空间直通缓存：** 如果有多个媒体 （驱动器） 类型可供存储空间直通使用，使最快作为缓存设备 （读取和写入在大多数情况下）

-   **层：** 作为默认层中创建两个层。 其中一个称为“容量”，另一个称为“性能”。 cmdlet 通过组合设备类型和复原能力来分析设备并配置每个层。

通过管理系统，在以管理员权限打开的 PowerShell 命令窗口中，启动以下命令。 群集名称是在前面的步骤中创建的群集的名称。 如果在其中一个节点上本地运行此命令，-CimSession 参数则不是必需的。

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

若要使用以上命令启用存储空间直通，可以使用节点名称而不是群集名称。 使用节点名称可能更可靠，因为新创建的群集名称可能出现 DNS 复制延迟。

此命令完成（可能需要几分钟时间）之后，系统将准备好要创建卷。

### <a name="step-36-create-volumes"></a>步骤 3.6:创建卷

我们建议使用`New-Volume`cmdlet 为其提供的最快、 最直观的体验。 此单个 cmdlet 会自动创建虚拟磁盘，对其进行分区和格式化，使用匹配的名称创建卷，并将其添加到群集共享卷 － 这些全都在一个简单的步骤中完成。

有关详细信息，请查看[在存储空间直通中创建卷](create-volumes.md)。

### <a name="step-37-optionally-enable-the-csv-cache"></a>步骤 3.7:启用 CSV 缓存 （可选）

您可以选择启用群集共享卷 (CSV) 缓存使用系统内存 (RAM) 作为直写块级别的缓存不由 Windows 缓存管理器已缓存的读取操作。 这可以提高应用程序，如 HYPER-V 的性能。 CSV 缓存可以提高读取请求的性能，也适用于横向扩展文件服务器方案。

启用 CSV 缓存可以减少可用于超聚合群集上运行的 Vm，因此您必须平衡使用 Vhd 的可用内存的存储性能的内存量。

若要设置的 CSV 缓存的大小，存储群集具有管理员权限的帐户打开管理系统上的 PowerShell 会话以及如何将此脚本中，更改`$ClusterName`和`$CSVCacheSize`变量根据需要记录 （这示例设置每个服务器的 2GB CSV 缓存）：

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

有关详细信息，请参阅[使用中内存 CSV 读取缓存](csv-cache.md)。

### <a name="step-38-deploy-virtual-machines-for-hyper-converged-deployments"></a>步骤 3.8:部署超聚合部署的虚拟机

如果你正在部署超聚合群集，最后一步是预配存储空间直通群集上的虚拟机。

虚拟机的文件应存储在系统 CSV 命名空间 (示例： c:\\ClusterStorage\\Volume1) 上的故障转移群集的群集的 Vm 一样。

可以使用现成工具或其他工具来管理存储和虚拟机，如 System Center Virtual Machine Manager。

## <a name="step-4-deploy-scale-out-file-server-for-converged-solutions"></a>步骤 4：聚合解决方案部署横向扩展文件服务器

如果你正在部署聚合的解决方案下, 一步是创建横向扩展文件服务器实例并设置一些文件共享。 如果你正在部署超聚合群集-完成后，不需要此部分。

### <a name="step-41-create-the-scale-out-file-server-role"></a>步骤 4.1:创建横向扩展文件服务器角色

设置你的文件服务器群集服务的下一步创建群集的文件服务器角色，这是创建持续可用文件共享托管的横向扩展文件服务器实例时。

#### <a name="to-create-a-scale-out-file-server-role-by-using-server-manager"></a>若要使用服务器管理器中创建横向扩展文件服务器角色

1.  在故障转移群集管理器中，选择的群集，请转到**角色**，然后单击**配置角色...**.<br>高可用性向导会显示。
2.  上**选择角色**页上，单击**文件服务器**。
3.  上**文件服务器类型**页上，单击**应用程序数据的横向扩展文件服务器**。
4.  上**客户端访问点**页上，键入在横向扩展文件服务器的名称。
5.  验证该角色已成功地设置了通过转到**角色**并确认**状态**列将显示**运行**旁边你创建的群集的文件服务器角色如图 1 中所示。

    ![屏幕截图的故障转移群集管理器显示横向扩展文件服务器](media\Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016\SOFS_in_FCM.png "显示横向扩展文件服务器故障转移群集管理器")

     **图 1**显示横向扩展文件服务器的运行状态的故障转移群集管理器

> [!NOTE]
>  创建群集的角色后，可能有一些网络传播延迟可能会阻止您从其上创建文件共享，在几分钟内，或可能更长时间。  
  
#### <a name="to-create-a-scale-out-file-server-role-by-using-windows-powershell"></a>若要使用 Windows PowerShell 创建横向扩展文件服务器角色

 在 Windows PowerShell 会话连接到文件服务器群集中，输入以下命令以创建横向扩展文件服务器角色，请更改*FSCLUSTER*以匹配你的群集的名称和*SOFS*以匹配你想要提供横向扩展文件服务器角色的名称：

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  创建群集的角色后，可能有一些网络传播延迟可能会阻止您从其上创建文件共享，在几分钟内，或可能更长时间。 如果 SOFS 角色将立即失败，不会启动，则可能是因为群集的计算机对象不具有创建 SOFS 角色的计算机帐户的权限。 有关帮助，请参阅这篇博客文章：[横向扩展文件服务器角色无法启动具有事件 Id 1205、 1069 和 1194年](http://www.aidanfinn.com/?p=14142)。

### <a name="step-42-create-file-shares"></a>步骤 4.2:创建文件共享

已创建虚拟磁盘并将其添加到 Csv 后，就可以创建文件共享上的每个 CSV 每个虚拟磁盘的一个文件共享。 System Center Virtual Machine Manager (VMM) 可能是这样做，因为它可处理权限，但如果你尚未在环境中，可以使用 Windows PowerShell 部分自动部署的便利方式。

使用中包含的脚本[SMB 共享的 HYPER-V 工作负荷配置](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)脚本中，部分可以创建组和共享的过程自动执行。 写入以供 HYPER-V 工作负荷，因此如果你正在部署的其他工作负荷，可能需要修改设置，或在创建共享后执行其他步骤。 例如，如果您使用 Microsoft SQL Server，必须将 SQL Server 服务帐户授予对共享和文件系统的完全控制。

> [!NOTE]
>  必须将更新的组成员身份，除非你使用 System Center Virtual Machine Manager 创建您的共享资源添加群集节点时。

若要使用 PowerShell 脚本创建文件共享，请执行以下操作：

1. 下载中包含的脚本[SMB 共享的 HYPER-V 工作负荷配置](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)到文件服务器群集的节点之一。
2. 使用在管理系统上，域管理员凭据打开 Windows PowerShell 会话，然后使用以下脚本创建的 HYPER-V 计算机对象，Active Directory 组更改为适合于变量的值在环境：

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. 使用一个存储节点上的管理员凭据打开 Windows PowerShell 会话，然后使用以下脚本来为每个 CSV 创建共享和共享的管理权限授予 Domain Admins 组和计算群集。

    ```PowerShell
    # Replace the values of these variables
    $StorageClusterName = "StorageSpacesDirect1"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $SOFSName = "SOFS"
    $SharePrefix = "Share"
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of the script itself
    CD $ScriptFolder
    Get-ClusterSharedVolume -Cluster $StorageClusterName | ForEach-Object
    {
        $ShareName = $SharePrefix + $_.SharedVolumeInfo.friendlyvolumename.trimstart("C:\ClusterStorage\Volume")
        Write-host "Creating share $ShareName on "$_.name "on Volume: " $_.SharedVolumeInfo.friendlyvolumename
        .\FileShareSetup.ps1 -HyperVClusterName $StorageClusterName -CSVVolumeNumber $_.SharedVolumeInfo.friendlyvolumename.trimstart("C:\ClusterStorage\Volume") -ScaleOutFSName $SOFSName -ShareName $ShareName -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName
    }
    ```

### <a name="step-43-enable-kerberos-constrained-delegation"></a>步骤 4.3 启用 Kerberos 约束委派

若要设置 Kerberos 约束委派，对于远程方案中管理和提高实时迁移安全性，从一个存储群集节点，使用中包含的 KCDSetup.ps1 脚本[SMB 的 HYPER-V 工作负荷共享配置](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a). 下面是脚本的小包装器：

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## <a name="next-steps"></a>后续步骤

在部署后群集的文件服务器，建议测试解决方案使用综合工作负荷，然后再使任何实际工作负载的性能。 这可让你确认该解决方案正常以及添加的工作负荷的复杂性之前解决任何延迟问题。 有关详细信息，请参阅[测试存储空间性能使用综合工作负荷](https://technet.microsoft.com/library/dn894707.aspx)。

## <a name="see-also"></a>请参阅

-   [Windows Server 2016 中的存储空间直通](storage-spaces-direct-overview.md)
-   [了解存储空间直通中的缓存](understand-the-cache.md)
-   [存储空间直通中规划卷](plan-volumes.md)
-   [存储空间容错能力](storage-spaces-fault-tolerance.md)
-   [存储空间直通的硬件要求](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [要不要连接到 RDMA，这才是问题所在](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/)（TechNet 博客）
