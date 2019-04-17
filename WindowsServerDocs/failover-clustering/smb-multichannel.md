---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: "简化的 SMB 多通道和多 NIC 群集网络"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 45d8364adf9d3db24a8e6d8f7bc91178ce7d1551
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>简化的 SMB 多通道和多 NIC 群集网络

> 适用于：Windows Server（半年通道）、Windows Server 2016

简化 SMB 多通道和多个<abbr title="网络接口卡">NIC</abbr>群集网络都是中，将能够使用相同的群集网上的多个 Nic 自动使 SMB 多通道 Windows Server 2016 的新增功能。  

简化 SMB 多通道和多 NIC 群集网络提供以下优势：  
- 自动故障转移群集上使用相同的切换的节点识别所有 Nic / 相同子网-无需额外的配置。  
- 自动 SMB 多通道处于启用状态。  
- 只需 IPv6 链接本地 (fe80) 的 IP 地址资源的网络被识别仅群集 （专用） 网络。  
- 默认情况下在每个群集接入点 （笔帽） 网络名称 (NN) 配置单个资源 IP 地址。  
- 多个 Nic 发现相同子网时，不会再群集验证发出警告消息。  

## <a name="requirements"></a>要求  
-   使用同一个切换的多个 Nic 服务器上，每月子网。  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>如何充分利用多 NIC 群集网络并简化的 SMB 多通道  
此部分中介绍了如何在 Windows Server 2016 充分利用新多 NIC 群集网络并简化的 SMB 多通道功能。  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>用于在至少两个网络故障转移群集   
虽然很少，可能会失败网络交换机的成员，用于在至少两个网络故障转移群集仍最佳做法是。 找到的所有网络都适用于群集检测信号。 避免使用单个为故障转移群集网络，以避免失败单点。 理想的情况下，应该有多个物理通信路径中群集节点和无故障单点之间。  

![故障转移群集的两个网络的的图示](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)  
**图 1： 用于在至少两个网络故障转移群集**  

### <a name="use-multiple-nics-across-clusters"></a>跨群集使用多个 Nic  

简化 SMB 多通道的最大好处，跨群集-存储和存储工作负载群集中使用多个 Nic 时实现。 这样更有效地使用该网络中的工作负载群集 （HYPER-V、 SQL Server 故障转移群集实例、 存储副本等） 使用 SMB 多通道和结果。 群集的配置规模文件服务器群集用于存储有关 HYPER-V 工作负载数据或 SQL Server 故障转移群集实例群集，该网络通常称为"北美南子网"位置/网络中已合并好 (也称为 disaggregated)。 许多客户通过投入 RDMA 支持 NIC 卡和开关最大化吞吐量该网络。  

![北美南 SMB 子网的图示](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)  
**图 2： 若要获得网络最大的吞吐量，使用多个 Nic 规模文件服务器群集和 HYPER-V 或 SQL Server 故障转移群集实例群集-共享北美南子网**  

![两个群集相同子网中使用多个 Nic 利用 SMB 多通道 Screencap](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png)  
**图 3: 2 个群集 (规模文件服务器的 SQL Server 存储<abbr title="故障转移群集实例">FCI</abbr>工作负载) 同时使用多个 Nic 相同子网利用 SMB 多通道并取得更好的网络吞吐量。** 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>自动识别 IPv6 链接本地专用网络  
当检测到具有多个 Nic 专用 （仅适用于群集） 网络时，群集将自动识别 IPv6 链接本地 (fe80) 的 IP 地址每个 NIC 每个子网。 这可以节省管理员次因为它们不再需要手动配置 IPv6 链接本地 (fe80) IP 地址的资源。  

当使用多个的专用 （仅适用于群集） 网络，检查以确保该路由未配置为交叉网，因为这会减少网络性能 IPv6 路由配置。  

![在故障转移群集管理器 UI 中的自动网络配置的 Screencap](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)  
**图 4： 自动 IPv6 链接本地 (fe80) 的地址资源配置**  

## <a name="throughput-and-fault-tolerance"></a>吞吐量和故障功能  
Windows Server 2016 自动检测到 NIC 功能，并将尝试使用的每个 NIC 中可能的最快配置。 搭配使用的 Nic、 使用 RSS，Nic 和 Nic RDMA 功能都可以用。 下表总结权衡时使用这些技术。 最大的吞吐量使用多个 RDMA 支持 Nic 时。 有关详细信息，请参阅[SMB Mutlichannel 的基础知识](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/)。

![吞吐量和错所允许的各种 NIC 配置的图示](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)  
**图 5： 吞吐量和错所允许的各种 NIC conifigurations**   

## <a name="frequently-asked-questions"></a>常见问题  
**适用于群集核心再多 NIC 网络中的所有 Nic？**  
    是的。  

**多 NIC 网络是否可用于群集的通信 或者，可以它仅用于客户端和群集通信？**  
    可以将工作配置-群集的所有网络角色都适用于多 NIC 网络。  

**SMB 多通道也可用于 CSV 和群集交通？**  
    是，默认情况下所有群集和 CSV 交通将都使用多 NIC 可用网络。 管理员可以使用故障转移群集 PowerShell cmdlet 或故障转移群集管理器 UI 更改的网络角色。  

**如何查看 SMB 多通道设置？**  
    使用**获取 SMBServerConfiguration** cmdlet，查找 EnableMultiChannel 属性的价值。  

**PlumbAllCrossSubnetRoutes 考虑群集常见财产位于多 NIC 网络？**  
     是的。  

## <a name="see-also"></a>请参阅  
- [什么是 Windows 服务器在群集故障转移中的新增功能](whats-new-in-failover-clustering.md)  
