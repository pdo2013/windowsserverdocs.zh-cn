---
title: RAS 网关中的新增功能
description: 你可以使用本主题了解 RAS 网关的新功能，它是 Windows Server 2016 中基于软件、多租户、边界网关协议（BGP）的路由器。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 709cb192-313a-47b5-954e-eb5f6fee51a7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 85595c47c599a72039e93e67ea2f33f92af7200c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405901"
---
# <a name="whats-new-in-ras-gateway"></a>RAS 网关中的新增功能

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题了解 RAS 网关的新功能，它是 Windows Server 2016 中基于软件、多租户、边界网关协议（BGP）的路由器。 RAS 网关多租户 BGP 路由器专为使用 Hyper-v 网络虚拟化托管多个租户虚拟网络的云服务提供商（Csp）和企业而设计。  
  
> [!NOTE]  
> 在 Windows Server 2012 R2 中，RAS 网关名为 RRAS 网关;在 System Center Virtual Machine Manager 中，RAS 网关名为 "Windows Server 网关"。  
  
本主题包含以下部分。  
  
-   [站点到站点连接选项](#bkmk_s2s)  
  
-   [网关池](#bkmk_pools)  
  
-   [网关池可伸缩性](#bkmk_gps)  
  
-   [M + N 网关池冗余](#bkmk_m)  
  
-   [路由反射器](#bkmk_rr)  
  
## <a name="bkmk_s2s"></a>站点到站点连接选项  
RAS 网关现在支持三种类型的 VPN 站点到站点连接：Internet 密钥交换版本2（IKEv2）站点到站点虚拟专用网络（VPN）、第3层（L3） VPN 和通用路由封装（GRE）隧道。  
  
有关 GRE 的详细信息，请参阅[Windows Server 2016 中的 Gre 隧道](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
## <a name="bkmk_pools"></a>网关池  
在 Windows Server 2016 中，可以创建不同类型的网关池。 网关池包含 RAS 网关的多个实例，并在物理网络与虚拟网络之间路由网络流量。 网关池可以执行任何单个网关功能-Internet 密钥交换版本2（IKEv2）站点到站点虚拟专用网络（VPN）、第3层（L3） VPN 和通用路由封装（GRE）隧道，或者池可执行所有这些功能函数和充当混合池。  
  
可以根据基础结构要求使用所需的任何逻辑来创建网关池。 例如，你可以基于以下任何特征创建网关池。  
  
-   隧道类型（IKEv2 VPN、L3 VPN、GRE VPN）  
  
-   容量  
  
-   冗余级别（基于租户计费计划的可靠性）  
  
-   客户的自定义分离  
  
有关详细信息，请参阅[RAS 网关高可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_gps"></a>网关池可伸缩性  
可以通过在池中添加或删除网关 Vm 来轻松地向上或向下缩放网关池。 删除或添加网关不会中断池提供的服务。 你还可以添加和删除整个网关池。  
  
有关详细信息，请参阅[RAS 网关高可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_m"></a>M + N 网关池冗余  
每个网关池均为 M + N 冗余。 这意味着，"N" 个备用网关虚拟机（Vm）数量由 "N" 个备用网关 Vm 备份。 通过 M + N 冗余，可以更灵活地确定部署 RAS 网关时所需的可靠性级别。 你可以根据需要配置任意数量的备用 Vm，而不是每个活动 RAS 网关 VM 只使用一个备用 RAS 网关（这是 Windows Server 2012 R2 的唯一配置选项）。 如果活动 RAS 网关 VM 发生故障或失去连接，网络控制器网关 Service Manager 功能有效地使用备用 RAS 网关 VM 容量来提供可靠的故障转移。  
  
有关详细信息，请参阅[RAS 网关高可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_rr"></a>路由反射器  
边界网关协议（BGP）路由反射器现在包含在 RAS 网关中，并提供对路由器之间的路由同步所需的 BGP 完整网格拓扑的替代方法。 对于完全网格同步，所有 BGP 路由器必须与路由拓扑中的所有其他路由器连接。 然而，使用路由反射器时，路由反射器是连接到其他所有路由器的唯一路由器，称为 BGP 客户端，因此简化了路由同步和减少网络流量。 路由反射器学习所有路由，计算最佳路由，并将最佳路由重新分发到其 BGP 客户端。  
  
使用 Windows Server 2016，你可以配置单个租户的远程访问隧道，使其在多个 RAS 网关 VM 上终止。 这为云服务提供商提供了更大的灵活性，因为面临一个 RAS 网关 VM 无法满足租户连接的所有带宽要求的情况。  
  
然而，此功能引入了路由管理的额外复杂性，并有效地同步了租户远程站点及其虚拟资源在云数据中心。 为租户提供与多个 RAS 网关的连接还会在企业结束时增加配置的复杂性，其中每个租户网站将有单独的路由邻居。  
  
控制平面中的 BGP 路由反射器可解决这些问题，并使 CSP 内部结构部署对企业租户透明。 下面是 RAS 路由反射器附带的、与网络控制器集成的一些要点。  
  
-   软件定义的网络部署中的路由反射器是位于 RAS 网关和网络控制器之间的控制平面上的逻辑实体。 但它不参与数据平面路由。  
  
-   将新租户添加到数据中心时，网络控制器会自动将第一个租户 RAS 网关配置为路由反射器。  
  
-   每个租户都有相应的路由反射器，并驻留在与该租户关联的 RAS 网关 Vm 之一中。  
  
-   租户路由反射器充当与租户关联的所有 RAS 网关 Vm 的路由反射器。 除了 RAS 网关路由反射器外，租户网关是路由反射器客户端。 路由反射器在所有路由反射器客户端之间执行路由同步，以便可以进行实际的数据路径路由。  
  
-   路由反射器不提供在其上配置它的 RAS 网关的路由反射器服务。  
  
-   路由反射器使用与租户的企业站点相对应的企业路由更新网络控制器。 这允许网络控制器在租户虚拟网络上配置所需的 Hyper-v 网络虚拟化策略，以实现端到端的数据路径访问。  
  
-   如果企业客户在客户地址空间中使用 BGP 路由，则 RAS 网关路由反射器是对应租户的所有站点的唯一外部 BGP （eBGP）邻居。 无论企业租户的隧道终止点如何都是如此。 换句话说，无论 CSP 数据中心的哪个 RAS 网关 VM 终止租户站点的站点到站点 VPN 隧道，所有租户站点的 eBGP 对等方都是路由反射器。  
  
有关详细信息，请参阅[RAS 网关部署体系结构](RAS-Gateway-Deployment-Architecture.md)和 Internet 工程任务组（IETF）征求意见主题 [RFC 4456 BGP 路由反射：完整网格内部 BGP （IBGP） ](https://tools.ietf.org/html/rfc4456) 的替代方法。  
  

