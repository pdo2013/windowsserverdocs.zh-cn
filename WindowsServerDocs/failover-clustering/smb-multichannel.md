---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: 简化的 SMB 多通道和多 NIC 群集网络
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 45d8364adf9d3db24a8e6d8f7bc91178ce7d1551
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881128"
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>简化的 SMB 多通道和多 NIC 群集网络

> 适用于：Windows 服务器 （半年频道），Windows Server 2016

简化的 SMB 多通道和多-<abbr title="网络接口卡">NIC</abbr>群集网络是允许使用多个 Nic 上的相同的群集网络子网，并自动启用 SMB 多通道的 Windows Server 2016 中的新功能。  

简化的 SMB 多通道和多 NIC 群集网络提供以下优势：  
- 故障转移群集会自动识别正在使用相同的交换机的节点上的所有 Nic / 相同子网-无需额外配置。  
- 自动启用 SMB 多通道。  
- 仅限群集的 （专用） 网络上识别网络只有 IPv6 链接本地 (fe80) IP 地址资源。  
- 默认情况下在每个群集的访问点 (CAP) 网络名称 (NN) 配置单个 IP 地址资源。  
- 当多个 Nic 位于相同子网上时，群集验证不再发出警告消息。  

## <a name="requirements"></a>要求  
-   每个服务器，使用相同的交换机的多个 Nic / 子网。  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>如何充分利用多 NIC 群集网络和简化的 SMB 多通道  
本部分介绍如何利用 Windows Server 2016 中的新的多 NIC 群集网络和简化的 SMB 多通道功能。  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>使用故障转移群集的至少两个网络   
虽然很少见，但可能会失败的网络交换机-它仍然最佳做法是使用至少两个网络故障转移群集。 找到的所有网络都用于群集检测信号。 避免使用故障转移群集的单个网络，以避免单点故障。 理想情况下，应该有多个物理通信路径在群集中的节点之间任何单点故障。  

![有关故障转移群集的两个网络图示](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)  
**图 1：使用故障转移群集的至少两个网络**  

### <a name="use-multiple-nics-across-clusters"></a>跨群集使用多个 Nic  

简化的 SMB 多通道的最大的好处是跨群集的存储和存储工作负荷群集中使用多个 Nic 时实现的。 这样更有效地使用网络中的工作负荷群集 （HYPER-V、 SQL Server 故障转移群集实例，存储副本等） 使用 SMB 多通道和结果。 在聚合 （也称为非聚合） 群集的配置用于存储的 HYPER-V 工作负荷数据的横向扩展文件服务器群集或 SQL Server 故障转移群集实例的群集，此网络通常称为"北-南子网"位置/网络. 许多客户通过投资 RDMA 支持的 NIC 卡和交换机中的此网络吞吐量最大化。  

![北-南 SMB 子网的图示](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)  
**图 2:若要实现最大网络吞吐量，使用多个 Nic 上的横向扩展文件服务器群集和 HYPER-V 或 SQL Server 故障转移群集实例群集-共享北-南子网**  

![两个群集位于同一子网使用多个 Nic 利用 SMB 多通道的方屏幕截图](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png)  
**图 3:两个群集 (横向扩展文件服务器存储、 SQL Server<abbr title="故障转移群集实例">FCI</abbr>工作负荷) 都使用相同的子网中的多个 Nic 来利用 SMB 多通道和实现更好的网络吞吐量。** 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>自动识别 IPv6 链接本地专用网络  
检测到具有多个 Nic 的专用 （仅限群集） 网络后，群集会自动识别 IPv6 链接本地 (fe80) IP 地址的每个 NIC 上的每个子网。 因为它们不再必须手动配置 IPv6 链接本地 (fe80) IP 地址资源此节省管理员时间。  

使用多个专用 （仅限群集） 网络时，检查以确保该路由未配置为跨子网，因为这会降低网络性能的 IPv6 路由配置。  

![自动网络配置故障转移群集管理器 UI 中的方屏幕截图](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)  
**图 4:自动 IPv6 链接本地 (fe80) 地址资源配置**  

## <a name="throughput-and-fault-tolerance"></a>吞吐量和容错能力  
Windows Server 2016 会自动检测 NIC 功能，并将尝试使用最快的可能配置中的每个 NIC。 已成组 Nic，使用 RSS 的 Nic 和 Nic 具有 RDMA 功能都可用。 下表总结了权衡时使用这些技术。 使用支持的多个 RDMA Nic 时，才能够达到最大吞吐量。 有关详细信息，请参阅[SMB Mutlichannel 的基础知识](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/)。

![举例说明了各种 NIC 配置吞吐量和容错能力](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)  
**图 5:有关各种 NIC conifigurations 吞吐量和容错能力**   

## <a name="frequently-asked-questions"></a>常见问题解答  
**多 NIC 网络中的所有 Nic 的都用途是群集信号？**  
    是。  

**多 NIC 网络是否可用于群集的通信或者，可它仅用于客户端和群集通信？**  
    可以配置将起作用-所有群集网络角色将在多个 NIC 的网络上都工作。  

**SMB 多通道还用于 CSV 和群集流量？**  
    是的默认情况下所有群集和 CSV 流量将都使用可用的多个 NIC 的网络。 管理员可以使用故障转移群集 PowerShell cmdlet 或故障转移群集管理器 UI 更改网络角色。  

**如何看到 SMB 多通道设置？**  
    使用**Get SMBServerConfiguration** cmdlet，寻找 EnableMultiChannel 属性的值。  

**多 NIC 网络上是群集公用属性，PlumbAllCrossSubnetRoutes 考虑？**  
     是。  

## <a name="see-also"></a>请参阅  
- [什么是 Windows Server 中故障转移群集中的新增功能](whats-new-in-failover-clustering.md)  
