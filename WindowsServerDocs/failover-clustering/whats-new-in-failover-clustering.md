---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: Windows Server 中故障转移群集的新增功能
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: dongill
author: JasonGerend
ms.author: jgerend
ms.date: 10/18/2018
ms.openlocfilehash: 26417f0fdbe2c4c8c374b3a1b8955c6297865397
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360833"
---
# <a name="whats-new-in-failover-clustering"></a>故障转移群集中的新增功能

> 适用于：Windows Server 2019、Windows Server 2016

本主题介绍 Windows Server 2019 和 Windows Server 2016 的故障转移群集中的新增功能和更改的功能。

## <a name="whats-new-in-windows-server-2019"></a>Windows Server 2019 中的新增功能

- **群集集**

    群集集使你可以增加单个软件定义的数据中心（SDDC）解决方案中超出群集当前限制的服务器数。 这是通过将多个群集分组到一个群集集中来实现的，这是多个故障转移群集的松耦合分组：计算、存储和超聚合。
    利用群集集，可以在群集集中的群集之间移动联机虚拟机（实时迁移）。

    有关详细信息，请参阅[群集集](../storage/storage-spaces/cluster-sets.md)。

- **Azure 感知群集**

    故障转移群集现在会自动检测到它们在 Azure IaaS 虚拟机中运行的时间，并对配置进行优化，以便为 Azure 计划内维护事件提供主动故障转移和日志记录，以实现最高级别的可用性。 还可以通过无需使用群集名称的动态网络名称配置负载均衡器来简化部署。

- **跨域群集迁移**

    故障转移群集现在可以动态地从一个 Active Directory 域移到另一个域，从而简化了域合并，并允许硬件伙伴创建群集并稍后联接到客户的域。
- **USB 见证**

    你现在可以使用连接到网络交换机的简单 USB 驱动器，作为确定群集仲裁的见证服务器。 这会扩展文件共享见证，以支持任何 SMB2 兼容设备。

- **群集基础结构改进**

    默认情况下，CSV 缓存已启用以提高虚拟机性能。 MSDTC 现在支持群集共享卷，以允许在存储空间直通上部署 MSDTC 工作负载，例如 SQL Server。 增强型逻辑可利用自我修复检测分区节点，以恢复节点的群集成员身份。 增强型群集网络路由检测和自我修复。

- **群集感知更新支持存储空间直通**

    现在集成了群集感知更新 (CAU)，并可感知存储空间直通，验证并确保了每个节点上数据重新同步完成。 群集感知更新会检查更新以在必要时智能地重新启动。 这样就可以协调群集中所有服务器的重新启动，以便进行计划内维护。

- **文件共享见证增强功能**在以下情况下，我们启用了文件共享见证的使用： 
  - 由于远程位置不存在或极其糟糕的 Internet 访问，因此无法使用云见证。 
  - 磁盘见证缺少共享驱动器。 这可能是存储空间直通超聚合配置、SQL Server Always On 可用性组（AG）或 * Exchange 数据库可用性组（DAG），它们都不使用共享磁盘。 
  - 由于群集在 DMZ 后面，缺少域控制器连接。 
  - 没有 Active Directory 群集名称对象（CNO）的工作组或跨域群集。 有关这些增强功能的详细信息，请参阅服务器 & 管理博客中的以下文章：故障转移群集文件共享见证和 DFS。
    
    现在，我们还显式阻止使用 DFS 命名空间共享作为位置。 向 DFS 共享添加文件共享见证会导致群集出现稳定性问题，且从未支持此配置。 我们添加了逻辑来检测某个共享是否使用 DFS 命名空间，如果检测到 DFS 命名空间，故障转移群集管理器会阻止创建见证服务器，并显示有关不受支持的错误消息。
- **群集强化**

    通过群集共享卷和存储空间直通的服务器消息块 (SMB) 进行群集内通信现在可利用证书来提供最安全的平台。 这样，故障转移群集就可以不依赖于 NTLM 而运行，并实现了安全基线。
- **故障转移群集不再使用 NTLM 身份验证**

    故障转移群集不再使用 NTLM 身份验证。 相反，使用 Kerberos 和基于证书的身份验证。 用户或部署工具不需要进行任何更改即可利用此安全增强功能。 它还允许在禁用了 NTLM 的环境中部署故障转移群集。 


## <a name="whats-new-in-windows-server-2016"></a>Windows Server 2016 中的新增功能

### <a name="BKMK_RollingUpgrade"></a>群集操作系统滚动升级

群集操作系统滚动升级允许管理员将群集节点的操作系统从 Windows Server 2012 R2 升级到较新版本，而无需停止 Hyper-v 或横向扩展文件服务器工作负荷。 使用此功能可以避免服务级别协议 (SLA) 的停机时间损失。 

**这一更改增添了什么价值？**  

将 Hyper-v 或横向扩展文件服务器群集从 Windows Server 2012 R2 升级到 Windows Server 2016 不再需要停机。 在群集中的所有节点都运行 Windows Server 2016 之前，群集将继续在 Windows Server 2012 R2 级别上工作。 通过使用 Windows PowerShell cmdlt `Update-ClusterFunctionalLevel` 将群集功能级别升级到 Windows Server 2016。 

> [!WARNING]  
> -   更新群集功能级别后，无法返回到 Windows Server 2012 R2 群集功能级别。 
> -   在运行 `Update-ClusterFunctionalLevel` cmdlet 之前，该过程是可逆的，可以添加 Windows Server 2012 R2 节点并删除 Windows Server 2016 节点。 

**工作原理的不同之处是什么？**  

Hyper-v 或横向扩展文件服务器故障转移群集现在可以轻松升级，无需任何停机时间，也无需使用运行 Windows Server 2016 操作系统的节点构建新群集。 将群集迁移到 Windows Server 2012 R2 涉及到使现有群集脱机并为每个节点重新安装新操作系统，然后使群集重新联机。 旧进程很繁琐，需要停机。 但是，在 Windows Server 2016 中，群集在任何时候都无需脱机。 

对于群集中的每个节点，分阶段升级的群集操作系统如下：  
-   节点已暂停，并已将其上运行的所有虚拟机排出。 
-   虚拟机（或其他群集工作负载）迁移到群集中的其他节点。 
-   系统会删除现有操作系统，并在节点上执行 Windows Server 2016 操作系统的干净安装。 
-   运行 Windows Server 2016 操作系统的节点会重新添加到群集中。 
-   此时，群集被称为在混合模式下运行，因为群集节点正在运行 Windows Server 2012 R2 或 Windows Server 2016。 
-   群集功能级别保留在 Windows Server 2012 R2 上。 在此功能级别，Windows Server 2016 中的新功能影响与早期版本的操作系统的兼容性。 
-   最终，所有节点都将升级到 Windows Server 2016。 
-   然后，使用 Windows PowerShell cmdlet `Update-ClusterFunctionalLevel` 将群集功能级别更改为 Windows Server 2016。 此时，你可以利用 Windows Server 2016 功能。 

有关详细信息，请参阅[群集操作系统滚动升级](cluster-operating-system-rolling-upgrade.md)。 

### <a name="BKMK_SR"></a>存储副本  
存储副本是一项新功能，可在服务器或群集之间实现存储不可知的块级同步复制以进行灾难恢复，以及在站点之间拉伸故障转移群集。 同步复制支持物理站点中的镜像数据和在崩溃时保持一致的卷，以确保文件系统级别的数据损失为零。 异步复制允许超出都市范围、可能存在数据损失的站点扩展。 

**这一更改增添了什么价值？**  

存储副本使你能够执行以下操作：  

-   为关键任务工作负荷的计划内和计划外中断提供单一供应商灾难恢复解决方案。 

-   使用具有广为赞誉的可靠性、可伸缩性和高性能的 SMB3 传输。 

-   将 Windows 故障转移群集扩展到都市距离。 

-   使用 Microsoft 软件端到端进行存储和群集，例如 Hyper-v、存储副本、存储空间、群集、横向扩展文件服务器、SMB3、重复数据删除和 ReFS/NTFS。 

-   可帮助降低成本和复杂性，如下所示：  

    -   与硬件无关，对特定存储配置（例如 DAS 或 SAN）没有要求。 

    -   允许使用商品存储和网络技术。 

    -   通过故障转移群集管理器轻松对单独的节点和群集进行图形管理的功能。 

    -   包括通过 Windows PowerShell 的全面、大型的脚本选项。 

-   有助于减少停机时间，提高可靠性和 Windows 内部的工作效率。 

-   提供支持能力、性能度量标准和诊断功能。 

有关详细信息，请参阅 [Windows Server 2016 中的存储副本](../storage/storage-replica/storage-replica-overview.md)。 


### <a name="BKMK_CloudWitness"></a>云见证  
云见证是 Windows Server 2016 中一种新型的故障转移群集仲裁见证，它将 Microsoft Azure 作为仲裁点。 与其他仲裁见证一样，云见证获取投票，并可以参与仲裁计算。 可以使用“配置群集仲裁向导”将云见证配置为仲裁见证。 

**这一更改增添了什么价值？**  

使用云见证作为故障转移群集仲裁见证具有以下优势：  

-   利用 Microsoft Azure，无需使用第三个单独的数据中心。 

-   使用标准公开可用 Microsoft Azure Blob 存储，消除了在公有云中托管的 Vm 额外的维护开销。 

-   同一个 Microsoft Azure 存储帐户可用于多个群集（每个群集一个 blob 文件; 用作 blob 文件名的群集唯一 id）。 

-   为存储帐户提供了很低的成本（每个 blob 文件写入非常小的数据，仅当群集节点的状态发生更改时才更新 blob 文件）。 

有关详细信息，请参阅为[故障转移群集部署云见证](deploy-cloud-witness.md)。 

**工作原理的不同之处是什么？**  

此功能是 Windows Server 2016 的新增功能。 

### <a name="BKMK_VMs"></a>虚拟机复原  
**计算复原能力**Windows Server 2016 包括增加的虚拟机计算弹性，有助于减少计算群集中的群集内通信问题，如下所示： 

-   **可用于虚拟机的复原选项：** 你现在可以配置虚拟机复原选项，这些选项定义了在暂时性故障期间虚拟机的行为：  

    -   **复原级别：** 帮助您定义如何处理暂时性故障。 

    -   **复原周期：** 帮助您定义允许所有虚拟机独立运行的时间长度。 

-   **不正常节点的隔离：** 不正常的节点会被隔离，并且不再允许加入群集。 这可以防止稳定节点负面影响其他节点和整个群集。 

有关控制如何将节点置于隔离或隔离中的详细信息，请参阅[Windows Server 2016 中的虚拟机计算复原](http://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx)功能。 

**存储复原**在 Windows Server 2016 中，虚拟机能够更灵活地防御暂时性存储故障。 改善了虚拟机复原能力，有助于在发生存储中断时保留租户虚拟机会话状态。 这是通过智能和快速虚拟机对存储基础结构问题的响应实现的。 

当虚拟机从其基础存储断开连接时，它会暂停并等待存储恢复。 暂停时，虚拟机将保留正在其中运行的应用程序的上下文。 当虚拟机与存储的连接恢复时，虚拟机将恢复为其运行状态。 因此，租户计算机的会话状态会在恢复时保留。 

在 Windows Server 2016 中，虚拟机存储复原功能也适用于来宾群集。 

### <a name="BKMK_Diagnostics"></a>故障转移群集中的诊断改进  
为了帮助诊断故障转移群集的问题，Windows Server 2016 包括以下各项：  

-   群集日志文件（如时区信息和 DiagnosticVerbose 日志）的几项增强功能，可更轻松地排查故障转移群集问题。 有关详细信息，请参阅[Windows Server 2016 故障转移群集故障排除增强-群集日志](http://blogs.msdn.com/b/clustering/archive/2015/05/15/10614930.aspx)。 

-   新的转储类型为**活动内存转储**，它会筛选出分配给虚拟机的大部分内存页面，从而使内存的更小、更易于保存或复制。 有关详细信息，请参阅[Windows Server 2016 故障转移群集故障排除增强-活动转储](http://blogs.msdn.com/b/clustering/archive/2015/05/18/10615526.aspx)。 

### <a name="BKMK_SiteAware"></a>站点感知故障转移群集  
Windows Server 2016 包括站点感知故障转移群集，这些群集基于其物理位置（站点）启用延伸群集中的组节点。 群集站点感知改进了群集生命周期内的关键操作，例如故障转移行为、位置策略、节点之间的检测信号和仲裁行为。 有关详细信息，请参阅[Windows Server 2016 中的站点感知故障转移群集](http://blogs.msdn.com/b/clustering/archive/2015/08/19/10636304.aspx)。 

### <a name="BKMK_multidomainclusters"></a>工作组和多域群集  
在 Windows Server 2012 R2 和早期版本中，只能在联接到同一个域的成员节点之间创建群集。 Windows Server 2016 打破了这些障碍，并引入了创建故障转移群集的功能，且无需 Active Directory 依赖项。 你现在可以在以下配置中创建故障转移群集：  

-   **单域群集。** 将所有节点加入到同一个域的群集。 

-   **多域群集。** 具有属于不同域成员的节点的群集。 

-   **工作组群集。** 节点是成员服务器/工作组的群集（未加入域）。 

有关详细信息，请参阅[Windows Server 2016 中的工作组和多域群集](http://blogs.msdn.com/b/clustering/archive/2015/08/17/10635825.aspx)  
### <a name="BKMK_VMLoadBalancing"></a>虚拟机负载平衡  
虚拟机负载平衡是故障转移群集中的一项新功能，用于简化群集中各个节点上的虚拟机的无缝负载平衡。 基于节点上的虚拟机内存和 CPU 使用率标识过度提交的节点。 然后，将虚拟机从已过度提交的节点移动（实时迁移）到具有可用带宽的节点（如果适用）。 可以优化平衡的做法，以确保群集的最佳性能和利用率。 默认情况下，在 Windows Server 2016 Technical Preview 中启用负载平衡。 但是，启用 SCVMM 动态优化后，将禁用负载平衡。 

### <a name="BKMK_VMStartOrder"></a>虚拟机启动顺序  
虚拟机启动顺序是故障转移群集中的一项新功能，用于为群集中的虚拟机（和所有组）引入开始订单业务流程。 现在可将虚拟机分组为层，并且可以在不同的层之间创建启动顺序依赖关系。 这可确保先启动最重要的虚拟机（如域控制器或实用工具虚拟机）。 虚拟机在依赖的虚拟机也已启动之前，不会启动。 

### <a name="BKMK_SMBMultiChannel"></a>简化的 SMB 多通道和多 NIC 群集网络  
故障转移群集网络不再限制为每个子网/网络单个 NIC。 使用简化的 SMB 多通道和多 NIC 群集网络，网络配置是自动的，子网上的每个 NIC 都可用于群集和工作负载流量。 此增强功能使客户能够最大程度地提高 Hyper-v、SQL Server 故障转移群集实例和其他 SMB 工作负载的网络吞吐量。 

有关详细信息，请参阅[简化的 SMB 多通道和多 NIC 群集网络](smb-multichannel.md)。

## <a name="see-also"></a>请参阅  
* [存储](../storage/storage.md)  
* [Windows Server 2016 中的存储的新增功能](../storage/whats-new-in-storage.md)  
