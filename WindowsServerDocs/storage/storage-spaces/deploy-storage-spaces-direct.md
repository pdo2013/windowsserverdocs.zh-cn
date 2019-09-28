---
title: 部署存储空间直通
ms.prod: windows-server
manager: eldenc
ms.author: stevenek
ms.technology: storage-spaces
ms.topic: get-started-article
ms.assetid: 20fee213-8ba5-4cd3-87a6-e77359e82bc0
author: stevenek
ms.date: 06/07/2019
description: 逐步说明使用 Windows Server 中的存储空间直通将软件定义的存储部署为超聚合基础结构或聚合（也称为非聚合）基础结构。
ms.localizationpriority: medium
ms.openlocfilehash: 0ab96f737f7700e202c9d0382c06859c4ea84118
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402816"
---
# <a name="deploy-storage-spaces-direct"></a>部署存储空间直通

> 适用于：Windows Server 2019、Windows Server 2016

本主题提供部署[存储空间直通](storage-spaces-direct-overview.md)的分步说明。

> [!Tip]
> 想要获取超聚合基础结构？ Microsoft 建议从我们的合作伙伴购买经验证的硬件/软件解决方案，其中包括部署工具和过程。 针对我们的参考体系结构设计、汇编和验证这些解决方案，以确保兼容性和可靠性，使你能够快速启动和运行。 对于 Windows Server 2019 解决方案，请访问[AZURE STACK HCI 解决方案网站](https://azure.microsoft.com/overview/azure-stack/hci)。 对于 Windows Server 2016 解决方案，请参阅[Windows Server 软件定义](https://microsoft.com/wssd)的详细信息。

> [!Tip]
> 你可以使用 Hyper-v 虚拟机，包括在 Microsoft Azure 中，以[评估没有硬件存储空间直通](storage-spaces-direct-in-vm.md)。 你可能还想要查看方便使用的[Windows Server 快速实验室部署脚本](https://aka.ms/wslab)，这些脚本用于定型目的。

## <a name="before-you-start"></a>开始之前

查看[存储空间直通硬件要求](Storage-Spaces-Direct-Hardware-Requirements.md)，并浏览本文档以熟悉与某些步骤相关的总体方法和重要说明。

收集以下信息：

- **部署选项。** 存储空间直通支持[两个部署选项：超聚合和聚合](storage-spaces-direct-overview.md#deployment-options)（也称为非聚合）。 熟悉每个的优点，决定哪种方式适合你。 下面的步骤1-3 适用于这两个部署选项。 步骤4仅用于聚合部署。

- **服务器名称。** 熟悉组织的计算机、文件、路径和其他资源的命名策略。 需要预配多台服务器，每个服务器都具有唯一的名称。

- **域名。** 熟悉组织用于域命名和加入域的策略。  将服务器加入到域中，并且需要指定域名。 

- **RDMA 网络。** RDMA 协议分为两种类型： iWarp 和 RoCE。 请注意，网络适配器使用哪一个，如果 RoCE，还请注意版本（v1 或 v2）。 对于 RoCE，还请注意机箱顶部交换机的型号。

- **VLAN ID。** 请注意要用于服务器上的管理 OS 网络适配器的 VLAN ID （如果有）。 应该能够从网络管理员处获取此信息。

## <a name="step-1-deploy-windows-server"></a>第 1 步：部署 Windows Server

### <a name="step-11-install-the-operating-system"></a>步骤1.1：安装操作系统。

第一步是在将位于群集中的每个服务器上安装 Windows Server。 存储空间直通需要 Windows Server 2016 Datacenter Edition。 你可以使用服务器核心安装选项或具有桌面体验的服务器。

当你使用安装向导安装 Windows Server 时，你可以在*Windows server* （引用 server Core）和*windows Server （具有桌面体验的服务器）* 之间进行选择，这等同于*完全*安装选项在 Windows Server 2012 R2 中可用。 如果不选择，则会获得服务器核心安装选项。 有关详细信息，请参阅[Windows Server 2016 安装选项](../../get-started/Windows-Server-2016.md)。

### <a name="step-12-connect-to-the-servers"></a>步骤1.2：连接到服务器

本指南重点介绍 "服务器核心" 安装选项，以及如何从单独的管理系统远程进行远程管理，这必须：

- Windows Server 2016 与它所管理的服务器具有相同的更新
- 与管理的服务器的网络连接
- 加入到同一个域或完全受信任的域
- Hyper-V 和故障转移群集的远程服务器管理工具 (RSAT) 和 PowerShell 模块。 RSAT 工具和 PowerShell 模块在 Windows Server 上可用，无需安装其他功能即可进行安装。 你还可以在 Windows 10 管理 PC 上安装[远程服务器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)。

在管理系统上安装故障转移群集和 Hyper-V 管理工具。 这可以通过使用“**添加角色和功能**”向导的服务器管理器来完成。 在“**功能**”页上，选择“**远程服务器管理工具**”，然后选择要安装的工具。

输入 PS 会话并使用服务器名称或想要连接到的节点的 IP 地址。 执行此命令后，系统会提示输入密码，请输入设置 Windows 时指定的管理员密码。

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   下面的示例使用在脚本中更有用的方式执行相同的操作，以防需要多次执行此操作：

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> 如果是从管理系统远程部署，则可能会收到错误，如*WinRM 无法处理该请求。* 若要解决此问题，请使用 Windows PowerShell 将每个服务器添加到管理计算机上的 "受信任的主机" 列表中：  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> 注意：受信任的主机列表支持通配符，如 `Server*`。
>
> 若要查看受信任的主机列表，请键入 `Get-Item WSMAN:\Localhost\Client\TrustedHosts`。  
>   
> 若要清空该列表，请键入 `Clear-Item WSMAN:\Localhost\Client\TrustedHost`。  

### <a name="step-13-join-the-domain-and-add-domain-accounts"></a>步骤1.3：加入域并添加域帐户

到目前为止，你已配置了具有本地管理员帐户的各个服务器，`<ComputerName>\Administrator`。

若要管理存储空间直通，需要将服务器加入域，并使用每台服务器上管理员组中的 Active Directory 域服务域帐户。

在管理系统中，使用管理员权限打开 PowerShell 控制台。 使用 @no__t 来连接到每台服务器并运行以下 cmdlet，同时替换自己的计算机名、域名和域凭据：

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

如果你的存储管理员帐户不是 Domain Admins 组的成员，请将你的存储管理员帐户添加到每个节点上的本地管理员组，或者，添加你的存储管理员组。 你可以使用以下命令（或编写 Windows PowerShell 函数来执行此操作-请参阅[使用 PowerShell 将域用户添加到本地组](http://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx)，了解详细信息）：

```
Net localgroup Administrators <Domain\Account> /add
```

### <a name="step-14-install-roles-and-features"></a>步骤1.4：安装角色和功能

下一步是在每个服务器上安装服务器角色。 可以通过使用[Windows 管理中心](../../manage/windows-admin-center/use/manage-servers.md)、[服务器管理器](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)）或 PowerShell 来执行此操作。 下面是要安装的角色：

- 故障转移群集
- Hyper-V
- 文件服务器（如果想要托管任何文件共享，例如聚合部署）
- Data-Center-Bridging（如果正在使用 RoCEv2，而不是 iWARP 网络适配器）
- RSAT-Clustering-PowerShell
- Hyper-V-PowerShell

若要通过 PowerShell 安装，请使用[add-windowsfeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) cmdlet。 可以在单个服务器上使用它，如下所示：

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

若要在群集中的所有服务器上同时运行此命令，请使用这一小段脚本，修改脚本开头的变量列表，以适合您的环境。

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

如果要在虚拟机中部署存储空间直通，请跳过此部分。

存储空间直通要求群集中的服务器之间具有高带宽、低延迟的网络连接。 至少需要10个 GbE 网络，建议使用远程直接内存访问（RDMA）。 你可以使用 iWARP 或 RoCE，前提是它具有 Windows Server 2016 徽标，但 iWARP 通常更易于设置。

> [!Important]
> 根据你的网络设备，尤其是对于 RoCE v2，可能需要一些顶层交换机的配置。 正确的交换机配置对于确保存储空间直通的可靠性和性能非常重要。

Windows Server 2016 引入了 Hyper-v 虚拟交换机中的交换机嵌入组合（SET）。 这样，在使用 RDMA 时，可以将相同的物理 NIC 端口用于所有网络流量，从而减少所需的物理 NIC 端口的数量。 建议存储空间直通使用交换机嵌入组合。

交换或 switchless 节点互连
- 进必须正确配置网络交换机以处理带宽和网络类型。 如果使用的 RDMA 实现了 RoCE 协议，则网络设备和交换机配置更为重要。
- Switchless:节点可以使用直接连接互连，避免使用交换机。 每个节点都必须与群集中的每个其他节点具有直接连接。

有关为存储空间直通设置网络的说明，请参阅[Windows Server 2016 汇聚 NIC 和来宾 RDMA 部署指南](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx)。

## <a name="step-3-configure-storage-spaces-direct"></a>步骤 3:配置存储空间直通

以下步骤将在一个与配置的服务器版本相同的管理系统上完成。 以下步骤不应使用 PowerShell 会话远程运行，而是使用管理权限在管理系统上的本地 PowerShell 会话中运行。

### <a name="step-31-clean-drives"></a>步骤3.1：清洗驱动器

启用存储空间直通之前，请确保驱动器为空：没有旧分区或其他数据。 运行以下脚本来替换计算机名称，以删除所有旧分区或其他数据。

> [!Warning]
> 此脚本将永久删除操作系统启动驱动器以外的任何驱动器上的所有数据！

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

输出将如下所示，其中**Count**是每个服务器中每个模型的驱动器数量：

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

### <a name="step-32-validate-the-cluster"></a>步骤3.2：验证群集

在此步骤中，你将运行群集验证工具，以确保正确配置服务器节点，以便使用存储空间直通创建群集。 如果在创建群集之前运行群集验证（`Test-Cluster`），则它会运行测试，验证配置是否显示为适用于成功充当故障转移群集。 下面的示例使用 @no__t 参数，然后指定特定类别的测试。 这样可以确保验证中包含存储空间直通特定的测试。

使用以下 PowerShell 命令验证一组用作存储空间直通群集的服务器。

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### <a name="step-33-create-the-cluster"></a>步骤3.3：创建群集

在此步骤中，你将创建一个群集，其中包含你在前面步骤中使用以下 PowerShell cmdlet 验证群集创建的节点。

创建群集时，你将收到一条警告： "创建群集角色时出现问题，可能会阻止其启动。 有关详细信息，请查看以下报告文件。” 可以放心地忽略此警告。 出现此警告是因为群集仲裁上没有可用的磁盘。 建议在创建群集后配置文件共享见证或云见证。

> [!Note]
> 如果服务器正在使用静态 IP 地址，通过添加以下参数和指定 IP 地址来修改以下命令以反映静态 IP 地址：–StaticAddress &lt;X.X.X.X&gt;。
> 在以下命令中，ClusterName 占位符应被唯一的且字符数等于或少于 15 个的 netbios 名称替换。
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

创建群集后，复制群集名称的 DNS 条目可能要花点时间。 时间取决于环境和 DNS 复制配置。 如果解析群集未成功，在大多数情况下，可以使用节点的计算机名称来完成，它是可用于取代群集名称的群集的活动成员。

### <a name="step-34-configure-a-cluster-witness"></a>步骤3.4：配置分类见证

建议为群集配置见证服务器，使具有三个或更多服务器的群集可以承受两台服务器发生故障或处于脱机状态。 双服务器部署需要分类见证，否则服务器脱机会导致另一个服务器无法使用。 通过这些系统，可以使用文件共享作为见证或使用云见证。 

有关更多信息，请参见下列主题之一：

- [配置和管理仲裁](../../failover-clustering/manage-cluster-quorum.md)
- [为故障转移群集部署云见证](../../failover-clustering/deploy-cloud-witness.md)

### <a name="step-35-enable-storage-spaces-direct"></a>步骤3.5：启用存储空间直通

创建群集后，请使用 `Enable-ClusterStorageSpacesDirect` PowerShell cmdlet，该 cmdlet 会将存储系统置于存储空间直通模式，并自动执行以下操作：

-   **创建池：** 创建名为 "S2D on Cluster1" 的单个大型池。

-   **配置存储空间直通缓存：** 如果有多个媒体（驱动器）类型可用于存储空间直通使用，它将启用最快的缓存设备（在大多数情况下读取和写入）

-   **底层**创建两个层作为默认层。 其中一个称为“容量”，另一个称为“性能”。 cmdlet 通过组合设备类型和复原能力来分析设备并配置每个层。

通过管理系统，在以管理员权限打开的 PowerShell 命令窗口中，启动以下命令。 群集名称是在前面的步骤中创建的群集的名称。 如果在其中一个节点上本地运行此命令，-CimSession 参数则不是必需的。

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

若要使用以上命令启用存储空间直通，可以使用节点名称而不是群集名称。 使用节点名称可能更可靠，因为新创建的群集名称可能出现 DNS 复制延迟。

此命令完成（可能需要几分钟时间）之后，系统将准备好要创建卷。

### <a name="step-36-create-volumes"></a>步骤3.6：创建卷

建议使用 `New-Volume` cmdlet，因为它提供最快、最直接的体验。 此单个 cmdlet 会自动创建虚拟磁盘，对其进行分区和格式化，使用匹配的名称创建卷，并将其添加到群集共享卷 － 这些全都在一个简单的步骤中完成。

有关详细信息，请查看[在存储空间直通中创建卷](create-volumes.md)。

### <a name="step-37-optionally-enable-the-csv-cache"></a>步骤3.7：（可选）启用 CSV 缓存

你可以选择启用群集共享卷（CSV）缓存，以将系统内存（RAM）用作 Windows 缓存管理器尚未缓存的读取操作的写操作块级别缓存。 这可以提高 Hyper-v 的应用程序的性能。 CSV 缓存可以提高读取请求的性能，也可用于横向扩展文件服务器情况。

启用 CSV 缓存可减少可用于在超聚合群集上运行 Vm 的内存量，因此，你必须使用 Vhd 可用的内存来平衡存储性能。

若要设置 CSV 缓存的大小，请使用对存储群集具有管理员权限的帐户打开管理系统上的 PowerShell 会话，然后使用此脚本根据需要更改 `$ClusterName` 和 `$CSVCacheSize` 变量（此示例将设置为2每台服务器 GB CSV 缓存）：

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

有关详细信息，请参阅[使用 CSV 内存中读取缓存](csv-cache.md)。

### <a name="step-38-deploy-virtual-machines-for-hyper-converged-deployments"></a>步骤3.8：部署用于超聚合部署的虚拟机

如果要部署超聚合群集，最后一步是在存储空间直通群集上预配虚拟机。

虚拟机的文件应存储在系统 CSV 命名空间（例如： c： \\ClusterStorage @ no__t-1Volume1）上，就像故障转移群集上的群集 Vm 一样。

你可以使用内置工具或其他工具来管理存储和虚拟机，如 System Center Virtual Machine Manager。

## <a name="step-4-deploy-scale-out-file-server-for-converged-solutions"></a>步骤 4：部署聚合解决方案的横向扩展文件服务器

如果要部署聚合解决方案，下一步就是创建横向扩展文件服务器实例，并设置一些文件共享。 如果要部署超聚合群集，则已完成，不需要此部分。

### <a name="step-41-create-the-scale-out-file-server-role"></a>步骤4.1：创建横向扩展文件服务器角色

为文件服务器设置群集服务的下一步是创建群集文件服务器角色，这是在创建持续可用文件共享的横向扩展文件服务器实例时。

#### <a name="to-create-a-scale-out-file-server-role-by-using-server-manager"></a>使用服务器管理器创建横向扩展文件服务器角色

1. 在故障转移群集管理器中，选择群集，中转到 "**角色**"，然后单击 "**配置角色 ...** "。<br>此时将显示 "高可用性向导"。
2. 在 "**选择角色**" 页上，单击 "**文件服务器**"。
3. 在 "**文件服务器类型**" 页上，单击 "**应用程序数据横向扩展文件服务器**"。
4. 在 "**客户端访问点**" 页上，键入横向扩展文件服务器的名称。
5. 验证角色是否已成功设置，方法是转到 "**角色**"，确认 "状态"**列显示 "** **状态**" 列显示在创建的群集文件服务器角色旁边，如图1所示。

   显示(media/Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016/SOFS_in_FCM.png "横向扩展文件服务器")的![横向扩展文件服务器故障转移群集管理器故障转移群集管理器屏幕截图]

    **图 1**显示状态为 "正在运行" 的横向扩展文件服务器故障转移群集管理器

> [!NOTE]
>  创建群集角色后，可能会出现一些网络传播延迟，这可能会导致在几分钟内创建文件共享或可能更长时间。  
  
#### <a name="to-create-a-scale-out-file-server-role-by-using-windows-powershell"></a>使用 Windows PowerShell 创建横向扩展文件服务器角色

 在连接到文件服务器群集的 Windows PowerShell 会话中，输入以下命令以创建横向扩展文件服务器角色，将*FSCLUSTER*更改为与群集名称相匹配，并将*SOFS*更改为与你要为横向扩展文件服务器角色：

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  创建群集角色后，可能会出现一些网络传播延迟，这可能会导致在几分钟内创建文件共享或可能更长时间。 如果 SOFS 角色立即失败且不会启动，则可能是因为群集的计算机对象没有创建 SOFS 角色的计算机帐户的权限。 有关此操作的帮助，请参阅此博客文章：[横向扩展文件服务器角色无法启动，事件 Id 为1205、1069和 1194](http://www.aidanfinn.com/?p=14142)。

### <a name="step-42-create-file-shares"></a>步骤4.2：创建文件共享

创建虚拟磁盘并将其添加到 Csv 后，可以在这些磁盘上创建文件共享-每个虚拟磁盘每个 CSV 有一个文件共享。 System Center Virtual Machine Manager （VMM）可能是执行此操作的 handiest 方法，因为它处理你的权限，但如果你的环境中没有此权限，则可以使用 Windows PowerShell 部分自动完成部署。

使用[适用于 Hyper-v 工作负荷的 SMB 共享配置](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)脚本中包含的脚本，该脚本部分自动执行创建组和共享的过程。 它是针对 Hyper-v 工作负荷编写的，因此，如果要部署其他工作负载，则你可能需要在创建共享后修改设置或执行其他步骤。 例如，如果你使用 Microsoft SQL Server，则必须向 SQL Server 服务帐户授予对共享和文件系统的完全控制权限。

> [!NOTE]
>  添加群集节点时必须更新组成员身份，除非使用 System Center Virtual Machine Manager 创建共享。

若要使用 PowerShell 脚本创建文件共享，请执行以下操作：

1. 将[Hyper-v 工作负荷的 SMB 共享配置](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)中包含的脚本下载到文件服务器群集的一个节点。
2. 在管理系统上使用域管理员凭据打开 Windows PowerShell 会话，然后使用以下脚本为 Hyper-v 计算机对象创建 Active Directory 组，并根据需要更改变量的值环境

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. 在其中一个存储节点上使用管理员凭据打开 Windows PowerShell 会话，然后使用以下脚本创建每个 CSV 的共享，并将共享的管理权限授予 Domain Admins 组和计算群集。

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

### <a name="step-43-enable-kerberos-constrained-delegation"></a>步骤4.3 启用 Kerberos 约束委派

若要为远程方案管理设置 Kerberos 约束委派并增加实时迁移安全性，请在一个存储群集节点上，使用[适用于 Hyper-v 工作负荷的 SMB 共享配置](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)中包含的 KCDSetup 脚本。 下面是该脚本的小包装：

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## <a name="next-steps"></a>后续步骤

部署群集文件服务器后，我们建议在引入任何实际工作负荷之前，使用综合工作负荷测试解决方案的性能。 这样，你就可以在添加工作负荷复杂性之前，确认解决方案是否正在正常运行，并解决所有延迟问题。 有关详细信息，请参阅[使用综合工作负荷测试存储空间性能](https://technet.microsoft.com/library/dn894707.aspx)。

## <a name="see-also"></a>请参阅

-   [Windows Server 2016 中的存储空间直通](storage-spaces-direct-overview.md)
-   [了解存储空间直通中的缓存](understand-the-cache.md)
-   [规划存储空间直通中的卷](plan-volumes.md)
-   [存储空间容错](storage-spaces-fault-tolerance.md)
-   [存储空间直通硬件要求](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [要不要连接到 RDMA，这才是问题所在](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/)（TechNet 博客）
