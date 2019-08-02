---
title: 有关存储副本的常见问题
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: d4eb2ad65f436db28264650b8c8d7b0cf69b2cee
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544676"
---
# <a name="frequently-asked-questions-about-storage-replica"></a>有关存储副本的常见问题

>适用于：Windows Server 2019、Windows Server 2016、Windows Server（半年频道）

本主题包含存储副本相关的常见问题解答 (FAQ)。

## <a name="FAQ1"></a>Azure 上是否支持存储副本？
是。 你可以在 Azure 中使用以下方案:

1. Azure 中的服务器到服务器复制 (在一个或两个数据中心容错域中的 IaaS Vm 之间同步或异步复制, 或在两个不同的区域之间异步复制)
2. 在 Azure 和本地 (使用 VPN 或 Azure ExpressRoute) 之间进行服务器到服务器的异步复制
3. Azure 中的群集到群集复制 (在一个或两个数据中心容错域中的 IaaS Vm 之间同步或异步复制, 或在两个不同的区域之间异步复制)
4. 在 Azure 和本地 (使用 VPN 或 Azure ExpressRoute) 之间进行群集到群集的异步复制

有关 Azure 中的来宾群集的详细说明, 请参阅:[在 Microsoft Azure 中部署 IAAS VM 来宾群集](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126)。

重要说明:

1. Azure 不支持共享的 VHDX 来宾群集, 因此, Windows 故障转移群集虚拟机必须使用 iSCSI 目标来实现经典共享存储永久性磁盘保留群集或存储空间直通。
2. 有用于基于存储空间直通的存储副本群集的 Azure 资源管理器模板, 请在[创建包含存储副本的存储空间直通 SOFS 群集, 以实现跨 Azure 区域的灾难恢复](https://aka.ms/azure-storage-replica-cluster)。  
3. 群集到群集中的 RPC 通信 (群集 Api 在群集之间授予访问权限所需) 需要为 CNO 配置网络访问。 必须允许 TCP 端口135和 TCP 端口49152以上的动态范围。 参考[在 AZURE IAAS VM 上构建 Windows Server 故障转移群集–第2部分网络和创建](https://blogs.technet.microsoft.com/askcore/2015/06/24/building-windows-server-failover-cluster-on-azure-iaas-vm-part-2-network-and-creation/)。  
4. 可以使用双节点来宾群集, 其中每个节点都对存储副本复制的非对称群集使用环回 iSCSI。 但这可能会产生非常差的性能, 只应用于极有限的工作负荷或测试。  

## <a name="FAQ2"></a>如何实现在初始同步过程中查看复制进度？  
在目标服务器的存储副本管理员事件日志中显示的事件 1237 消息每 10 秒就会显示复制的字节数和剩余字节数。 还可以使用在显示一个或多个复制卷的 **\Storage Replica Statistics\Total Bytes Received** 的目标上使用存储副本性能计数器。 此外，可以查询使用 Windows PowerShell 的复制组。 例如，此示例命令获取目标上的组名，然后每隔 10 秒查询名为**复制 2** 的组来显示进度：  

```  
Get-SRGroup

do{
    $r=(Get-SRGroup -Name "Replication 2").replicas
    [System.Console]::Write("Number of remaining bytes {0}`n", $r.NumOfBytesRemaining)
    Start-Sleep 10
}until($r.ReplicationStatus -eq 'ContinuouslyReplicating')
Write-Output "Replica Status: "$r.replicationstatus

```  

## <a name="FAQ3"></a>是否可以指定用于复制的特定网络接口？  

是，使用 `Set-SRNetworkConstraint`。 此 cmdlet 在接口层进行操作，并可用于群集和非群集方案。  
例如，对于（每个节点上）的独立服务器：  

```  
Get-SRPartnership  

Get-NetIPConfiguration  
```  
记下网关和接口信息（在这两台服务器上）以及合作关系方向。 然后运行：  

```  
Set-SRNetworkConstraint -SourceComputerName sr-srv06 -SourceRGName rg02 -  
SourceNWInterface 2 -DestinationComputerName sr-srv05 -DestinationNWInterface 3 -DestinationRGName rg01  

Get-SRNetworkConstraint  

Update-SmbMultichannelConnection  

```  

对于在拉伸群集上配置网络约束：  

    Set-SRNetworkConstraint -SourceComputerName sr-cluster01 -SourceRGName group1 -SourceNWInterface "Cluster Network 1","Cluster Network 2" -DestinationComputerName sr-cluster02 -DestinationRGName group2 -DestinationNWInterface "Cluster Network 1","Cluster Network 2"

## <a name="FAQ4"></a>能否配置一对多复制或可传递 (从 A 到 B 到 C) 复制？  
不可以, 存储副本仅支持服务器、群集或 stretch 群集节点的一个复制。 这可能会在后续版本中更改。 当然，你可以在特定卷对的各种服务器之间以任一方向配置复制。 例如，服务器 1 可以将其 D 卷复制到服务器 2，以及从服务器 3 复制其 E 卷。

## <a name="FAQ5"></a>是否可以增加或减少存储副本复制的复制卷？  
可以增大（扩展）卷，但不能收缩它们。 默认情况下，存储副本会阻止管理员扩展复制卷；请在调整大小之前对源组使用 `Set-SRGroup -AllowVolumeResize $TRUE` 选项。 例如：

1. 针对源计算机使用:`Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. 使用你喜欢的任何技术增大卷
3. 针对源计算机使用:`Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="FAQ6"></a>是否可以使目标卷联机以进行只读访问？  
在 Windows Server 2016 中不能。 存储副本可在复制开始时卸除目标卷。 

但是, 在 Windows Server 2019 和 Windows Server 半年版中, 从版本1709开始, 现在可以用于装载目标存储的选项, 此功能称为 "测试故障转移"。 若要执行此操作，你必须具有当前未在目标位置进行复制的未使用的 NTFS 或 ReFS 格式卷。 然后，你可以临时安装复制的存储的快照以便进行测试或备份。 

例如，若要创建一个测试故障转移，其中，你要在目标服务器“SRV2”上的复制组“RG2”中复制卷“D:”，并且 SRV2 上有未被复制的“T:”驱动器，可执行以下命令：

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
复制的卷 D: 现在可以在 SRV2 上访问。 您可以在 D: 路径下对它进行常规的读写操作, 将文件复制到其中, 或者运行在其他位置保存的联机备份。 T: 卷将只包含日志数据。

若要删除测试故障转移快照并放弃对其所做的更改，可执行以下命令：

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
测试故障转移功能只应该用于短期临时操作。 它不适合长期使用。 在使用过程中，复制操作会继续复制到真正的目标卷。 

## <a name="FAQ7"></a>能否在 stretch 群集中配置横向扩展文件服务器 (SOFS)？  
尽管从技术上讲, 这不是推荐的配置, 因为在与 SOFS 联系的计算节点中缺少站点感知。 如果使用的是校园网络, 延迟通常是毫秒, 则此配置通常不会出现问题。   

在两个群集间进行复制时，如果配置群集到群集复制，存储副本会完全支持横向扩展文件服务器，包括可以使用存储空间直通。  

## <a name="FAQ7.5"></a>在 stretch 群集或群集之间复制是否需要 CSV？  
否。 可以使用群集资源 (如文件服务器角色) 拥有的 CSV 或持久磁盘保留 (PDR) 进行复制。 

在两个群集间进行复制时，如果配置群集到群集复制，存储副本会完全支持横向扩展文件服务器，包括可以使用存储空间直通。  

## <a name="FAQ8"></a>能否在包含存储副本的 stretch 群集中配置存储空间直通？  
这不是 Windows Server 支持的配置。 这可能会在后续版本中更改。 如果配置群集到群集复制，存储副本会完全支持横向扩展文件服务器和 Hyper-V 服务器，包括可以使用存储空间直通。  

## <a name="FAQ9"></a>如何实现配置异步复制？  

指定 `New-SRPartnership -ReplicationMode`并提供参数 **Asynchronous**。 默认情况下，存储副本中的所有复制都是同步的。 还可以使用 `Set-SRPartnership -ReplicationMode` 更改模式。  

## <a name="FAQ10"></a>如何实现阻止 stretch 群集的自动故障转移？  
为了防止自动故障转移，可以使用 PowerShell 来配置 `Get-ClusterNode -Name "NodeName").NodeWeight=0`。 这将在灾难恢复站点中删除每个节点上的投票。 然后，可以在主站点的节点上使用 `Start-ClusterNode -PreventQuorum`，并在灾难站点的节点上使用 `Start-ClusterNode -ForceQuorum` 来强制进行故障转移。 没有阻止自动故障转移的图形选项，不建议阻止自动故障转移。  

## <a name="FAQ11"></a>如何实现禁用虚拟机复原能力？
若要防止新的 Hyper-v 虚拟机复原功能运行, 从而暂停虚拟机, 而不是将其故障转移到灾难恢复站点, 请运行`(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="FAQ12"></a>如何缩短初始同步的时间？

可以使用精简配置的存储来缩短初始同步时间。 存储副本查询并自动使用精简配置的存储，包括非聚集存储空间、Hyper-V 动态磁盘和 SAN LUN。  

你还可以使用种子数据卷来减少带宽使用时间, 有时可以通过确保目标卷具有主数据的一部分数据, 然后使用故障转移群集管理器或`New-SRPartnership`中的种子选项来减少带宽使用量。 如果该卷大部分为空，则使用植入的同步会减少时间和带宽的使用量。 有多种方法可对数据进行种子设定, 并具有不同的效力度:

1. 以前的复制-通过使用常规初始同步在包含磁盘和卷的节点之间进行复制, 删除复制, 将目标磁盘发送到其他位置, 然后使用种子选项添加复制。 这是最有效的方法, 因为存储副本保证块复制镜像, 而复制的唯一内容是增量块。
2. 还原快照或已还原的基于快照的备份-通过将基于卷的快照还原到目标卷上, 块布局中的差异应该最小。 这是下一种最有效的方法, 因为由于卷快照是镜像映像, 块可能会匹配。
3. 复制的文件-通过在目标上创建从未使用过的、并执行数据的完整 robocopy/MIR 树副本的新卷, 可能会有块匹配。 使用 Windows 文件资源管理器或复制部分树不会创建多个块匹配项。 手动复制文件是种子设定的最小方法。

## <a name="FAQ13"></a>我是否可以委派用户管理复制？  

可以使用`Grant-SRDelegation` cmdlet。 这样，可以设置特定用户在服务器到服务器、群集到群集以及拉伸群集复制方案中拥有创建、修改或删除复制的权限，而无需成为本地管理员组中的成员。 例如：  

    Grant-SRDelegation -UserName contso\tonywang  

该 cmdlet 将提醒你，用户需要登录和注销他们计划进行管理的服务器，以便使更改生效。 可以使用 `Get-SRDelegation` 和 `Revoke-SRDelegation` 对此进行进一步控制。  

## <a name="FAQ13"></a>复制的卷有哪些备份和还原选项？
存储副本支持对源卷进行备份和还原。 它还支持创建和还原源卷的快照。 无法备份或还原由存储副本保护的目标卷，因为该卷未装载或不可访问。 如果你经历灾难，源卷丢失，请使用 `Set-SRPartnership` 将以前的目标卷提升至现在可读/写的源，这样就可以备份或还原该卷。 还可以使用 `Remove-SRPartnership` 和 `Remove-SRGroup` 删除复制以将该卷重新安装为可读/写。
若要定期创建应用程序的一致性快照，可以在源服务器上使用 VSSADMIN.EXE 来截取复制数据卷的快照。 例如，使用存储副本复制 F: 卷：

    vssadmin create shadow /for=F:
然后在切换复制方向后删除复制，或仍在同一个源卷上，可以及时将任何快照还原到其点。 例如，仍使用 F:

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
还可以使用计划任务安排此工具定期运行。 有关使用 VSS 的详细信息，请参阅 [Vssadmin](../../administration/windows-commands/vssadmin.md)。 备份日志卷没有必要，也没有价值。 若尝试执行此操作，VSS 将会忽略该操作。
存储副本支持使用 Windows Server 备份、Microsoft Azure 备份、Microsoft DPM 或其他快照、VSS、虚拟机或基于文件的技术，只要它们在卷层中运行。 存储副本不支持基于块的备份和还原。

## <a name="FAQ14"></a>是否可以配置复制以限制带宽使用率？
可以，可通过 SMB 带宽限制实现限制。 这是适用于所有存储副本流量的全局设置，因此会影响此服务器的所有复制。 通常情况下，仅在进行存储副本初始同步设置（其中所有卷数据都必须进行传输）的情况下需要执行此设置。 如果在初始同步后仍需要进行此设置，则你的网络带宽过低，不足以支持 IO 工作负荷；减少 IO 或增加带宽。

这应仅与异步复制一起使用（请注意：即使你已指定同步，初始同步仍会始终以异步方式进行）。
还可以使用网络 QoS 策略来设置存储副本流量。 使用高度匹配的植入存储副本复制还会大幅降低整体初始同步带宽的使用。


若要设置带宽限制，请使用：

    Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x

若要查看带宽限制，请使用：

    Get-SmbBandwidthLimit -Category StorageReplication

若要删除带宽限制，请使用：

    Remove-SmbBandwidthLimit -Category StorageReplication
    
## <a name="FAQ15"></a>存储副本需要哪些网络端口？
存储副本依赖 SMB 和 WSMAN 进行其复制和管理。 这意味着必需使用以下端口：

 445 (SMB-复制传输协议, 群集 RPC 管理协议) 5445 (仅在使用 iWARP RDMA 网络时需要 iWARP SMB) 5985 (WSManHTTP-WMI/CIM/PowerShell 的管理协议)

注意:Test-srtopology cmdlet 需要 ICMPv4/ICMPv6, 但不需要复制或管理。

## <a name="FAQ15.5"></a>日志卷的最佳做法是什么？
日志的最佳大小因环境和工作负荷而异, 由工作负荷执行的写入 IO 量决定。 

1.  日志大小与操作速度无关
2.  例如，在对比 10 GB 数据卷与 10 TB 数据卷时日志大小无任何影响

较大的日志可在写入 IO 被换出之前简单收集和保留更多的写入 IO。这可允许源计算机和目标计算机之间的服务中断（例如网络中断或目标脱机）持续更长时间。 如果日志可以保存 10 小时的写入，而网络中断的时间为 2 小时，则当网络恢复的时候，源计算机可以非常轻松、快速地将未同步更改的增量同步至目标计算机，从而快速地再次提供保护。 如果日志可保存 10 小时的写入，而中断时间为 2 天，则源计算机必须从被称为位图的其他日志进行操作，并可能会较慢恢复同步。同步后它将返回使用日志。

存储副本依靠该日志来实现所有写入性能。 对复制性能至关重要的日志性能。 必须确保日志卷的性能优于数据卷，因为日志将对所有写入 IO 进行序列化和顺序化。 应始终在日志卷上使用闪存介质，例如 SSD。 不得允许其他任何工作负荷在日志卷上运行，同样不得允许其他工作负荷在 SQL 数据库日志卷上运行。 

遍Microsoft 强烈建议日志存储速度要快于数据存储, 并且日志卷永远不能用于其他工作负荷。

可以通过运行 Test-srtopology 工具获取日志大小建议。 或者, 你可以使用现有服务器上的性能计数器来使日志大小毋庸置疑。 公式非常简单: 监视工作负荷下的数据磁盘吞吐量 (平均写入字节数/秒), 并使用它来计算填充不同大小的日志所需的时间量。 例如, 50 MB/s 的数据磁盘吞吐量将导致120GB 的日志换行 120GB/50MB 秒或2400秒或40分钟。 因此, 在日志包装之前目标服务器无法访问的时间长度为40分钟。 如果日志换行但目标再次可访问, 则源会通过位图日志而不是主日志重播块。 日志的大小不会影响性能。

只应备份源群集中的数据磁盘。 由于备份可能与存储副本操作冲突, 因此不应备份存储副本日志磁盘。

## <a name="FAQ16"></a>为什么要选择 stretch 群集与群集到群集的拓扑, 以及服务器到服务器的拓扑？  
存储副本有三个主要配置: stretch 群集、群集到群集和服务器到服务器。 每种配置都具有不同的优势。

拉伸群集拓扑适用于需要带业务流程的自动故障转移的工作负荷，例如 Hyper-V 私有云群集和 SQL Server FCI。 此外还具有使用故障转移群集管理器的内置图形界面。 它通过永久保留采用存储空间的经典对称群集共享存储架构，即 SAN、iSCSI 和 RAID。 使用至少 2 个节点进行运行。

群集到群集拓扑使用两个独立群集，非常适用于需要手动故障转移的管理员，尤其是当预配第二个站点以进行灾难恢复以及无需每日使用的时候。 业务流程为手动操作。 与拉伸群集不同, 存储空间直通可以在此配置中使用 (请参阅存储副本常见问题解答和群集到群集文档)。 使用至少四个节点进行运行。 

服务器到服务器拓扑适用于运行无法组成群集的硬件的客户。 这种配置需要手动故障转移和业务流程。 它非常适合用于分支机构和中心数据中心之间的廉价部署, 尤其是在使用异步复制时。 此配置通常可替代用于单主机灾难恢复方案的受 DFSR 保护的文件服务器的实例。

在所有的情况下，拓扑支持在物理硬件和虚拟机运行。 在虚拟机上运行时，基础虚拟机监控程序无需 Hyper-V；可以是 VMware、KVM 和 Xen 等。

存储副本还具有一个服务器到自我模式，可以在此模式中将复制指定到同一台计算机上的两个不同卷。

## <a name="FAQ18"></a>存储副本是否支持重复数据删除？

是的, 存储副本支持数据 Deduplcation。 在源服务器上的卷上启用重复数据删除, 在复制过程中, 目标服务器收到卷的已删除重复的副本。

尽管应该在源服务器和目标服务器上*安装*重复数据删除 (请参阅[安装和启用重复数据删除](../data-deduplication/install-enable.md)), 但不要在目标服务器上*启用*重复数据删除。 存储副本仅允许在源服务器上写入。 由于重复数据删除将写入卷, 因此它只应在源服务器上运行。 

## <a name="FAQ19"></a>能否在 Windows Server 2019 和 Windows Server 2016 之间进行复制？

遗憾的是, 我们不支持在 Windows Server 2019 和 Windows Server 2016 之间创建*新*的合作关系。 您可以安全地将运行 Windows Server 2016 的服务器或群集升级到 Windows Server 2019, 并且任何*现有*的合作关系都将继续工作。

但是, 若要获得改进 Windows Server 2019 的复制性能, 合作关系的所有成员必须运行 Windows Server 2019, 并且必须删除现有的合作关系和关联的复制组, 然后再用种子数据重新创建它们 (在 Windows 管理中心或 Remove-srpartnership cmdlet 中创建合作关系时。

## <a name="FAQ17"></a>如何实现报告存储副本或本指南存在问题？  
如需存储副本的技术援助，可以通过 [Microsoft TechNet 论坛](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview) 发布。 还可以将有关存储副本的疑问或本文档中出现的问题通过电子邮件发送至 srfeed@microsoft.com。 <https://windowsserver.uservoice.com> 站点是设计更改请求的首选，因为它允许客户对你的想法提供支持和反馈。



## <a name="related-topics"></a>相关主题  
- [存储副本概述](storage-replica-overview.md) 
- [使用共享存储拉伸群集复制](stretch-cluster-replication-using-shared-storage.md)  
- [服务器到服务器存储复制](server-to-server-storage-replication.md)  
- [群集到群集存储复制](cluster-to-cluster-storage-replication.md)  
- [存储副本：已知问题](storage-replica-known-issues.md)  

## <a name="see-also"></a>请参阅  
- [存储概述](../storage.md)  
- [存储空间直通](../storage-spaces/storage-spaces-direct-overview.md)  
