---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: 简化的 SMB 多通道和多 NIC 群集网络
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 7816016daae1d06568cd6149791a9a368d8602f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361111"
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>简化的 SMB 多通道和多 NIC 群集网络

> 适用于：Windows Server 2019、Windows Server 2016

简化的 SMB 多通道和多<abbr title="网络接口卡">具</abbr> 群集网络是一项功能，可在同一群集网络子网上使用多个 Nic，并自动启用 SMB 多通道。

简化的 SMB 多通道和多 NIC 群集网络具有以下优势：  
- 故障转移群集会自动识别使用同一交换机/相同子网的节点上的所有 Nic-无需其他配置。  
- 自动启用 SMB 多通道。  
- 只有 IPv6 链接本地（fe80） IP 地址资源的网络可在仅群集（专用）网络上识别。  
- 默认情况下，在每个群集访问点（CAP）网络名称（NN）上配置单个 IP 地址资源。  
- 当在同一子网上找到多个 Nic 时，群集验证不再发出警告消息。  

## <a name="requirements"></a>要求  
-   每台服务器多个 Nic，使用同一个交换机/子网。  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>如何利用多 NIC 群集网络和简化的 SMB 多通道  
本部分介绍如何利用新的多 NIC 群集网络和简化的 SMB 多通道功能。  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>为故障转移群集使用至少两个网络   
虽然很少见，但网络交换机可能会失败，但最好还是至少使用两个网络进行故障转移群集。 找到的所有网络都用于群集检测信号。 避免对故障转移群集使用单个网络，以避免单点故障。 理想情况下，群集中的节点之间应该有多个物理通信路径，并且无单点故障。  

用于故障转移群集 @ no__t 的两个网络的 @no__t 0Illustration  
**图 1：使用至少两个网络进行故障转移群集 @ no__t-0  

### <a name="use-multiple-nics-across-clusters"></a>跨群集使用多个 Nic  

在存储和存储工作负荷群集中使用多个 Nic 时，可实现简化的 SMB 多通道的最大优势。 这允许工作负荷群集（Hyper-v、SQL Server 故障转移群集实例、存储副本等）使用 SMB 多通道，从而更有效地使用网络。 在聚合（也称为非聚合）群集配置中，其中，横向扩展文件服务器群集用于存储 Hyper-v 或 SQL Server 故障转移群集实例群集的工作负荷数据，此网络通常称为 "北南部子网"/网络. 许多客户通过购买支持 RDMA 的 NIC 卡和交换机来最大限度地提高此网络的吞吐量。  

@no__t-南-南部 SMB 子网 @ no__t-1 的0Illustration  
**图 2:若要实现最大网络吞吐量，请在横向扩展文件服务器群集和 Hyper-v 或 SQL Server 故障转移群集实例群集上使用多个 Nic，该群集共享北南部子网 @ no__t-0  

在同一子网中使用多个 Nic 的两个群集 @no__t 0Screencap，以利用 SMB 多通道 @ no__t-1  
**图 3:两个群集（用于存储的横向扩展文件服务器、SQL Server @no__t 0Failover 群集实例 @ no__t-1FCI @ no__t-2 工作负载）都在同一子网中使用多个 Nic 来利用 SMB 多通道并获得更好的网络吞吐量。 @no__t 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>自动识别 IPv6 链接本地专用网络  
检测到具有多个 Nic 的专用（仅群集）网络时，群集将自动识别每个子网上每个 NIC 的 IPv6 链接本地（fe80） IP 地址。 这样可以节省管理员时间，因为他们不再需要手动配置 IPv6 链接本地（fe80） IP 地址资源。  

如果使用多个专用（仅限群集）网络，请检查 IPv6 路由配置以确保路由未配置为跨子网，因为这会降低网络性能。  

@no__t-故障转移群集管理器 UI @ no__t 中自动网络配置的0Screencap  
**图4：自动 IPv6 链接本地（fe80）地址资源配置 @ no__t-0  

## <a name="throughput-and-fault-tolerance"></a>吞吐量和容错  
Windows Server 2019 和 Windows Server 2016 会自动检测 NIC 功能，并将尝试在尽可能快的配置中使用每个 NIC。 使用 RSS 的 nic 和具有 RDMA 功能的 nic 都可以使用。 下表总结了使用这些技术时的利弊。 当使用多个支持 RDMA 的 Nic 时，实现最大吞吐量。 有关详细信息，请参阅[SMB Mutlichannel 的基础知识](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/)。

针对各种 NIC 配置的吞吐量和容错的0An 说明 @ no__t-1 @no__t  
**图5：各种 NIC conifigurations @ no__t 的吞吐量和容错   

## <a name="frequently-asked-questions"></a>常见问题解答  
**多 NIC 网络中是否有用于信号的 Nic？**  
    是。  

@no__t 是否将多 NIC 网络仅用于群集通信？还是只能将它用于客户端和群集通信？ **  
    任何一个配置都将起作用-所有群集网络角色将在多 NIC 网络上运行。  

**SMB 多通道还用于 CSV 和群集通信？**  
    是的，默认情况下，所有群集和 CSV 流量将使用可用的多 NIC 网络。 管理员可以使用故障转移群集 PowerShell cmdlet 或故障转移群集管理器 UI 来更改网络角色。  

**如何查看 SMB 多通道设置？**  
    使用**SMBServerConfiguration** Cmdlet 查找 EnableMultiChannel 属性的值。  

**群集公用属性是否在多 NIC 网络上 PlumbAllCrossSubnetRoutes？**  
     是。  

## <a name="see-also"></a>请参阅  
- [Windows Server 中故障转移群集的新增功能](whats-new-in-failover-clustering.md)  
