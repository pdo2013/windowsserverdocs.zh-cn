---
title: 存储副本的已知问题
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 06/25/2019
ms.assetid: ceddb0fa-e800-42b6-b4c6-c06eb1d4bc55
ms.openlocfilehash: 681e07b85af603d11295bf1ca2a08f0eb7181725
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865249"
---
# <a name="known-issues-with-storage-replica"></a>存储副本的已知问题

>适用于：Windows Server 2019、Windows Server 2016、Windows Server（半年频道）

本主题讨论 Windows Server 中存储副本的已知问题。

## <a name="after-removing-replication-disks-are-offline-and-you-cannot-configure-replication-again"></a>删除复制后，磁盘处于脱机状态，且不能再次配置复制

在 Windows Server 2016 中，可能无法在之前已复制的卷或可能发现无法装入的卷上预配复制。 当某些错误条件阻止删除复制或在之前复制数据的计算机上重新安装操作系统时，可能会出现此问题。  

若要修复此问题，必须使用 `Clear-SRMetadata` cmdlet 将隐藏的存储副本分区从磁盘清除，并将其返回到可写入状态。  

-   若要删除所有孤立的存储副本分区数据库槽并重新安装所有分区，请使用 `-AllPartitions` 参数，如下所示：  

    ```PowerShell
    Clear-SRMetadata -AllPartitions  
    ```  

-   若要删除所有孤立的存储副本日志数据，请使用 `-AllLogs` 参数，如下所示：  

    ```PowerShell
    Clear-SRMetadata -AllLogs  
    ```  

-   若要删除所有孤立的故障转移群集配置数据，请使用 `-AllConfiguration` 参数，如下所示：  

    ```PowerShell
    Clear-SRMetadata -AllConfiguration  
    ```  

-   若要删除单个复制组元数据，请使用 `-Name` 参数并指定一个复制组，如下所示：  

    ```PowerShell
    Clear-SRMetadata -Name RG01 -Logs -Partition  
    ```  

在清理分区数据库后，服务器可能需要重新启动；可以使用 `-NoRestart` 暂时取消此操作，但如果 cmdlet 请求，则不应跳过重新启动服务器。 此 cmdlet 既不会删除数据卷，也不会删除这些卷内包含的数据。  

## <a name="during-initial-sync-see-event-log-4004-warnings"></a>在初始同步阶段，请查看事件日志 4004 警告

在 Windows Server 2016 中，配置复制时，源和目标服务器可能在初始同步阶段均显示多个 **StorageReplica\Admin** 事件日志 4004 警告，状态为“完成 API 的系统资源不足”。 你也很可能会看到 5014 错误。 这些错误表示服务器没有足够的可用内存 (RAM) 来执行初始同步和运行工作负荷。 从功能和应用程序（而不是从存储副本）添加 RAM 或减少使用的 RAM。  

## <a name="when-using-guest-clusters-with-shared-vhdx-and-a-host-without-a-csv-virtual-machines-stop-responding-after-configuring-replication"></a>当使用具有共享 VHDX 的来宾群集和没有 CSV 的主机时，虚拟机将在配置复制后停止响应

在 Windows Server 2016 中，当使用 Hyper-V 来宾群集用于存储副本测试或演示目的并使用共享 VHDX 作为来宾群集存储时，虚拟机将在配置复制后停止响应。 重新启动 Hyper-V 主机后，虚拟机开始响应，但复制配置不会完成，也不会出现复制。  

使用 **fltmc.exe attach svhdxflt** 绕过运行 CSV 的 Hyper-V 主机的要求时，会出现此行为。 不支持使用此命令，且仅用于测试和演示目的。  

导致缓慢的原因是 Windows Server 2016 中的存储 QoS 功能和手动附加的共享 VHDX 筛选器之间的按设计互操作性问题。 若要解决此问题，禁用存储 QoS 筛选器驱动程序并重新启动 Hyper-V 主机：  

```  
SC config storqosflt start= disabled  
```  

## <a name="cannot-configure-replication-when-using-new-volume-and-differing-storage"></a>在使用新卷和不同的存储时无法配置复制

在源和目标服务器上将 `New-Volume` cmdlet 与不同的存储集一起使用时（如两个不同的 SAN 或两个具有不同磁盘的 JBOD），可能随后无法使用 `New-SRPartnership` 配置复制。 可能显示的错误包括：  

    Data partition sizes are different in those two groups  

使用 `New-Partition**` cmdlet 创建卷并将其格式化来替代 `New-Volume`，因为后一个 cmdlet 可能会在不同的存储阵列上轮循卷的大小。 如果已创建了 NTFS 卷，则可使用 `Resize-Partition` 以增大或收缩其中的一个卷以匹配另一个卷（ReFS 卷不能执行此操作）。 如果使用 **Diskmgmt** 或**服务器管理器**，则不会进行轮循。  

## <a name="running-test-srtopology-fails-with-name-related-errors"></a>运行 Test-SRTopology 失败，出现与名称相关的错误

尝试使用 `Test-SRTopology` 时，收到以下错误之一：  

**错误示例1：**

    WARNING: Invalid value entered for target computer name: sr-srv03. Test-SrTopology cmdlet does not accept IP address as  
    input for target computer name parameter. NetBIOS names and fully qualified domain names are acceptable inputs  
    WARNING: System.Exception  
    WARNING:    at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.BeginProcessing()  
    Test-SRTopology : Invalid value entered for target computer name: sr-srv03. Test-SrTopology cmdlet does not accept IP  
    address as input for target computer name parameter. NetBIOS names and fully qualified domain names are acceptable  
    inputs  
    At line:1 char:1  
    + Test-SRTopology -SourceComputerName sr-srv01 -SourceVolumeName d: -So ...  
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~  
        + CategoryInfo          : InvalidArgument: (:) [Test-SRTopology], Exception  
        + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand  

**错误示例2：**

    WARNING: Invalid value entered for source computer name

**错误示例3：**

    The specified volume cannot be found G: cannot be found on computer SRCLUSTERNODE1

此 cmdlet 在 Windows Server 2016 中具有有限的错误报告，并将对很多常见问题返回同一输出。 由于以下原因，可能出现错误：  

* 作为本地用户（而非域用户）登录到源计算机。  
* 目标计算机未运行或无法通过网络访问。  
* 为目标计算机指定了不正确的名称。  
* 为目标服务器指定了 IP 地址。  
* 目标计算机防火墙阻止对 PowerShell 和/或 CIM 调用的访问。  
* 目标计算机未运行 WMI 服务。   
* 从管理计算机远程运行 `Test-SRTopology` cmdlet 时未使用 CREDSSP。
* 指定的源或目标卷是群集节点上的本地磁盘，而非群集磁盘。  

## <a name="configuring-new-storage-replica-partnership-returns-an-error---failed-to-provision-partition"></a>配置新的存储副本合作关系返回错误 -“设置分配失败”

尝试使用 `New-SRPartnership` 创建新的复制合作关系时，收到以下错误：

    New-SRPartnership : Unable to create replication group test01, detailed reason: Failed to provision partition ed0dc93f-107c-4ab4-a785-afd687d3e734.
    At line: 1 char: 1
    + New-SRPartnership -SourceComputerName srv1 -SourceRGName test01
    + Categorylnfo : ObjectNotFound: (MSFT_WvrAdminTasks : root/ Microsoft/. . s) CNew-SRPartnership], CimException
    + FullyQua1ifiedErrorId : Windows System Error 1168 ,New-SRPartnership

这是由在同一分区上选择数据卷作为系统驱动器（即 **C:** 驱动器及其 Windows 文件夹）导致的。 例如，在驱动器上包含从同一分区创建的 **C:** 和 **D:** 卷。 在存储副本中不支持此功能；你必须选择不同的卷用于复制。

## <a name="attempting-to-grow-a-replicated-volume-fails-due-to-missing-update"></a>尝试增大复制卷因缺少更新而失败

尝试增长或扩展已复制卷时，收到以下错误：

    PS C:\> Resize-Partition -DriveLetter d -Size 44GB
    Resize-Partition : The operation failed with return code 8
    At line:1 char:1
    + Resize-Partition -DriveLetter d -Size 44GB
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (StorageWMI:ROOT/Microsoft/.../MSFT_Partition
    [Resize-Partition], CimException
    + FullyQualifiedErrorId : StorageWMI 8,Resize-Partition

如果使用磁盘管理 MMC 管理单元，将收到此错误：

    Element not found

即使你使用 `Set-SRGroup -Name rg01 -AllowVolumeResize $TRUE` 在源服务器上正确地启用了调整卷大小，仍将发生此问题。 

此问题已在 Windows 10 版本1607（周年更新）和 Windows Server 2016 的累积更新中解决：2016年12月9日（KB3201845）。 

## <a name="attempting-to-grow-a-replicated-volume-fails-due-to-missing-step"></a>尝试增大复制卷因缺少步骤而失败

如果尝试在源服务器上调整复制卷的大小，但未首先设置 `-AllowResizeVolume $TRUE`，则会收到以下错误：

    PS C:\> Resize-Partition -DriveLetter I -Size 8GB
    Resize-Partition : Failed

    Activity ID: {87aebbd6-4f47-4621-8aa4-5328dfa6c3be}
    At line:1 char:1
    + Resize-Partition -DriveLetter I -Size 8GB
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (StorageWMI:ROOT/Microsoft/.../MSFT_Partition) [Resize-Partition], CimException
        + FullyQualifiedErrorId : StorageWMI 4,Resize-Partition

    Storage Replica Event log error 10307:

    Attempted to resize a partition that is protected by Storage Replica .

    DeviceName: \Device\Harddisk1\DR1
    PartitionNumber: 7
    PartitionId: {b71a79ca-0efe-4f9a-a2b9-3ed4084a1822}

    Guidance: To grow a source data partition, set the policy on the replication group containing the data partition.

    Set-SRGroup -ComputerName [ComputerName] -Name [ReplicationGroupName] -AllowVolumeResize $true

    Before you grow the source data partition, ensure that the destination data partition has enough space to grow to an equal size. Shrinking of data partition protected by Storage Replica is blocked.

磁盘管理管理单元错误： 

    An unexpected error has occurred 

调整卷大小后，请记住使用 `Set-SRGroup -Name rg01 -AllowVolumeResize $FALSE` 禁用调整大小。 此参数可阻止管理员在确保目标卷上有足够空间（通常是因为他们不知道存在存储副本）之前尝试调整卷大小。 

## <a name="attempting-to-move-a-pdr-resource-between-sites-on-an-asynchronous-stretch-cluster-fails"></a>尝试在异步拉伸群集上的站点间移动 PDR 资源失败

尝试移动附加了物理磁盘资源的角色（如常规用途的文件服务器），以在异步拉伸群集中移动关联存储时，收到了一个错误。

如果使用故障转移群集管理器管理单元：

    Error
    The operation has failed.
    The action 'Move' did not complete.
    Error Code: 0x80071398
    The operation failed because either the specified cluster node is not the owner of the group, or the node is not a possible owner of the group

如果使用群集 Powershell cmdlet：

    PS C:\> Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
    Move-ClusterGroup : An error occurred while moving the clustered role 'sr-fs-006'.
    The operation failed because either the specified cluster node is not the owner of the group, or the node is not a
    possible owner of the group
    At line:1 char:1
    + Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Move-ClusterGroup], ClusterCmdletException
    + FullyQualifiedErrorId : Move-ClusterGroup,Microsoft.FailoverClusters.PowerShell.MoveClusterGroupCommand

这归因于 Windows Server 2016 中的按设计行为。 使用 `Set-SRPartnership` 在异步拉伸群集中移动这些 PDR 磁盘。  

此行为已在 Windows Server 版本1709中进行了更改，以允许根据客户反馈进行手动和自动故障转移。

## <a name="attempting-to-add-disks-to-a-two-node-asymmetric-cluster-returns-no-disks-suitable-for-cluster-disks-found"></a>尝试将磁盘添加到两个节点的非对称群集时，返回了“没有发现适用于群集磁盘的磁盘”

在添加存储副本拉伸复制之前，尝试预配只有两个节点的群集时，尝试将第二个站点中的磁盘添加到可用磁盘。 收到以下错误：

    "No disks suitable for cluster disks found. For diagnostic information about disks available to the cluster, use the Validate a Configuration Wizard to run Storage tests." 

如果群集中至少有三个节点，就不会发生这种情况。 由于 Windows Server 2016 中非对称存储群集行为的按设计代码更改，于是发生了此问题。 

若要添加存储，可以在第二个站点中的节点上运行以下命令：

`Get-ClusterAvailableDisk -All | Add-ClusterDisk`

这不适用于节点本地存储。 你可以使用存储副本在两个总节点之间复制拉伸群集，**每个群集都使用其自己的共享存储集**。 

## <a name="the-smb-bandwidth-limiter-fails-to-throttle-storage-replica-bandwidth"></a>SMB 带宽限制无法限制存储副本带宽

指定存储副本的带宽限制时，会忽略限制并使用完整带宽。 例如：

`Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond 32MB`

出现此问题是由于存储副本和 SMB 之间的互操作性问题。 此问题是在 Windows Server 2016 的2017年7月累积更新和 Windows Server 1709 版本中首先修复的。

## <a name="event-1241-warning-repeated-during-initial-sync"></a>在初始同步期间重复事件 1241 警告

指定复制合作关系为异步时，源计算机会在存储副本管理通道中重复记录警告事件 1241。 例如：

    Log Name:      Microsoft-Windows-StorageReplica/Admin
    Source:        Microsoft-Windows-StorageReplica
    Date:          3/21/2017 3:10:41 PM
    Event ID:      1241
    Task Category: (1)
    Level:         Warning
    Keywords:      (1)
    User:          SYSTEM
    Computer:      sr-srv05.corp.contoso.com
    Description:
    The Recovery Point Objective (RPO) of the asynchronous destination is unavailable.

    LocalReplicationGroupName: rg01
    LocalReplicationGroupId: {e20b6c68-1758-4ce4-bd3b-84a5b5ef2a87}
    LocalReplicaName: f:\
    LocalPartitionId: {27484a49-0f62-4515-8588-3755a292657f}
    ReplicaSetId: {1f6446b5-d5fd-4776-b29b-f235d97d8c63}
    RemoteReplicationGroupName: rg02
    RemoteReplicationGroupId: {7f18e5ea-53ca-4b50-989c-9ac6afb3cc81}
    TargetRPO: 30

    Guidance: This is typically due to one of the following reasons: 

异步目标当前断开连接。 恢复连接后可能会支持 RPO。

    The asynchronous destination is unable to keep pace with the source such that the most recent destination log record is no longer present in the source log. The destination will start block copying. The RPO should become available after block copying completes.

这是初始同步期间的预期行为，可以安全地忽略。 此行为可能会在后续版本中更改。 如果在正在进行的异步复制过程中看到此行为，请调查合作关系，以确定复制延迟并超出配置的 RPO（默认情况下为 30 秒）的原因。

## <a name="event-4004-warning-repeated-after-rebooting-a-replicated-node"></a>重新启动复制节点后会重复事件 4004 警告

在少数通常无法再现的情况下，重启合作关系中的服务器会导致复制失败，且重启的节点会记录带有拒绝访问错误的警告事件 4004。

    Log Name:      Microsoft-Windows-StorageReplica/Admin
    Source:        Microsoft-Windows-StorageReplica
    Date:          3/21/2017 11:43:25 AM
    Event ID:      4004
    Task Category: (7)
    Level:         Warning
    Keywords:      (256)
    User:          SYSTEM
    Computer:      server.contoso.com
    Description:
    Failed to establish a connection to a remote computer.

    RemoteComputerName: server
    LocalReplicationGroupName: rg01
    LocalReplicationGroupId: {a386f747-bcae-40ac-9f4b-1942eb4498a0}
    RemoteReplicationGroupName: rg02
    RemoteReplicationGroupId: {a386f747-bcae-40ac-9f4b-1942eb4498a0}
    ReplicaSetId: {00000000-0000-0000-0000-000000000000}
    RemoteShareName:{a386f747-bcae-40ac-9f4b-1942eb4498a0}.{00000000-0000-0000-0000-000000000000}
    Status: {Access Denied}
    A process has requested access to an object, but has not been granted those access rights.

    Guidance: Possible causes include network failures, share creation failures for the remote replication group, or firewall settings. Make sure SMB traffic is allowed and there are no connectivity issues between the local computer and the remote computer. You should expect this event when suspending replication or removing a replication partnership.

`A process has requested access to an object, but has not been granted those access rights.`请注意`Status: "{Access Denied}"` , 此消息是存储副本中已知的问题, 已在2017年9月12日的质量更新中修复-KB4038782 (OS Build 14393.1715) https://support.microsoft.com/help/4038782/windows-10-update-kb4038782 

## <a name="error-failed-to-bring-the-resource-cluster-disk-x-online-with-a-stretch-cluster"></a>“无法使资源‘Cluster Disk x’联机。”错误 具有拉伸群集

在成功故障转移后尝试使群集磁盘联机时（你会尝试使初始源站点重新成为主站点），你会在故障转移群集管理器中收到错误。 例如：

    Error
    The operation has failed.
    Failed to bring the resource 'Cluster Disk 2' online.

    Error Code: 0x80071397
    The operation failed because either the specified cluster node is not the owner of the resource, or the node is not a possible owner of the resource.

如果你尝试手动移动磁盘或 CSV，你将收到其他错误。 例如：

    Error
    The operation has failed.
    The action 'Move' did not complete.

    Error Code: 0x8007138d
    A cluster node is not available for this operation

此问题是由一个或多个未初始化磁盘附加到一个或多个群集节点引起的。 要解决此问题，请使用 DiskMgmt.msc、DISKPART.EXE 或 Initialize-Disk PowerShell cmdlet 初始化所有附加的存储。

我们正致力于提供可永久解决此问题的更新。 如果你有兴趣协助我们并拥有 Microsoft 高级支持协议，请发送电子邮件到 SRFEED@microsoft.com，以便我们与你一同提出反向移植请求。

## <a name="gpt-error-when-attempting-to-create-a-new-sr-partnership"></a>尝试创建新的 SR 合作关系时出现的 GPT 错误

运行 New-SRPartnership 时失败，并出现以下错误：

    Disk layout type for volume \\?\Volume{GUID}\ is not a valid GPT style layout.
    New-SRPartnership : Unable to create replication group SRG01, detailed reason: Disk layout type for volume
    \\?\Volume{GUID}\ is not a valid GPT style layout.
    At line:1 char:1
    + New-SRPartnership -SourceComputerName nodesrc01 -SourceRGName SRG01 ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (MSFT_WvrAdminTasks:root/Microsoft/...T_WvrAdminTasks) [New-SRPartnership]
    , CimException
    + FullyQualifiedErrorId : Windows System Error 5078,New-SRPartnership

在故障转移群集管理器 GUI 中，没有为磁盘设置复制的选项。

运行 Test-SRTopology 时失败，并出现以下内容： 

    WARNING: Object reference not set to an instance of an object.
    WARNING: System.NullReferenceException
    WARNING:    at Microsoft.FileServices.SR.Powershell.MSFTPartition.GetPartitionInStorageNodeByAccessPath(String AccessPath, String ComputerName, MIObject StorageNode)
       at Microsoft.FileServices.SR.Powershell.Volume.GetVolume(String Path, String ComputerName)
       at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.BeginProcessing()
    Test-SRTopology : Object reference not set to an instance of an object.
    At line:1 char:1
    + Test-SRTopology -SourceComputerName nodesrc01 -SourceVolumeName U: - ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidArgument: (:) [Test-SRTopology], NullReferenceException
    + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand 

这是由于群集功能级别仍然设置为 Windows Server 2012 R2（即 FL 8）导致的。 存储副本应该将特定错误返回到此处，而不是返回不正确的错误映射。

在每个节点上运行 Get-Cluster | fl *。

如果 ClusterFunctionalLevel = 9，即为在此节点上实现存储副本所需的 Windows 2016 ClusterFunctionalLevel 版本。
如果 ClusterFunctionalLevel 是不 9，则需要更新 ClusterFunctionalLevel 才能在此节点上实现存储副本。

若要解决此问题，请运行 PowerShell cmdlet，提高群集功能级别：[更新-Update-clusterfunctionallevel](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel)

## <a name="small-unknown-partition-listed-in-diskmgmt-for-each-replicated-volume"></a>DISKMGMT 中列出的针对每个已复制卷的小未知分区

当运行磁盘管理管理单元 (DISKMGMT.MSC) 时，你会注意到会列出没有标签或驱动器号且大小为 1 MB 的一个或多个卷。 你可能能够删除未知卷，或者你可能会收到：

    "An Unexpected Error has Occurred"  

此行为是设计使然。 这不是一个卷，而是一个分区。 存储副本创建一个 512 KB 的分区作为复制操作的数据库槽（旧版 DiskMgmt.msc 工具四舍五入到最接近的 MB）。 使每个已复制的卷都有一个像这样的分区是正常和可取的做法。 当不再使用时，你可以自由删除此 512 KB 分区；不能删除正在使用的分区。 此分区不会增加或缩小。 如果你正在重新创建我们建议的复制，则将此分区保留为存储副本将声明未使用的分区。

要查看详细信息，请使用 DISKPART 工具或 Get-Partition cmdlet。 这些分区将具有 `558d43c5-a1ac-43c0-aac8-d1472b2923d1` 的 GPT 类型。

## <a name="a-storage-replica-node-hangs-when-creating-snapshots"></a>创建快照时存储副本节点挂起

创建 VSS 快照时（通过备份、VSSADMIN 等）存储副本节点挂起时，必须重新启动该节点才能进行恢复。 没有错误，只是服务器的硬挂起。

当你创建日志卷的 VSS 快照时，会出现此问题。 根本原因是 VSS 的传统设计方面，而不是存储副本。 快照存储副本日志卷时产生的行为是 VSS i/o 队列机制使服务器死锁。

若要防止此行为，请不要快照存储副本日志卷。 无需快照存储副本日志卷，因为这些日志无法还原。 而且，日志卷绝不会包含任何其他工作负荷，因此一般不需要任何快照。

## <a name="high-io-latency-increase-when-using-storage-spaces-direct-with-storage-replica"></a>将存储空间直通与存储副本结合使用时，高 IO 延迟增加

将存储空间直通与 NVME 或 SSD 缓存一起使用时，在存储空间直通群集之间配置存储副本复制时，延迟时间会超出预期的延迟。 延迟的变化比在性能 + 容量配置中使用 NVME 和 SSD 时看到的要高得多，并且没有 HDD 层和容量层。

发生此问题的原因在于，与较慢的媒体相比，存储副本的日志机制内的体系结构限制与 NVME 的延迟非常低。 当使用存储空间直通缓存时，所有存储副本日志的 i/o 以及所有最近的读取/写入 IO 应用程序将出现在缓存中，而从不会出现在性能层或容量层上。 这意味着所有存储副本活动都在同一速度介质上发生-支持此配置，但不建议使用此 https://aka.ms/srfaq 配置（有关日志建议，请参阅）。 

将存储空间直通与 Hdd 一起使用时，不能禁用或避免缓存。 一种解决方法是，如果仅使用 SSD 和 NVME，则可以仅配置性能层和容量层。 如果使用该配置，并且只通过将 SR 日志放在性能层上，只使用其服务在容量层上的数据卷，则可以避免上述高延迟问题。 同样，也可以通过混合速度更快、速度更慢的 Ssd，而不是 NVME。

此解决方法当然并不理想，一些客户可能无法利用它。 存储副本团队正在致力于优化，并提供更新的日志机制，以减少这些人为瓶颈。 此 v1.1 日志首先在 Windows Server 2019 中提供，并在[服务器存储博客](https://blogs.technet.microsoft.com/filecab/2018/12/13/chelsio-rdma-and-storage-replica-perf-on-windows-server-2019-are-💯/)上的中介绍。

## <a name="error-could-not-find-file-when-running-test-srtopology-between-two-clusters"></a>在两个群集之间运行 Test-srtopology 时出现 "找不到文件" 错误

在两个群集之间运行 Test-srtopology 并在其 CSV 路径上运行时，它会失败并出现错误： 

    PS C:\Windows\system32> Test-SRTopology -SourceComputerName NedClusterA -SourceVolumeName C:\ClusterStorage\Volume1 -SourceLogVolumeName L: -DestinationComputerName NedClusterB -DestinationVolumeName C:\ClusterStorage\Volume1 -DestinationLogVolumeName L: -DurationInMinutes 1 -ResultPath C:\Temp

    Validating data and log volumes...
    Measuring Storage Replica recovery and initial synchronization performance...
    WARNING: Could not find file '\\NedNode1\C$\CLUSTERSTORAGE\VOLUME1TestSRTopologyRecoveryTest\SRRecoveryTestFile01.txt'.
    WARNING: System.IO.FileNotFoundException
    WARNING:    at System.IO.__Error.WinIOError(Int32 errorCode, String maybeFullPath)
    at System.IO.FileStream.Init(String path, FileMode mode, FileAccess access, Int32 rights, Boolean useRights, FileShare share, Int32 buff
    erSize, FileOptions options, SECURITY_ATTRIBUTES secAttrs, String msgPath, Boolean bFromProxy, Boolean useLongPath, Boolean checkHost)
    at System.IO.FileStream..ctor(String path, FileMode mode, FileAccess access, FileShare share, Int32 bufferSize, FileOptions options)
    at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.GenerateWriteIOWorkload(String Path, UInt32 IoSizeInBytes, UInt32 Parallel
    IoCount, UInt32 Duration)
    at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.<>c__DisplayClass75_0.<PerformRecoveryTest>b__0()
    at System.Threading.Tasks.Task.Execute()
    Test-SRTopology : Could not find file '\\NedNode1\C$\CLUSTERSTORAGE\VOLUME1TestSRTopologyRecoveryTest\SRRecoveryTestFile01.txt'.
    At line:1 char:1
    + Test-SRTopology -SourceComputerName NedClusterA -SourceVolumeName  ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (:) [Test-SRTopology], FileNotFoundException
    + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand 

这是由 Windows Server 2016 中的已知代码缺陷导致的。 此问题是在 Windows Server 版本1709和关联的 RSAT 工具中首先修复的。 若要进行下层解析，请联系 Microsoft 支持部门并请求向后移植更新。 此问题没有解决方法。

## <a name="error-specified-volume-could-not-be-found-when-running-test-srtopology-between-two-clusters"></a>在两个群集之间运行 Test-srtopology 时，出现错误 "找不到指定的卷"

在两个群集之间运行 Test-srtopology 并在其 CSV 路径上运行时，它会失败并出现错误：

    PS C:\> Test-SRTopology -SourceComputerName RRN44-14-09 -SourceVolumeName C:\ClusterStorage\Volume1 -SourceLogVolumeName L: -DestinationComputerName RRN44-14-13 -DestinationVolumeName C:\ClusterStorage\Volume1 -DestinationLogVolumeName L: -DurationInMinutes 30 -ResultPath c:\report

    Test-SRTopology : The specified volume C:\ClusterStorage\Volume1 cannot be found on computer RRN44-14-09. If this is a cluster node, the volume must be part of a role or CSV; volumes in Available Storage are not accessible
    At line:1 char:1
    + Test-SRTopology -SourceComputerName RRN44-14-09 -SourceVolumeName C:\ ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (:) [Test-SRTopology], Exception
        + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand

将源节点 CSV 指定为源卷时，必须选择拥有 CSV 的节点。 可以将 CSV 移动到指定的节点，也可以更改在中`-SourceComputerName`指定的节点名称。 此错误在 Windows Server 2019 中收到改进的消息。

## <a name="unable-to-access-the-data-drive-in-storage-replica-after-unexpected-reboot-when-bitlocker-is-enabled"></a>启用 BitLocker 后，在意外重新启动后无法访问存储副本中的数据驱动器

如果在两个驱动器（日志驱动器和数据驱动器）和两个存储副本驱动器中启用了 BitLocker，则当主服务器重新启动时，即使在从 BitLocker 中解除日志驱动器的锁定后，也无法访问主驱动器。

这是预期的行为。 若要恢复数据或访问驱动器，需要先解锁日志驱动器，然后打开 Diskmgmt.msc 以查找数据驱动器。 再次将数据驱动器脱机和联机。 找到驱动器上的 BitLocker 图标并解锁驱动器。

## <a name="issue-unlocking-the-data-drive-on-secondary-server-after-breaking-the-storage-replica-partnership"></a>中断存储副本合作关系后，对辅助服务器上的数据驱动器进行解锁

禁用 SR 合作关系并删除存储副本后，如果无法通过其各自的密码或密钥解锁辅助服务器的数据驱动器，则会出现这种情况。 

需要使用主服务器的数据驱动器的密钥或密码来解锁辅助服务器的数据驱动器。

## <a name="test-failover-doesnt-mount-when-using-asynchronous-replication"></a>使用异步复制时测试故障转移不会装载

当运行 SRDestination 以使目标卷在测试故障转移功能中联机时，它会失败，并出现以下错误：

    Mount-SRDestination: Unable to mount SR group <TEST>, detailed reason: The group or resource is not in the correct state to perform the supported operation.
    At line:1 char:1
    + Mount-SRDestination -ComputerName SRV1 -Name TEST -TemporaryP . . .
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (MSFT WvrAdminTasks : root/Microsoft/...(MSFT WvrAdminTasks : root/Microsoft/. T_WvrAdminTasks) (Mount-SRDestination], CimException
        + FullyQua1ifiedErrorId : Windows System Error 5823, Mount-SRDestination.  

如果使用同步合作关系类型，则测试故障转移会正常运行。

这是由 Windows Server 版本1709中的已知代码缺陷导致的。 若要解决此问题，请安装[10 月18日2018更新](https://support.microsoft.com/help/4462932/windows-10-update-kb4462932)。 Windows Server 2019 和 Windows Server 1809 和更高版本中不存在此问题。

## <a name="see-also"></a>请参阅

- [存储副本](storage-replica-overview.md)  
- [使用共享存储拉伸群集复制](stretch-cluster-replication-using-shared-storage.md)  
- [服务器到服务器存储复制](server-to-server-storage-replication.md)  
- [群集到群集存储复制](cluster-to-cluster-storage-replication.md)  
- [存储副本：常见问题解答](storage-replica-frequently-asked-questions.md)  
- [存储空间直通](../storage-spaces/storage-spaces-direct-overview.md)  
