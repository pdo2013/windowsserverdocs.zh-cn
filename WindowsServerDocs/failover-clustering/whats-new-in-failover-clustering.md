---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: 什么是 Windows Server 中故障转移群集中的新增功能
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: dongill
author: JasonGerend
ms.author: jgerend
ms.date: 10/18/2018
ms.openlocfilehash: b4fa59aa62acba5c89f20c191da2c3c1b776b1ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884748"
---
# <a name="whats-new-in-failover-clustering"></a>故障转移群集中的新增功能

> 适用于：Windows Server 2019，Windows Server 2016 中，Windows Server （半年频道）

本主题介绍了在故障转移群集的 Windows Server 2019，Windows Server 2016 中的新功能和更改功能和 Windows Server 半年频道发布。

## <a name="whats-new-in-windows-server-2019"></a>Windows Server 2019 中的新增功能

- **群集设置**

    群集设置，可增加到超出群集的当前限制的单个软件定义数据中心 (SDDC) 解决方案中的服务器数目。 这通过对分组到群集 set--多个故障转移群集的松散耦合分组的多个群集实现： 计算、 存储和超聚合。
    使用群集集时，您可以移动联机虚拟机 （实时迁移） 在群集中的群集之间设置。

    有关详细信息，请参阅[群集集合](../storage/storage-spaces/cluster-sets.md)。

- **Azure 感知群集**

    故障转移群集现在自动检测时它们在 Azure IaaS 虚拟机中运行和优化配置以提供主动的故障转移和日志记录的 Azure 计划内的维护事件，以实现最高级别的可用性。 通过删除与群集名称的动态网络名称配置负载均衡器的需要，还简化部署。

- **跨域的群集迁移**

    故障转移群集现在可以动态移动从一个 Active Directory 域之间，从而简化了域合并，并允许创建由硬件合作伙伴和更高版本连接到客户的域的群集。
- **USB 见证服务器**

    现在可以使用简单的 USB 驱动器连接到网络交换机为确定群集的仲裁见证。 这将扩展文件共享见证，以支持任何 SMB2 合规的设备。

- **群集基础结构改进**

    默认值现在启用 CSV 缓存来提高虚拟机性能。 MSDTC 现在支持群集共享卷，以允许在存储空间直通上部署 MSDTC 工作负载，例如 SQL Server。 增强型逻辑可利用自我修复检测分区节点，以恢复节点的群集成员身份。 增强型群集网络路由检测和自我修复。

- **群集感知更新支持存储空间直通**

    现在集成了群集感知更新 (CAU)，并可感知存储空间直通，验证并确保了每个节点上数据重新同步完成。 群集感知更新检查更新，以仅在必要时以智能方式重新启动。 这样安排重新启动计划内维护群集中的所有服务器。

- **文件共享见证服务器增强功能**我们启用了文件共享见证在以下方案中使用： 
    - 找不到或极差由于远程位置，导致无法使用云见证服务器的 Internet 访问。 
    - 缺少的共享驱动器的磁盘见证。 这可能是存储空间直通超聚合配置，SQL Server 始终在可用性组 (AG)，或 * Exchange 数据库可用性组 (DAG)，其中没有任何使用共享的磁盘。 
    - 由于正在受 dmz 保护群集的域控制器连接的不足。 
    - 工作组或跨域群集为其存在是没有 Active Directory 群集名称对象 (CNO)。 了解有关在服务器和管理博客中的以下文章中的这些增强功能的详细信息：故障转移群集文件共享见证和 DFS。
    
    我们现在还显式阻止使用 DFS 命名空间共享作为位置。 添加文件共享见证到 DFS 共享可能会导致群集稳定性问题和永远不会支持此配置。 我们添加了逻辑来检测如果共享使用 DFS 命名空间，并且如果检测到 DFS 命名空间，则故障转移群集管理器阻止的见证服务器创建并显示有关不受支持的错误消息。
- **群集强化**

    通过群集共享卷和存储空间直通的服务器消息块 (SMB) 进行群集内通信现在可利用证书来提供最安全的平台。 这样，故障转移群集就可以不依赖于 NTLM 而运行，并实现了安全基线。
- **故障转移群集不再使用 NTLM 身份验证**

    故障转移群集不再使用 NTLM 身份验证。 而 Kerberos 和基于证书的身份验证是以独占方式使用的。 没有用户或部署工具，才能利用此安全增强功能所需的更改。 它还允许在具有已禁用 NTLM 的环境中部署的故障转移群集。 


## <a name="whats-new-in-windows-server-2016"></a>什么是 Windows Server 2016 中的新增功能

### <a name="BKMK_RollingUpgrade"></a>群集操作系统滚动升级

群集操作系统滚动升级使管理员能够升级群集节点的操作系统从 Windows Server 2012 R2 到较新版本而无需停止 HYPER-V 或横向扩展文件服务器工作负荷。 使用此功能可以避免服务级别协议 (SLA) 的停机时间损失。 

**这一更改增添了什么价值？**  

升级 HYPER-V 或横向扩展文件服务器群集从 Windows Server 2012 R2 到 Windows Server 2016 不再需要停机时间。 群集将继续工作在 Windows Server 2012 R2 级别之前在群集中节点的所有运行 Windows Server 2016。 群集功能级别升级到 Windows Server 2016 上，通过使用 Windows PowerShell cmdlt `Update-ClusterFunctionalLevel`。 

> [!WARNING]  
> -   更新群集功能级别后，您不能再返回到 Windows Server 2012 R2 群集功能级别。 
> -   直到`Update-ClusterFunctionalLevel`运行 cmdlet，该过程不可逆的并可以将 Windows Server 2012 R2 节点添加和可以删除 Windows Server 2016 节点。 

**工作原理的不同之处是什么？**  

HYPER-V 或横向扩展文件服务器故障转移群集可以现在轻松地将升级而无需任何停机时间，或者需要生成新的群集具有运行 Windows Server 2016 操作系统的节点。 使现有群集脱机并重新安装新操作系统中，对于每个节点，然后开始使用群集重新联机，涉及到迁移到 Windows Server 2012 R2 群集。 旧进程过于繁琐，而且所需的停机时间。 但是，在 Windows Server 2016 中，群集不会不需要进入脱机状态，任何时候。 

在阶段中升级群集的操作系统，如下所示适用于在群集中每个节点：  
-   节点是暂停和排除其上运行的所有虚拟机。 
-   虚拟机 （或其他群集工作负荷） 迁移到群集中的另一个节点。 
-   删除现有的操作系统，并执行清理安装在节点上的 Windows Server 2016 操作系统。 
-   运行 Windows Server 2016 操作系统的节点添加回群集。 
-   在此情况下，群集被认为运行在混合模式下，因为群集节点都在运行 Windows Server 2012 R2 或 Windows Server 2016。 
-   群集功能级别会停留在 Windows Server 2012 R2。 在此功能级别，Windows Server 2016 的会影响与以前版本的操作系统的兼容性的新功能将不可用。 
-   最终，所有节点将都升级到 Windows Server 2016。 
-   群集功能级别然后更改为使用 Windows PowerShell cmdlet 的 Windows Server 2016 `Update-ClusterFunctionalLevel`。 此时，您可以利用 Windows Server 2016 功能。 

有关详细信息，请参阅[群集操作系统滚动升级](cluster-operating-system-rolling-upgrade.md)。 

### <a name="BKMK_SR"></a>存储副本  
存储副本是实现存储不可知的新功能、 块级、 同步服务器或群集的灾难恢复以及扩展的站点之间故障转移群集之间的复制。 同步复制支持物理站点中的镜像数据和在崩溃时保持一致的卷，以确保文件系统级别的数据损失为零。 异步复制允许超出都市范围、可能存在数据损失的站点扩展。 

**这一更改增添了什么价值？**  

存储副本，可执行以下操作：  

-   为关键任务工作负荷的计划内和计划外中断提供单一供应商灾难恢复解决方案。 

-   使用具有广为赞誉的可靠性、可伸缩性和高性能的 SMB3 传输。 

-   将 Windows 故障转移群集扩展到都市距离。 

-   用于存储和聚类分析，如 HYPER-V、 存储副本，存储空间、 群集、 横向扩展文件服务器、 SMB3、 重复数据删除和 ReFS/NTFS 的端到端的 Microsoft 软件。 

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

使用云见证服务器作为故障转移群集仲裁见证服务器提供以下优势：  

-   利用 Microsoft Azure，无需第三个单独的数据中心。 

-   使用标准公开发布 Microsoft Azure Blob 存储而不在公有云中托管的 Vm 的额外维护开销。 

-   同一 Microsoft Azure 存储帐户可以用于多个群集 （每个群集的一个 blob 文件; 群集用作 blob 文件名称的唯一 id）。 

-   提供对存储帐户 （写入每个 blob 文件，仅当群集节点的状态发生更改后更新的 blob 文件非常小数据） 的持续成本非常低。 

有关详细信息，请参阅[部署云见证服务器的故障转移群集中](deploy-cloud-witness.md)。 

**工作原理的不同之处是什么？**  

此功能是 Windows Server 2016 的新增功能。 

### <a name="BKMK_VMs"></a>虚拟机复原性  
**计算复原**Windows Server 2016 包括增加的虚拟机计算复原，以帮助减少计算群集中的群集内通信问题，如下所示： 

-   **复原能力选项适用于虚拟机：** 现在可以配置在发生暂时性故障期间定义行为的虚拟机的虚拟机复原选项：  

    -   **复原级别：** 可以帮助您定义如何处理暂时性故障。 

    -   **复原能力段：** 可以帮助您定义多长时间的所有虚拟机都可以运行独立。 

-   **不正常节点的隔离区：** 不正常节点会被隔离，并且不再被允许加入群集。 这可以防止摇摆节点造成负面影响的其他节点和整个群集。 

有关详细信息用于虚拟机计算复原工作流和节点隔离设置控制如何将节点位于隔离或隔离区中，请参阅[Windows Server 2016 中的虚拟机计算复原](http://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx)。 

**存储复原能力**在 Windows Server 2016 中，虚拟机都出现的暂时性存储故障更具弹性。 改进的虚拟机复原性帮助保持在出现存储中断时的租户虚拟机的会话状态。 这被通过对存储基础结构问题的智能和快速虚拟机响应。 

当虚拟机将从其基础存储断开连接时，它将暂停并等待存储空间来恢复。 在暂停时，虚拟机将保留在其中运行的应用程序的上下文。 还原到其存储的虚拟机的连接后，虚拟机返回到其运行状态。 因此，租户计算机的会话状态将保留在恢复上。 

在 Windows Server 2016 中，虚拟机存储复原能力太感知和优化为来宾群集。 

### <a name="BKMK_Diagnostics"></a>故障转移群集中的诊断改进  
为了帮助诊断问题的故障转移群集，Windows Server 2016，包括以下：  

-   为使的群集日志文件 （例如时区信息和 DiagnosticVerbose 日志） 的多项增强功能是更轻松地解决故障转移群集问题。 有关详细信息，请参阅[Windows Server 2016 故障转移群集故障排除增强措施-群集日志](http://blogs.msdn.com/b/clustering/archive/2015/05/15/10614930.aspx)。 

-   转储的键入一个新**活动内存转储**，其中筛选出大多数分配给虚拟机的内存页，因此 memory.dmp 显著变小且更轻松地保存或复制。 有关详细信息，请参阅[Windows Server 2016 故障转移群集故障排除增强措施-Active 转储](http://blogs.msdn.com/b/clustering/archive/2015/05/18/10615526.aspx)。 

### <a name="BKMK_SiteAware"></a>站点感知故障转移群集  
Windows Server 2016 包括启用基于其物理位置 （站点） 的外延式群集中的组节点的站点感知故障转移群集。 群集站点感知改进了关键操作，例如故障转移行为、 位置策略、 节点和仲裁行为之间的检测信号的群集生命周期。 有关详细信息，请参阅[Windows Server 2016 中的站点感知故障转移群集](http://blogs.msdn.com/b/clustering/archive/2015/08/19/10636304.aspx)。 

### <a name="BKMK_multidomainclusters"></a>工作组和多域群集  
在 Windows Server 2012 R2 和早期版本中，仅可以加入同一个域的成员节点间创建群集。 Windows Server 2016 打破了这些障碍，并引入了创建故障转移群集的功能，且无需 Active Directory 依赖项。 现在可以按下列配置创建故障转移群集：  

-   **单域群集。** 已加入到同一个域的所有节点的群集。 

-   **多域群集。** 哪些是不同的域的成员节点的群集。 

-   **工作组群集。** 这是成员服务器的节点的群集 / 工作组 （未加入域）。 

有关详细信息，请参阅[Windows Server 2016 中的工作组和多域群集](http://blogs.msdn.com/b/clustering/archive/2015/08/17/10635825.aspx)  
### <a name="BKMK_VMLoadBalancing"></a>虚拟机负载平衡  
虚拟机负载平衡是一项新功能，方便的无缝负载平衡的虚拟机在群集中节点之间的故障转移群集。 处于过载状态的节点都被标识基于虚拟机内存和 CPU 使用率在节点上。 然后 （实时迁移） 移动虚拟机处于过载状态节点到节点的可用带宽 （如果适用）。 可以优化均衡入侵，以确保最佳群集性能和利用率。 Windows Server 2016 Technical Preview 中默认情况下启用负载平衡。 但是，负载平衡时，禁用 SCVMM 动态优化已启用。 

### <a name="BKMK_VMStartOrder"></a>虚拟机启动顺序  
虚拟机启动顺序是在群集中引入了开始顺序业务流程的虚拟机 （和所有组） 的故障转移群集的新功能。 虚拟机现在可以分组到的层，并启动顺序的依赖项可以创建不同层之间。 这可确保最重要的虚拟机 （例如域控制器或实用程序的虚拟机） 启动的第一个。 此外启动它们具有依赖关系的虚拟机之前，不会启动虚拟机。 

### <a name="BKMK_SMBMultiChannel"></a> 简化的 SMB 多通道和多 NIC 群集网络  
故障转移群集网络已不再限制为单个 NIC 每个子网 / 网络。 使用简化的 SMB 多通道和多 NIC 群集网络，网络配置是自动和子网上的每个 NIC 可以用于群集和工作负荷的流量。 此增强功能允许客户的 HYPER-V、 SQL Server 故障转移群集实例和其他 SMB 工作负荷最大化网络吞吐量。 

有关详细信息，请参阅[简体 SMB 多通道和多 NIC 群集网络](smb-multichannel.md)。

## <a name="see-also"></a>请参阅  
* [存储](../storage/storage.md)  
* [什么是 Windows Server 2016 中的存储中的新增功能](../storage/whats-new-in-storage.md)  
