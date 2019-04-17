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
description: 将使用存储空间直通中 Windows Server 软件定义存储部署为超级集成基础架构或聚合 （也称为分解部署） 基础结构的分步说明。
ms.localizationpriority: medium
ms.openlocfilehash: 55cfa0e066506d7174f9e5b1e61cc0aa290706d7
ms.sourcegitcommit: 65e4f760be73104e67847f77e834e7b5e065211b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "5429039"
---
# 部署存储空间直通

> 适用于： Windows Server 2019、 Windows Server 2016

本主题提供部署[存储空间直通](storage-spaces-direct-overview.md)的分步说明。

> [!Tip]
> 想要获取超级集成基础架构？ Microsoft 建议从我们的合作伙伴这些[Windows Server 软件定义](https://microsoft.com/wssd)的解决方案。 它们设计、 组装和验证对我们的参考体系结构，以确保兼容性和可靠性，以便获取和快速运行。

> [!Tip]
> 你可以使用 HYPER-V 虚拟机，包括 Microsoft Azure 中的[评估存储空间直通，而无需硬件](storage-spaces-direct-in-vm.md)。 你可能还想要查看方便[Windows Server 快速实验室部署脚本](https://aka.ms/wslab)，我们用它来培训目的。

## 开始之前

查看[存储空间直通的硬件要求](Storage-Spaces-Direct-Hardware-Requirements.md)并浏览本文档，以使自己熟悉整体方法和与一些步骤相关的重要说明。

收集的以下信息：

- **部署选项。** 存储空间直通支持[两个部署选项： 超聚合和聚合](storage-spaces-direct-overview.md#deployment-options)，也称为分解部署。 熟悉各自来确定哪种适合你的优点。 步骤 1-3 下面适用于这两个部署选项。 步骤 4 仅用于聚合部署。

- **服务器名称。** 了解你的组织的计算机、 文件、 路径和其他资源的命名策略。 你将需要预配多个服务器，每个都有唯一的名称。

- **域名。** 熟悉的域命名和域加入你的组织的策略。  你将为你的域、 加入服务器，并且你将需要指定域名称。 

- **RDMA 网络。** There are two types of RDMA protocols: iWarp and RoCE. Note which one your network adapters use, and if RoCE, also note the version (v1 or v2). For RoCE, also note the model of your top-of-rack switch.

- **VLAN ID。** 如果有，请注意要用于在服务器上，管理 OS 网络适配器的 VLAN ID。 应该能够从网络管理员处获取此信息。

## 步骤 1：部署 Windows Server

### 步骤 1.1： 安装操作系统

第一步是将在群集中每个服务器上安装 Windows Server。 存储空间直通需要 Windows Server 2016 Datacenter Edition。 具有桌面体验，你可以使用服务器核心安装选项或服务器。

当你安装 Windows Server 使用安装向导时，你可以选择之间 （服务器核心引用） 的*Windows Server*和*Windows Server （带有桌面体验的服务器）*，这是*完整*的安装选项的等效项在 Windows Server 2012 R2 中可用。 如果你未选择，你将获得的服务器核心安装选项。 有关详细信息，请参阅[Windows Server 2016 的安装选项](../../get-started/Windows-Server-2016.md)。

### 步骤 1.2： 连接到服务器

本指南侧重的服务器核心安装选项和一个单独的管理系统，它必须从远程部署/管理：

- 它管理 Windows Server 2016 的服务器相同的更新
- 网络连接到它正在管理的服务器
- 加入到同一个域或完全受信任的域
- Hyper-V 和故障转移群集的远程服务器管理工具 (RSAT) 和 PowerShell 模块。 RSAT 工具和 PowerShell 模块在 Windows Server 上可用，并且可以安装而无需安装其他功能。 你还可以在 Windows 10 管理电脑上安装[远程服务器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)。

在管理系统上安装故障转移群集和 Hyper-V 管理工具。 这可以通过使用“**添加角色和功能**”向导的服务器管理器来完成。 在“**功能**”页上，选择“**远程服务器管理工具**”，然后选择要安装的工具。

输入 PS 会话并使用服务器名称或想要连接到的节点的 IP 地址。 你将提示输入密码时执行此命令后，输入你在设置 Windows 时指定管理员密码。

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   下面是执行相同的操作是在脚本中更有用，以防需要多次执行此操作的方式的示例：

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> 如果你正在从管理系统远程部署，你可能会像错误*WinRM 无法处理请求。* 若要解决此问题，使用 Windows PowerShell 将每个服务器添加到管理计算机上的受信任主机列表：  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> 注意： 受信任的主机列表支持通配符，如`Server*`。
>
> 若要查看受信任主机列表，请键入`Get-Item WSMAN:\Localhost\Client\TrustedHosts`。  
>   
> 若要空列表，键入`Clear-Item WSMAN:\Localhost\Client\TrustedHost`。  

### 步骤 1.3： 加入域并添加域帐户

到目前为止你配置了各个服务器具有本地管理员帐户， `<ComputerName>\Administrator`。

若要管理存储空间直通，你需要将服务器加入到域并使用 Active Directory 域服务域帐户中每个服务器上的管理员组。

从管理系统中，使用管理员权限打开 PowerShell 控制台。 使用`Enter-PSSession`连接到每个服务器，并运行以下 cmdlet，替换你自己的计算机名称、 域名称和域凭据：

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

如果你的存储管理员帐户不是域管理员组的成员，将你的存储管理员帐户添加到每个节点的上的本地管理员组或更好的是，添加用于存储管理员组。 你可以使用以下命令 （或编写一个 Windows PowerShell 函数来执行此操作-的详细信息，请参阅[使用 PowerShell 添加到本地组添加域用户](http://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx)）：

```
Net localgroup Administrators <Domain\Account> /add
```

### 步骤 1.4： 安装角色和功能

下一步是在每个服务器上安装服务器角色。 你可以执行此操作通过使用[Windows Admin Center](../../manage/windows-admin-center/use/manage-servers.md)，[服务器管理器](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)），或 PowerShell。 下面是要安装的角色：

- 故障转移群集
- Hyper-V
- 文件服务器 （如果你想要托管任何文件共享，例如聚合部署）
- Data-Center-Bridging（如果正在使用 RoCEv2，而不是 iWARP 网络适配器）
- RSAT-Clustering-PowerShell
- Hyper-V-PowerShell

若要通过 PowerShell 安装，请使用[安装 WindowsFeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) cmdlet。 你可以使用它在一台服务器所示：

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

若要在同一时间作为群集中的所有服务器上运行此命令，使用此一小段脚本，修改的开头的脚本，以满足你的环境变量列表中。

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"
$FeatureList = "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"

# This part runs the Install-WindowsFeature cmdlet on all servers in $ServerList, passing the list of features into the scriptblock with the "Using" scope modifier so you don't have to hard-code them here.
Invoke-Command ($ServerList) {
    Install-WindowsFeature -Name $Using:Featurelist
}
```

## 步骤 2：配置网络

如果你正在部署存储空间直通虚拟机内跳过此部分。

存储空间直通需要高带宽，低延迟网络之间群集中的服务器。 至少 10 GbE 网络是必需的建议使用远程直接内存访问 (RDMA)。 You can use either iWARP or RoCE as long as it has the Windows Server 2016 logo, but iWARP is usually easier to set up.

> [!Important]
> Depending on your networking equipment, and especially with RoCE v2, some configuration of the top-of-rack switch may be required. 正确的交换机配置是一定要确保存储空间直通的可靠性和性能。

Windows Server 2016 引入了开关嵌入组合 (SET) 中的 HYPER-V 虚拟交换机。 这允许相同的物理 NIC 端口，同时使用 RDMA，减少了所需的物理 NIC 端口用于所有网络通信。 建议为存储空间直通开关嵌入组合。

若要设置网络，对于存储空间直通的说明，请参阅[Windows Server 2016 聚合 NIC 和来宾 RDMA 部署指南](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx)。

## 步骤 3：配置存储空间直通

以下步骤将在一个与配置的服务器版本相同的管理系统上完成。 以下步骤不应使用远程 PowerShell 会话中，运行而改为在具有管理权限的管理系统上运行在本地 PowerShell 会话中。

### 步骤 3.1： 清理驱动器

启用存储空间直通前，请确保你的驱动器为空： 没有旧分区或其他数据。 运行以下脚本，替换你的计算机名，若要删除任何旧的所有分区或其他数据。

> [!Warning]
> 此脚本将永久删除任何以外的操作系统启动驱动器的驱动器上的任何数据 ！

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

输出将如下所示，其中**计数**是每个模型中每个服务器的驱动器的数量：

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

### 步骤 3.2： 验证群集

在此步骤中，你将运行群集验证工具，以确保正确配置服务器节点创建使用存储空间直通群集。 当群集验证 (`Test-Cluster`) 是创建群集之前运行，它会运行测试，验证配置显示适合成功充当故障转移群集。 下面的示例直接使用`-Include`参数，然后测试的特定类别指定。 这样可以确保验证中包含存储空间直通特定的测试。

使用以下 PowerShell 命令验证一组用作存储空间直通群集的服务器。

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### 步骤 3.3： 创建群集

在此步骤中，你将已经验证可用于使用以下 PowerShell cmdlet 在前面步骤中创建的群集的节点创建群集。

在创建群集时，你将收到警告:-"时出现的问题，创建群集的角色可能会阻止它启动。 有关详细信息，请查看以下报告文件。” 可以放心地忽略此警告， 出现此警告是因为群集仲裁上没有可用的磁盘。 建议在创建群集后配置文件共享见证或云见证。

> [!Note]
> 如果服务器正在使用静态 IP 地址，通过添加以下参数和指定 IP 地址来修改以下命令以反映静态 IP 地址：–StaticAddress &lt;X.X.X.X&gt;。
> 在以下命令中，ClusterName 占位符应被唯一的且字符数等于或少于 15 个的 netbios 名称替换。
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

创建群集后，复制群集名称的 DNS 条目可能要花点时间。 时间取决于环境和 DNS 复制配置。 如果解析群集未成功，在大多数情况下，可以使用节点的计算机名称来完成，它是可用于取代群集名称的群集的活动成员。

### 步骤 3.4： 配置群集见证

我们建议你配置为群集，见证，以便有三个或多个服务器的群集中可写入两个服务器发生故障或处于脱机状态。 两台服务器部署需要群集见证，否则进入脱机状态任一服务器将导致另一个也变得不可用。 通过这些系统，可以使用文件共享作为见证或使用云见证。 

有关更多信息，请参见下列主题之一：

- [配置和管理仲裁](../../failover-clustering/manage-cluster-quorum.md)
- [部署故障转移群集的云见证](../../failover-clustering/deploy-cloud-witness.md)

### 步骤 3.5：启用存储空间直通

完成后创建的群集，请使用`Enable-ClusterStorageSpacesDirect`PowerShell cmdlet，它将存储系统置于存储空间直通模式，并自动执行以下操作：

-   **创建池：** 创建具有“S2D on Cluster1”之类的名称的单个大型池。

-   **配置存储空间直通缓存：** 如果存在多个媒体（驱动器）类型可供存储空间直通使用，作为缓存设备可实现最快速度（在大多数情况下读取和写入）

-   **层：** 创建两个层作为默认层。 其中一个称为“容量”，另一个称为“性能”。 cmdlet 通过组合设备类型和复原能力来分析设备并配置每个层。

通过管理系统，在以管理员权限打开的 PowerShell 命令窗口中，启动以下命令。 群集名称是在前面的步骤中创建的群集的名称。 如果在其中一个节点上本地运行此命令，-CimSession 参数则不是必需的。

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

若要使用以上命令启用存储空间直通，可以使用节点名称而不是群集名称。 使用节点名称可能更可靠，因为新创建的群集名称可能出现 DNS 复制延迟。

此命令完成（可能需要几分钟时间）之后，系统将准备好要创建卷。

### 步骤 3.6：创建卷

我们建议使用`New-Volume`cmdlet，因为它可以提供最快且最简单的体验。 此单个 cmdlet 会自动创建虚拟磁盘，对其进行分区和格式化，使用匹配的名称创建卷，并将其添加到群集共享卷 － 这些全都在一个简单的步骤中完成。

有关详细信息，请查看[在存储空间直通中创建卷](create-volumes.md)。

### 步骤 3.7: （可选） 启用 CSV 缓存

你可以选择性地启用群集共享卷 (CSV) 缓存，以用作写入通过块级别的缓存不由 Windows 缓存管理器已缓存的读取操作的系统内存 (RAM)。 这可以改善 HYPER-V 等应用程序的性能。 CSV 缓存上可以提高读取请求的性能和横向扩展文件服务器方案对于也很有用。

启用 CSV 缓存，可以减少内存可用于在超聚合群集上，运行 Vm，因此你必须有平衡存储性能，Vhd 的可用内存量。

若要设置 CSV 缓存的大小，使用存储群集具有管理员权限的帐户打开的管理系统上的 PowerShell 会话，并将此脚本，更改`$ClusterName`和`$CSVCacheSize`作为相应的变量 (此示例设置2 GB CSV 缓存每台服务器）：

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

有关详细信息，请参阅[使用 CSV 内存中读取缓存](csv-cache.md)。

### 步骤 3.8： 部署虚拟机的超聚合部署

如果你正在部署超聚合群集的最后一步是预配存储空间直通群集上的虚拟机。

虚拟机的文件应存储在系统 CSV 命名空间上（示例：c:\\ClusterStorage\\Volume1），就像故障转移群集上的群集 VM。

你可以使用现成工具或其他工具来管理存储和虚拟机，例如系统中心虚拟机管理器中。

## 步骤 4： 部署横向扩展文件服务器的聚合解决方案

如果你正在部署的聚合的解决方案在下一步是创建横向扩展文件服务器实例并设置某些文件共享。 如果你正在部署超聚合群集-你完成后，并且不需要此部分。

### 步骤 4.1： 创建横向扩展文件服务器角色

设置文件服务器的群集服务的下一步创建群集的文件服务器角色，这是当你创建在其托管你的持续可用文件共享的横向扩展文件服务器实例。

#### 若要使用服务器管理器创建横向扩展文件服务器角色

1.  故障转移群集管理器中，选择群集，转到**角色**，，然后单击**配置角色...**。<br>此时将出现高可用性向导。
2.  在**选择角色**页上，单击**文件服务器**。
3.  在**文件服务器类型**页上，单击**应用程序数据的横向扩展文件服务器**。
4.  在**客户端访问点**页上，键入横向扩展文件服务器的名称。
5.  验证角色成功设置通过转到**角色**你创建了确认，**状态**列显示**运行**旁边群集的文件服务器角色，如图 1 中所示。

    ![屏幕截图的故障转移群集管理器显示横向扩展文件服务器](media\Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016\SOFS_in_FCM.png "Failover Cluster Manager showing the Scale-Out File Server")

     **图 1**显示横向扩展文件服务器与运行状态的故障转移群集管理器

> [!NOTE]
>  创建群集的角色后, 可能有一些网络可以阻止你在其上创建的文件共享几分钟，或可能很长的传播延迟。  
  
#### 若要使用 Windows PowerShell 创建横向扩展文件服务器角色

 在 Windows PowerShell 会话中连接到的文件服务器群集，输入以下命令以创建横向扩展文件服务器角色，更改*FSCLUSTER*以匹配你群集的名称和*SOFS*以匹配的名称你希望提供横向扩展文件服务器角色：

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  创建群集的角色后, 可能有一些网络可以阻止你在其上创建的文件共享几分钟，或可能很长的传播延迟。 如果 SOFS 角色立即失败，并且无法启动，它可能是因为群集的计算机对象不具有创建 SOFS 角色的计算机帐户的权限。 有关帮助，请参阅此博客文章：[横向扩展文件服务器角色失败到开始菜单 With 事件 Id 1205，1069 和 1194年](http://www.aidanfinn.com/?p=14142)。

### 步骤 4.2： 创建文件共享

你已创建虚拟磁盘并将其添加到 Csv 后，就可以创建文件共享上的每个虚拟磁盘 CSV 每个文件共享。 系统中心虚拟机管理器 (VMM) 可能是便利的方法来执行此操作，因为它处理的权限，但如果你没有你的环境中，你可以使用 Windows PowerShell 部分自动执行部署。

使用[HYPER-V 工作负载的 SMB 共享配置](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)脚本，部分自动化创建组和共享的过程中包含的脚本。 它面向 HYPER-V 工作负载，因此如果你正在部署其他工作负荷，你可能需要修改的设置或在创建了共享后执行额外步骤。 例如，如果你使用 Microsoft SQL Server，SQL Server 服务帐户必须授予共享和文件系统上的完全控制。

> [!NOTE]
>  你将需要更新组成员身份，当你添加群集节点，除非你使用系统中心虚拟机管理器创建共享。

若要使用 PowerShell 脚本创建的文件共享，请执行以下操作：

1. 下载到一个文件服务器群集的节点的[SMB 共享配置 HYPER-V 工作负载](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)中包含的脚本。
2. 使用域管理员凭据管理系统上打开 Windows PowerShell 会话，然后使用以下脚本创建 HYPER-V 计算机对象，Active Directory 组更改适用于变量的值你环境：

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. 使用一个存储节点上的管理员凭据打开 Windows PowerShell 会话，然后使用以下脚本创建的每个 CSV 共享和用于共享的管理权限授予 Domain Admins 组和计算群集。

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

### 步骤 4.3 启用 Kerberos 约束委派

要设置 Kerberos 约束委派远程方案管理和增强的实时迁移安全性，从一个存储群集节点，使用[SMB 共享配置为 HYPER-V 工作负载](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)中包含的 KCDSetup.ps1 脚本。 下面是一些脚本的包装器：

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## 后续步骤

部署后你群集的文件服务器，我们建议测试你的解决方案使用之前调任何真实的工作负荷的综合工作负载的性能。 这允许你确认解决方案正确执行，并添加工作负载的复杂性之前计算出任何遗留的问题。 有关详细信息，请参阅[测试存储空间性能使用综合工作负载](https://technet.microsoft.com/library/dn894707.aspx)。

## 另请参阅

-   [Windows Server 2016 中的存储空间直通](storage-spaces-direct-overview.md)
-   [了解存储空间直通中的缓存](understand-the-cache.md)
-   [规划存储空间直通中的卷](plan-volumes.md)
-   [存储空间容错](storage-spaces-fault-tolerance.md)
-   [存储空间直通的硬件要求](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [要不要连接到 RDMA，这才是问题所在](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/)（TechNet 博客）
