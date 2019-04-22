---
title: 有关存储副本的常见问题
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 12/19/2018
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: 0e010f0319b46e04cf9aa15cde9552af1191ab22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824708"
---
# <a name="frequently-asked-questions-about-storage-replica"></a>有关存储副本的常见问题

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题包含存储副本相关的常见问题解答 (FAQ)。

## <a name="FAQ1"></a> 在 Azure 上支持存储副本？  
是。 可以在 Azure 中使用以下方案：

1. 在 Azure 内的服务器到服务器复制 （同步或异步之间一个或两个数据中心容错域中的 IaaS Vm 或异步两个单独的区域之间）
2. Azure 与本地之间服务器到服务器异步复制 （使用 VPN 或 Azure ExpressRoute）
3. 在 Azure 内的群集到群集复制 （同步或异步之间一个或两个数据中心容错域中的 IaaS Vm 或异步两个单独的区域之间）
4. 群集到群集在本地和 Azure 之间异步复制 （使用 VPN 或 Azure ExpressRoute）

可从 Azure 中的来宾群集上的进一步说明：[部署 Microsoft Azure 中的 IaaS VM 来宾群集](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/)。

重要说明：

1. Azure 不支持共享的 VHDX 来宾群集，因此 Windows 故障转移群集虚拟机必须使用经典的共享存储持久性磁盘保留群集或存储空间直通的 iSCSI 目标。
2. 基于存储空间直通的存储副本在聚类分析的 Azure 资源管理器模板[跨 Azure 区域存储空间直通 (S2D) 的 SOFS 群集使用存储副本创建灾难恢复](https://aka.ms/azure-storage-replica-cluster)。  
3. 群集在 Azure （需要由群集 Api 授予群集之间的访问权限） 中的 RPC 通信的群集需要为 CNO 配置网络访问权限。 TCP 端口 49152 上方，则必须允许 TCP 端口 135 和动态范围。 引用[构建 Windows Server 故障转移群集在 Azure IAAS VM – 第 2 个网络和创建部分](https://blogs.technet.microsoft.com/askcore/2015/06/24/building-windows-server-failover-cluster-on-azure-iaas-vm-part-2-network-and-creation/)。  
4. 它是可以使用两个节点来宾群集，其中每个节点由存储副本复制为非对称群集使用环回 iSCSI。 但这可能会很差的性能，并且仅应该用于非常有限的工作负荷或测试。  

## <a name="FAQ2"></a> 如何查看初始同步过程中的复制进度？  
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

## <a name="FAQ3"></a>可以指定要用于复制的特定网络接口？  

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

    Set-SRNetworkConstraint -SourceComputerName sr-srv01 -SourceRGName group1 -SourceNWInterface "Cluster Network 1","Cluster Network 2" -DestinationComputerName sr-srv03 -DestinationRGName group2 -DestinationNWInterface "Cluster Network 1","Cluster Network 2"  

## <a name="FAQ4"></a> 可以配置一个多复制或可传递 (A 到 B 到 C) 复制吗？  
在 Windows Server 2016 中不能。 此版本仅支持服务器、群集或拉伸群集节点的一对一复制。 这可能会在后续版本中更改。 当然，你可以在特定卷对的各种服务器之间以任一方向配置复制。 例如，服务器 1 可以将其 D 卷复制到服务器 2，以及从服务器 3 复制其 E 卷。

## <a name="FAQ5"></a> 可以扩大或缩小由存储副本复制的已复制的卷？  
可以增大（扩展）卷，但不能收缩它们。 默认情况下，存储副本会阻止管理员扩展复制卷；请在调整大小之前对源组使用 `Set-SRGroup -AllowVolumeResize $TRUE` 选项。 例如：

1. 使用针对源计算机： `Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. 使用你喜欢的任何技术增大卷
3. 使用针对源计算机： `Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="FAQ6"></a>可将目标卷联机放置的只读访问权限？  
在 Windows Server 2016 RTM（即所谓的“RS1”版本）中不能。 存储副本可在复制开始时卸除目标卷。 

但是，在 Windows Server 版本 1709 中，现在可以使用用于安装目标存储的选项 - 此功能称为“测试故障转移”。 若要执行此操作，你必须具有当前未在目标位置进行复制的未使用的 NTFS 或 ReFS 格式卷。 然后，你可以临时安装复制的存储的快照以便进行测试或备份。 

例如，若要创建一个测试故障转移，其中，你要在目标服务器“SRV2”上的复制组“RG2”中复制卷“D:”，并且 SRV2 上有未被复制的“T:”驱动器，可执行以下命令：

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
复制的卷 D: 现在可以在 SRV2 上访问。 你可以读取和写入到它通常情况下，将关闭它，文件复制或运行保存以保证安全，d： 路径下的其他位置的联机备份。 T： 卷将仅包含日志数据。

若要删除测试故障转移快照并放弃对其所做的更改，可执行以下命令：

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
测试故障转移功能只应该用于短期临时操作。 它不适合长期使用。 在使用过程中，复制操作会继续复制到真正的目标卷。 

## <a name="FAQ7"></a> 可以在拉伸群集中配置横向扩展文件服务器 (SOFS)？  
虽然从技术上来说，这是可以实现的，但建议不要在 Windows Server 2016 中进行此配置，因为联系 SOFS 的计算节点中缺少站点感知。 如果使用的校园网，其延迟通常是亚毫秒级的则此配置通常会正常时未出现问题。   

在两个群集间进行复制时，如果配置群集到群集复制，存储副本会完全支持横向扩展文件服务器，包括可以使用存储空间直通。  

## <a name="FAQ7.5"></a> CSV 被需要在拉伸群集或群集之间复制？  
否。 可以使用 CSV 或持久性磁盘保留 (PDR) 拥有的群集资源，例如文件服务器角色进行复制。 

在两个群集间进行复制时，如果配置群集到群集复制，存储副本会完全支持横向扩展文件服务器，包括可以使用存储空间直通。  

## <a name="FAQ8"></a>可以配置存储空间直通在拉伸群集中使用存储副本？  
这不是 Windows Server 2016 中受支持的配置。  这可能会在后续版本中更改。 如果配置群集到群集复制，存储副本会完全支持横向扩展文件服务器和 Hyper-V 服务器，包括可以使用存储空间直通。  

## <a name="FAQ9"></a>如何配置异步复制？  

指定 `New-SRPartnership -ReplicationMode`并提供参数 **Asynchronous**。 默认情况下，存储副本中的所有复制都是同步的。 还可以使用 `Set-SRPartnership -ReplicationMode` 更改模式。  

## <a name="FAQ10"></a>如何防止拉伸群集的自动故障转移？  
为了防止自动故障转移，可以使用 PowerShell 来配置 `Get-ClusterNode -Name "NodeName").NodeWeight=0`。 这将在灾难恢复站点中删除每个节点上的投票。 然后，可以在主站点的节点上使用 `Start-ClusterNode -PreventQuorum`，并在灾难站点的节点上使用 `Start-ClusterNode -ForceQuorum` 来强制进行故障转移。 没有阻止自动故障转移的图形选项，不建议阻止自动故障转移。  

## <a name="FAQ11"></a>如何禁用虚拟机复原性？  
若要防止从运行和暂停虚拟机而不是其故障转移到灾难恢复站点的新的 HYPER-V 虚拟机复原功能，请运行 `(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="FAQ12"></a> 如何减少初始同步的时间？  

可以使用精简配置的存储来缩短初始同步时间。 存储副本查询并自动使用精简配置的存储，包括非聚集存储空间、Hyper-V 动态磁盘和 SAN LUN。  

可以使用植入的数据卷减少带宽使用情况和有时时间、 通过确保目标卷具有某一部分的数据从主数据库然后使用故障转移群集管理器中的植入选项或`New-SRPartnership`。 如果该卷大部分为空，则使用植入的同步会减少时间和带宽的使用量。 有多种到种子数据，且不同程度的功效：

1. 上一个复制-通过使用普通的初始复制本地之间同步节点包含的磁盘和卷，删除复制，目标磁盘寄送到其他位置，然后添加带有植入选项的复制。 这是最有效方法，因为存储副本保证块复制镜像，唯一需要复制增量数据块。
2. 还原快照或还原基于快照的备份-还原到目标卷上的基于卷的快照应该有块布局中的最小差异。 这是下一步最有效方法，因为块很可能会由于正在镜像卷的快照与匹配。
3. 复制的文件-通过从未使用过的目标上创建新卷并执行完整的 robocopy /MIR 树将复制的数据，有可能是块匹配项。 使用 Windows 文件资源管理器或复制树的某些部分不会创建多块匹配项。 手动复制文件是种子设定的最有效方法。



## <a name="FAQ13"></a> 我可以委派用户管理复制？  

可以在 Windows Server 2016 中使用 `Grant-SRDelegation` cmdlet。 这样，可以设置特定用户在服务器到服务器、群集到群集以及拉伸群集复制方案中拥有创建、修改或删除复制的权限，而无需成为本地管理员组中的成员。 例如：  

    Grant-SRDelegation -UserName contso\tonywang  

该 cmdlet 将提醒你，用户需要登录和注销他们计划进行管理的服务器，以便使更改生效。 可以使用 `Get-SRDelegation` 和 `Revoke-SRDelegation` 对此进行进一步控制。  

## <a name="FAQ13"></a> 什么是我的备份和还原选项的已复制卷？
存储副本支持对源卷进行备份和还原。 它还支持创建和还原源卷的快照。 无法备份或还原由存储副本保护的目标卷，因为该卷未装载或不可访问。 如果你经历灾难，源卷丢失，请使用 `Set-SRPartnership` 将以前的目标卷提升至现在可读/写的源，这样就可以备份或还原该卷。 还可以使用 `Remove-SRPartnership` 和 `Remove-SRGroup` 删除复制以将该卷重新安装为可读/写。
若要定期创建应用程序的一致性快照，可以在源服务器上使用 VSSADMIN.EXE 来截取复制数据卷的快照。 例如，使用存储副本复制 F: 卷：

    vssadmin create shadow /for=F:
然后在切换复制方向后删除复制，或仍在同一个源卷上，可以及时将任何快照还原到其点。 例如，仍使用 F:

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
还可以使用计划任务安排此工具定期运行。 有关使用 VSS 的详细信息，请参阅 [Vssadmin](https://technet.microsoft.com/library/cc754968.aspx)。 备份日志卷没有必要，也没有价值。 若尝试执行此操作，VSS 将会忽略该操作。
存储副本支持使用 Windows Server 备份、Microsoft Azure 备份、Microsoft DPM 或其他快照、VSS、虚拟机或基于文件的技术，只要它们在卷层中运行。 存储副本不支持基于块的备份和还原。

## <a name="FAQ14"></a> 可以配置复制，以限制带宽的使用？
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

 445 (SMB 的复制传输协议、 群集 RPC 管理协议) 5445 (iWARP SMB-使用 iWARP RDMA 网络时，才需要) 5985 (WSManHTTP-WMI/CIM/PowerShell 的管理协议)

注意：Test-srtopology cmdlet 需要 ICMPv4/ICMPv6，而不是复制或管理。

## <a name="FAQ15.5"></a>日志卷的最佳做法是什么？
日志的最佳大小每个环境和工作负荷中，值变化很大，可以确定多大的写入的 IO 工作负荷执行。 

1.  日志大小与操作速度无关
2.  例如，在对比 10 GB 数据卷与 10 TB 数据卷时日志大小无任何影响

较大的日志可在写入 IO 被换出之前简单收集和保留更多的写入 IO。这可允许源计算机和目标计算机之间的服务中断（例如网络中断或目标脱机）持续更长时间。 如果日志可以保存 10 小时的写入，而网络中断的时间为 2 小时，则当网络恢复的时候，源计算机可以非常轻松、快速地将未同步更改的增量同步至目标计算机，从而快速地再次提供保护。 如果日志可保存 10 小时的写入，而中断时间为 2 天，则源计算机必须从被称为位图的其他日志进行操作，并可能会较慢恢复同步。同步后它将返回使用日志。

存储副本依靠该日志来实现所有写入性能。 对复制性能至关重要的日志性能。 必须确保日志卷的性能优于数据卷，因为日志将对所有写入 IO 进行序列化和顺序化。 应始终在日志卷上使用闪存介质，例如 SSD。 不得允许其他任何工作负荷在日志卷上运行，同样不得允许其他工作负荷在 SQL 数据库日志卷上运行。 

再次：Microsoft 强烈建议，日志存储比数据存储速度更快，日志卷必须永远不会使用的其他工作负荷。

可以通过运行 Test-srtopology 工具获取日志的大小调整建议。 或者，可以在现有的服务器上使用性能计数器以使其日志大小自行判断。 该公式很简单： 监视工作负荷下的数据磁盘吞吐量 (Avg Write Bytes/Sec) 并使用它来计算所需填满不同大小的日志的时间量。 例如，数据磁盘吞吐量为 50 MB/s 将导致 120 GB 才能将包装在 120 GB/50 MB 秒或 2400 秒或 40 分钟的日志。 因此，目标服务器可以是时间的包装的日志之前无法访问量是时间的 40 分钟的时间。 如果日志包装，但目标将重新变为可访问，源将重播通过位映射日志而不是主要的日志块。 日志的大小并没有对性能产生影响。

仅源群集中的数据磁盘应备份。 存储副本日志磁盘应不会备份由于备份可能会使用存储副本操作发生冲突。

## <a name="FAQ16"></a> 为什么要选择与群集到群集服务器到服务器拓扑与拉伸群集？  
存储副本有三种主要配置：拉伸群集、群集到群集和服务器到服务器。 每种配置都具有不同的优势。

拉伸群集拓扑适用于需要带业务流程的自动故障转移的工作负荷，例如 Hyper-V 私有云群集和 SQL Server FCI。 此外还具有使用故障转移群集管理器的内置图形界面。 它通过永久保留采用存储空间的经典对称群集共享存储架构，即 SAN、iSCSI 和 RAID。 使用至少 2 个节点进行运行。

群集到群集拓扑使用两个独立群集，非常适用于需要手动故障转移的管理员，尤其是当预配第二个站点以进行灾难恢复以及无需每日使用的时候。 业务流程为手动操作。 与不同的拉伸群集存储空间直通可在此配置 （但需要注意的问题-请参阅存储副本常见问题和群集到群集文档）。 使用至少四个节点进行运行。 

服务器到服务器拓扑适用于运行无法组成群集的硬件的客户。 这种配置需要手动故障转移和业务流程。 非常适用于分支机构和中央数据中心之间的低成本部署，尤其是当使用异步复制的时候。 此配置通常可替代用于单主机灾难恢复方案的受 DFSR 保护的文件服务器的实例。

在所有的情况下，拓扑支持在物理硬件和虚拟机运行。 在虚拟机上运行时，基础虚拟机监控程序无需 Hyper-V；可以是 VMware、KVM 和 Xen 等。

存储副本还具有一个服务器到自我模式，可以在此模式中将复制指定到同一台计算机上的两个不同卷。

## <a name="FAQ18"></a> 存储副本是否支持重复数据删除？

是的数据 Deduplcation 支持存储副本。 源服务器上的卷上启用重复数据删除，并在复制期间在目标服务器接收已删除重复的卷的副本。

而应该*安装*源和目标服务器上重复数据删除 (请参阅[安装和启用重复数据删除](../data-deduplication/install-enable.md))，它不应对*启用*重复数据删除在目标服务器上。 存储副本允许将仅在源服务器上的写入。 因为重复数据删除可以写入到卷，则它应仅在源服务器上运行。 

## <a name="FAQ17"></a> 如何报告存储副本或本指南的问题？  
如需存储副本的技术援助，可以通过 [Microsoft TechNet 论坛](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview) 发布。 还可以将有关存储副本的疑问或本文档中出现的问题通过电子邮件发送至 srfeed@microsoft.com。 https://windowsserver.uservoice.com站点是设计更改请求的首选，因为它允许客户对你的想法提供支持和反馈。



## <a name="related-topics"></a>相关主题  
- [存储副本概述](storage-replica-overview.md) 
- [使用共享的存储拉伸群集复制](stretch-cluster-replication-using-shared-storage.md)  
- [服务器到服务器存储复制](server-to-server-storage-replication.md)  
- [群集到群集存储复制](cluster-to-cluster-storage-replication.md)  
- [存储副本：已知的问题](storage-replica-known-issues.md)  

## <a name="see-also"></a>请参阅  
- [存储概述](../storage.md)  
- [Windows Server 2016 中的存储空间直通](../storage-spaces/storage-spaces-direct-overview.md)  
