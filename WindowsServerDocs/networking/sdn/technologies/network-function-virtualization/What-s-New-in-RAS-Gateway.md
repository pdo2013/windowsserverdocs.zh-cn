---
title: 什么是 RAS 网关中的新增功能
description: 你可以使用此主题以了解新功能的是基于软件，RAS 网关 multitenant、Windows Server 2016 中的边框网关协议 (BGP) 支持路由器。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 709cb192-313a-47b5-954e-eb5f6fee51a7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1a9ac762f6cd80d3889cf72478b7a7f8ce9e5cb7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-ras-gateway"></a>什么是 RAS 网关中的新增功能

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此主题以了解新功能的是基于软件，RAS 网关 multitenant、Windows Server 2016 中的边框网关协议 (BGP) 支持路由器。 RAS 网关 Multitenant BGP 路由器设计云服务提供商 (Csp) 和主机使用 Hyper-V 网络虚拟化多个租户虚拟网络的企业。  
  
> [!NOTE]  
> 在 Windows Server 2012 R2 RAS 网关称作 RRAS 网关;并在系统中心虚拟机管理器 RAS 网关称作 Windows Server 网关。  
  
本主题包含以下部分。  
  
-   [站点的连接选项](#bkmk_s2s)  
  
-   [网关池](#bkmk_pools)  
  
-   [网关池可扩展性](#bkmk_gps)  
  
-   [M + N 网关池冗余](#bkmk_m)  
  
-   [路线反射](#bkmk_rr)  
  
## <a name="bkmk_s2s"></a>站点的连接选项  
RAS 网关现在支持的三种类型的站点之间的 VPN 连接：Internet 键 Exchange 版本 (IKEv2) 2 站点的虚拟专用网络 (VPN)、3 层 (L3) VPN 和通用路由封装 (GRE) 隧道。  
  
有关 GRE 的详细信息，请参阅[GRE 隧道在 Windows Server 2016](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
## <a name="bkmk_pools"></a>网关池  
在 Windows Server 2016，可以创建不同类型的网关的池。 网关池包含许多情况下的 RAS 网关，并路由物理和虚拟网络之间的网络通信。 网关池可以执行个别网关函数-Internet 键 Exchange 版本 2 (IKEv2) 中的任意站点的虚拟专用网络 (VPN)、3 层 (L3) VPN 和泛型路由封装 (GRE) 隧道-或池可以执行的所有这些功能和充当混合池。  
  
您还可以使用任何你想基础结构要求基于的逻辑网关池。 例如，你可以创建基于任何以下特点网关池。  
  
-   隧道类型 (IKEv2 VPN，L3 VPN GRE VPN)  
  
-   容量  
  
-   冗余级别（基于你付费套餐租户可靠性）  
  
-   自定义的分隔客户  
  
有关详细信息，请参阅[RAS 网关高可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_gps"></a>网关池可扩展性  
你可以轻松调整网关池向上或向下通过添加或删除池中网关虚拟机的功能。 删除或网关加不会中断池提供服务。 你还可以添加和删除网关整个池。  
  
有关详细信息，请参阅[RAS 网关高可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_m"></a>M + N 网关池冗余  
每个网关池是 M + N 冗余。 这意味着已 ' n 许多待机网关 Vm 备份活动网关虚拟机 (Vm) 的数量。 M + N 冗余为您提供更大的灵活性决定级别部署 RAS 网关时，你需要的可靠性。 你现在可以而不是通过使用一只的待机 RAS 网关每个活动 RAS 网关虚拟机-这是与 Windows Server 2012 R2 的唯一配置选项-配置尽可能多待机 Vm 根据需要。 网络控制器网关服务管理器功能有效地使用待机 RAS 网关 VM 容量提供可靠故障转移如果活动 RAS 网关 VM 无法正常工作或断开连接。  
  
有关详细信息，请参阅[RAS 网关高可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_rr"></a>路线反射  
现已边框网关协议 (BGP) 路线反射附带 RAS 网关，并提供所需的路线同步路由器之间 BGP 交错拓扑另一种方法。 具有完整网格同步所有 BGP 路由器必须都联系路由拓扑中所有其他路由器。 当你使用路线反射时，但是，路线反射是唯一路由器使用所有其他路由器，称为 BGP 客户端，从而简化了路线同步和减少流量网络连接。 路线反射了解所有路线，计算最佳的路线，并将重新分发给其 BGP 客户端最佳的路线。  
  
与 Windows Server 2016 中，您可以配置个别租户远程访问隧道在多个 RAS 网关 VM 终止。 这提供改进的灵活性云服务提供商时怪脸庞的情况下其中一个 RAS 网关 VM 无法满足所有租户连接的带宽要求。  
  
此功能，但是，引入了其他路线管理的复杂性和有效同步租户远程站点和云数据中心中的其虚拟资源之间的路线。 向租户连接提供多个 RAS 网关还引入了其他复杂性企业结束时，每个租户网站都单独路由邻居配置中。  
  
在控制平面 BGP 路线反射解决这些问题，并使企业租户透明 CSP 内部结构部署。 下面是一些要点有关 BGP 路线反射附带 RAS 网关并与网络控制器集成。  
  
-   在软件定义网络部署 A 路线反射是位于控制平面 RAS 网关之间的网络控制器的逻辑实体。 它不会但是，参与数据平面路由。  
  
-   当你添加到你的数据中心的新租户时，网络控制器自动配置路线反射作为第一个租户 RAS 网关内容。  
  
-   每个租户相应的路线发送程序，并且位于与该租户 RAS 网关 Vm 之一。  
  
-   租户路线反射 RAS 网关 Vm 租户与相关联的所有充当路线反射。 租户网关之外 RAS 网关路线反射是路线反射客户端。 路线反射执行之间所有路线反射客户端的路线同步，以便实际数据路径路由可能会出现。  
  
-   一个路线反射不提供 RAS 网关依据配置路线反射服务。  
  
-   一个路线反射更新对租户企业站点对应企业路线网络控制器。 这将使网络控制器上 End-to-End 数据路径访问租户虚拟网络配置所需的 Hyper-V 网络虚拟化策略。  
  
-   如果你的企业客户的客户地址空间内使用 BGP 路由，RAS 网关路线反射是对所有相应租户的站点的唯一的外部 BGP (eBGP) 邻居。 无论是如此企业租户隧道终止点。 换言之，无论中 CSP 哪个 RAS 网关 VM datacenter 终止租户网站站点的 VPN 隧道时，所有租户站点 eBGP 等是路线反射。  
  
有关详细信息，请参阅[RAS 网关部署体系结构](RAS-Gateway-Deployment-Architecture.md)和评论主题 Internet 工程工作组 (IETF) 请求[RFC 4456 BGP 路线倒影：替代到完整 Mesh 内部 BGP (IBGP)](https://tools.ietf.org/html/rfc4456)。  
  

