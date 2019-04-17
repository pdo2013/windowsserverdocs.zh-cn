---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: "在故障转移群集 Windows Server 中的新增功能"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: dongill
author: JasonGerend
ms.author: jgerend
ms.date: 10/11/2016
ms.openlocfilehash: a4330f62095e13f2f4736f15924ed31fb4893e7a
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="whats-new-in-failover-clustering-in-windows-server-2016"></a>在故障转移群集在 Windows Server 2016 的新增功能
> 适用于：Windows Server（半年通道）、Windows Server 2016

本主题介绍了在故障转移群集 Windows Server 2016 中的新的和更改功能。  

## <a name="BKMK_RollingUpgrade"></a>滚动升级群集操作系统  
在故障转移群集、群集操作系统推出升级，一项新功能允许管理员升级的群集节点操作系统从 Windows Server 2012 R2 对 Windows Server 2016 而无需停止 Hyper-V 或规模文件服务器工作负载。 使用此功能，可以避免停机处罚防御服务级别协议 (SLA)。  

**此更改添加哪些值？**  

升级 Hyper-V 或规模文件服务器的群集运行 Windows Server 2012 r2 对 Windows Server 2016 不再需要宕机。 群集将继续运行 Windows Server 2012 R2 级别，直到的所有中的群集节点运行的 Windows Server 2016。 通过使用 Windows PowerShell cmdlt 群集功能级别升级到 Windows Server 2016 `Update-ClusterFunctionalLevel`。  

> [!WARNING]  
> -   更新群集功能级别后，你不能回退到 Windows Server 2012 R2 群集功能级别。  
> -   直到`Update-ClusterFunctionalLevel`cmdlet 运行时，该过程不可逆，可以添加 Windows Server 2012 R2 节点并可以删除 Windows Server 2016 节点。  

**内容的工作方式不同？**  

Hyper-V 或规模文件服务器故障转移群集可以现在轻松升级而无需任何停机或需要生成新的群集与运行 Windows Server 2016 的操作系统的节点。 对 Windows Server 2012 R2 的迁移群集涉及将现有群集离线和重新安装每个节点，新的操作系统，然后将返回到联机群集。 旧过程已繁琐和所需宕机。 但是，Windows Server 2016，群集不必转到离线在任意点。  

升级分阶段群集操作系统中的群集每个节点有如下：  
-   节点是暂停，并且它正在运行的所有虚拟机的耗尽。  
-   虚拟机（或其他群集运行工作负载）迁移到其他 cluster.The 虚拟机迁移到另一台群集节点。  
-   在删除现有的操作系统，并执行 Windows Server 2016 操作系统节点上的干净安装。  
-   运行 Windows Server 2016 的操作系统节点回群集添加。  
-   此时，群集称为型混合运行因为群集节点正在运行 Windows Server 2012 R2 或 Windows Server 2016。  
-   Windows Server 2012 R2 上一直群集功能级别。 在此功能的级别，Windows Server 2016 影响与以前版本的操作系统的兼容性的新功能将不提供。  
-   最终，所有节点都升级到 Windows Server 2016。  
-   群集功能级别然后更改为使用 Windows PowerShell cmdlet 的 Windows Server 2016 `Update-ClusterFunctionalLevel`。 此时，你可以充分利用 Windows Server 2016 功能。  

有关详细信息，请参阅[群集操作系统推出升级](cluster-operating-system-rolling-upgrade.md)。  

## <a name="BKMK_SR"></a>存储副本  
存储副本是一种新的功能，用于存储无关、服务器或灾难恢复以及张开故障转移群集的站点之间的群集之间的阻止级别、同步复制。 同步复制使镜像的物理崩溃一致的卷，以确保丢失数据的文件系统级别的站点中的数据。 异步复制允许有导致数据丢失大都市范围之外的站点扩展。  

**此更改添加哪些值？**  

存储副本使您能够执行以下操作：  

-   提供一次供应商灾难恢复解决方案计划和非计划不可用的使命的代表关键工作负载。  

-   使用 SMB3 传输成熟的可靠性、可扩展性和性能。  

-   延长 Windows 故障转移群集到本土距离。  

-   使用 Microsoft 软件端到端存储和群集、Hyper-V、存储副本、存储空间、群集、规模文件服务器、SMB3 数据消除和 ReFS/NTFS 等。  

-   帮助降低成本和复杂性，如下所示：  

    -   是硬件限，而不需要像 DAS 或 SAN 特定存储配置。  

    -   允许商品存储和网络的技术。  

    -   有关个别节点和群集故障转移群集管理器通过图形管理功能简单。  

    -   包含通过 Windows PowerShell 全面、大规模脚本的选项。  

-   有助于减少停机，并增加可靠性和内置于 Windows 的工作效率。  

-   提供性、性能指标和诊断功能。  

有关详细信息，请参阅[存储在 Windows Server 2016 的副本](../storage/storage-replica/storage-replica-overview.md)。  


## <a name="BKMK_CloudWitness"></a>云见证  
云见证是一种新故障转移群集仲裁见证利用作为仲裁点的 Microsoft Azure 的 Windows Server 2016。 云见证，如任何其他仲裁见证获取投票，并且可以参与仲裁计算。 你可以将云见证配置为使用配置群集仲裁向导仲裁见证。  

**此更改添加哪些值？**  

作为故障转移群集使用云见证仲裁见证提供以下优势：  

-   利用 Microsoft Azure 和不再需要单独的第三个数据中心。  

-   在公共云使用它消除了虚拟机的功能的额外维护开销公开可用 Microsoft Azure 斑点存储托管的标准。  

-   相同的 Microsoft Azure 存储帐户可用于多个群集（群集每一个斑点文件; 群集用作斑点文件名称的唯一 id）。  

-   提供了非常低成本上转到存储帐户（编写每个斑点文件仅当群集节点状态发生更改后，更新斑点文件很小数据）。  

有关详细信息，请参阅[部署云见证为故障转移群集](deploy-cloud-witness.md)。  

**内容的工作方式不同？**  

此功能是 Windows Server 2016 中的新增功能。  

## <a name="BKMK_VMs"></a>虚拟机复原  
**计算复原**Windows Server 2016 包含增加的虚拟机计算复原，以帮助减少群集内部通信中计算群集的问题，如下所示： 

-   **适用于虚拟机复原选项：**你现在可以配置期间瞬时失败定义行为虚拟机的虚拟机复原选项：  

    -   **复原级别：**可帮助你定义如何处理暂时失败的原因。  

    -   **复原期限：**可帮助你定义多久允许独立运行所有虚拟机。  

-   **隔离的节点不正常：**不正常节点隔离和不再允许加入群集。 这可以防止扇动翅膀节点其他节点和整个群集造成负面影响。  

有关详细信息虚拟机计算复原工作流程和节点隔离控制的设置，你节点如何放入隔离或隔离区，请参阅[虚拟机 Windows Server 2016 中的计算复原](http://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx)。  

**存储复原**虚拟机 Windows Server 2016 中，有更复原来瞬时存储失败情况。 改进了的虚拟机复原有助于保留存储中断时租户虚拟机会话状态。 这被通过智能和快速虚拟机响应存储基础结构的问题。  

从虚拟机断开连接时这与预期基础存储，它会暂停，并等待存储恢复。 在暂停，虚拟机可以保留在它正在运行的应用程序的上下文。 还原到其存储虚拟机的连接后，虚拟机返回到运行状态。 因此，在恢复保留租户计算机会话状态。  

Windows Server 2016，虚拟机存储复原也感知和来宾群集的优化。  

## <a name="BKMK_Diagnostics"></a>在故障转移群集诊断改进  
若要帮助诊断故障转移群集的问题，Windows Server 2016 包括：  

-   使的群集日志文件（如时区信息和 DiagnosticVerbose 日志）的多个增强功能很容易排除故障转移群集的问题。 有关详细信息，请参阅[Windows Server 2016 故障转移群集故障排除增强的群集日志](http://blogs.msdn.com/b/clustering/archive/2015/05/15/10614930.aspx)。  

-   一个新转储键入的**活动内存转储**，其中筛选出大多数分配给虚拟机的内存页和更较小、保存或复制更容易，因此使得 memory.dmp。  有关详细信息，请参阅[Windows Server 2016 故障转移群集故障排除增强的活动转储](http://blogs.msdn.com/b/clustering/archive/2015/05/18/10615526.aspx)。  

## <a name="BKMK_SiteAware"></a>站点感知故障转移群集  
Windows Server 2016 包括启用组节点拉伸群集根据他们的物理位置（站点）中的站点-感知故障转移群集。 群集的站点识别增强了主要操作，如故障转移行为、放置策略、检测信号之间节点，然后仲裁行为群集生命周期。 有关详细信息，请参阅[Windows Server 2016 中的网站了解故障转移群集](http://blogs.msdn.com/b/clustering/archive/2015/08/19/10636304.aspx)。  

## <a name="BKMK_multidomainclusters"></a>工作组和多域群集  
在 Windows Server 2012 R2 和以前的版本中，只可以成员节点到同一个域加入之间创建群集。   Windows Server 2016 这些障碍，并且引入了创建不 Active Directory 依赖项所执行的情况下进行故障转移群集的功能。 现在，你可以采用以下配置创建故障转移群集：  

-   **单域群集。** 使用所有节点到同一个域加入群集。  

-   **多域群集。** 群集节点它不同的域的成员。  

-   **工作组群集。** 与节点它成员服务器群集 / 工作组（不能加入域）。  

有关详细信息，请参阅[工作组和多域群集在 Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/17/10635825.aspx)  
## <a name="BKMK_VMLoadBalancing"></a>虚拟机负载平衡  
虚拟机负载平衡是故障转移群集便于无缝负载平衡虚拟机的跨中的群集节点中的新增功能。 根据虚拟机内存和节点上的 CPU 使用率标识过度提交的节点。 然后移虚拟机（实时迁移）从过度提交节点到具有（如果适用）可用带宽节点。 可以关注平衡，快速以确保获得最佳的群集性能和使用率。  负载平衡默认 Windows 服务器 2016 Technical Preview。 但是，负载平衡处于禁用状态启用 SCVMM 动态优化。  

## <a name="BKMK_VMStartOrder"></a>虚拟机开始订单  
虚拟机开始订单是在群集故障转移群集，其中引入了虚拟机的启动顺序业务流程（以及所有组）中的新增功能。 现在可以按层，分组虚拟机和相关性开始订单可以创建其他层之间。 这将确保最重要的虚拟机（如域控制器或实用程序虚拟机）会启动第一。 不启动虚拟机，直到他们具有依赖关系虚拟机还会启动。  

## <a name="BKMK_SMBMultiChannel"></a>简化的 SMB 多通道和多 NIC 群集网络  
不再子网每一个 nic 有限故障转移群集网络 / 网络。 简化 SMB 多通道和多 NIC 群集网络，网络配置自动并上子网每个网络接口卡可用于群集和工作负载的交通。 此增强使客户能够最大化 Hyper-V、SQL Server 故障转移群集实例和其他 SMB 工作负载的网络吞吐量。  

有关详细信息，请参阅[简体 SMB 多通道和多 NIC 群集网络](smb-multichannel.md)。

## <a name="see-also"></a>请参阅  
* [存储](../storage/storage.md)  
* [在存储中存储在 Windows Server 2016 的新增功能](../storage/whats-new-in-storage.md)  
