---
title: RAS 网关中的新增功能
description: 可以使用本主题以了解有关新功能的 RAS 网关，它是基于软件的多租户、 Windows Server 2016 中的边界网关协议 (BGP) 支持路由器。
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
ms.openlocfilehash: 5cc7d8bab3f2783750dbd723da745b1df3c2e462
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863018"
---
# <a name="whats-new-in-ras-gateway"></a>RAS 网关中的新增功能

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题以了解有关新功能的 RAS 网关，它是基于软件的多租户、 Windows Server 2016 中的边界网关协议 (BGP) 支持路由器。 RAS 网关支持多租户 BGP 路由器旨在为云服务提供商 (Csp) 和承载多个租户虚拟网络使用的 HYPER-V 网络虚拟化的企业。  
  
> [!NOTE]  
> 在 Windows Server 2012 R2 中，RAS 网关名为 RRAS 网关;并且在 System Center Virtual Machine Manager 中，RAS 网关名为 Windows Server 网关。  
  
本主题包含以下部分。  
  
-   [站点到站点连接选项](#bkmk_s2s)  
  
-   [网关池](#bkmk_pools)  
  
-   [网关池可伸缩性](#bkmk_gps)  
  
-   [M + N 网关池冗余](#bkmk_m)  
  
-   [路由反射器](#bkmk_rr)  
  
## <a name="bkmk_s2s"></a>站点到站点连接选项  
RAS 网关现在支持三种类型的 VPN 站点到站点连接：Internet 密钥交换版本 2 (IKEv2) 站点到站点虚拟专用网络 (VPN)、 第 3 层 (L3) VPN 和通用路由封装 (GRE) 隧道。  
  
GRE 有关详细信息，请参阅[Windows Server 2016 中的 GRE 隧道](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
## <a name="bkmk_pools"></a>网关池  
在 Windows Server 2016 中，可以创建不同类型的网关池。 网关池包含 RAS 网关的多个实例和物理和虚拟网络之间路由网络流量。 网关池可以执行任何单个网关函数-Internet 密钥交换版本 2 (IKEv2) 池可以执行所有这些或站点到站点虚拟专用网络 (VPN)、 第 3 层 (L3) VPN 和通用路由封装 (GRE) 隧道-函数及作为混合池。  
  
可以创建网关池使用的任何逻辑都愿意根据基础结构要求。 例如，可以创建基于以下特征的任何网关池。  
  
-   隧道类型 (IKEv2 VPN，L3 VPN GRE VPN)  
  
-   容量  
  
-   冗余级别 （可靠性基于租户计费计划）  
  
-   自定义的分离的客户  
  
有关详细信息，请参阅[RAS 网关高可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_gps"></a>网关池可伸缩性  
您可以轻松调整网关池向上或向下通过添加或删除网关 Vm 的池中。 删除或添加的网关不会中断以池提供服务。 您还可以添加和删除整个池的网关。  
  
有关详细信息，请参阅[RAS 网关高可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_m"></a>M + N 网关池冗余  
每个网关池是 M + N 冗余。 这意味着，将按 n 键数备用网关 Vm 备份的活动网关虚拟机 (Vm) 数。 M + N 冗余为您提供更灵活地确定部署 RAS 网关时，您需要的可靠性级别。 而不是使用一个的备用 RAS 网关每个活动 RAS 网关 VM-这是唯一的配置选项，与 Windows Server 2012 R2-现在可以配置根据需要尽可能多的备用 Vm。 网络控制器网关服务管理器功能有效地使用备用的 RAS 网关 VM 容量提供可靠的故障转移，如果活动 RAS 网关 VM 发生故障或断开连接。  
  
有关详细信息，请参阅[RAS 网关高可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_rr"></a>路由反射器  
边界网关协议 (BGP) 路由反射器现已包含在 RAS 网关，可用于替代 BGP 完整网格拓扑所需的路由的路由器之间的同步。 完整网格同步时，必须与所有其他路由器路由拓扑中连接所有 BGP 路由器。 当使用路由反射器时，但是，路由反射器是唯一的路由器将与所有其他路由器，名为 BGP 客户端，从而简化了路由同步并减少网络流量。 路由反射器了解所有路由、 计算最佳路线，并将向其 BGP 客户端的最佳路由重新分发。  
  
使用 Windows Server 2016 中，可以配置单个租户的远程访问隧道终止多个 RAS 网关 VM 上。 这提供更高的灵活性在面对一个 RAS 网关 VM 不能满足的情况下，云服务提供商的所有租户连接的带宽要求。  
  
此功能，但是，引入了路由管理的其他复杂问题和有效的同步的租户远程网站和云数据中心中的虚拟资源之间的路由。 向租户授予对多个 RAS 网关的连接还引入了在企业结束时，其中每个租户网站将具有单独的路由邻居的配置中的其他复杂性。  
  
在控制平面 BGP 路由反射器解决了这些问题，并使 CSP 内部 fabric 部署到企业租户透明。 以下是有关 BGP 路由反射器是包含在 RAS 网关，与网络控制器集成的一些要点。  
  
-   在部署中软件定义的网络的路由反射器是位于 RAS 网关和网络控制器之间的控制平面的逻辑实体。 它不，但是，参与数据平面的路由。  
  
-   当新租户添加到你的数据中心时，网络控制器会自动将第一个租户 RAS 网关配置为路由反射器。  
  
-   每个租户都有一个相应的路由反射器，并驻留在一个与该租户 RAS 网关 Vm 上。  
  
-   租户路由反射器为所有与租户相关联的 RAS 网关 Vm 充当路由反射器。 RAS 网关路由反射器以外的租户网关是路由反射器客户端。 路由反射器执行路由所有路由反射器客户端之间的同步，因此实际数据路径的路由可能发生。  
  
-   一个路由反射器不提供在其配置 RAS 网关路由反射器服务。  
  
-   一个路由反射器更新到租户的企业站点相对应的企业路由网络控制器。 这使网络控制器可以端到端数据路径访问的租户虚拟网络上配置所需的 HYPER-V 网络虚拟化策略。  
  
-   如果你的企业客户的客户地址空间中使用 BGP 路由，RAS 网关路由反射器是唯一的外部 BGP (eBGP) 邻居的所有相应租户网站。 这是无论企业租户的隧道终结点，则返回 true。 换而言之，无论在 CSP 中的 RAS 网关 VM 的数据中心终止租户站点的站点到站点 VPN 隧道，所有租户网站的 eBGP 对等路由反射器。  
  
有关详细信息，请参阅[RAS 网关部署体系结构](RAS-Gateway-Deployment-Architecture.md)和 Internet 工程任务组 (IETF) 征求意见主题[RFC 4456 BGP 路由反射：完整的替代方法 Mesh 内部 BGP (IBGP)](https://tools.ietf.org/html/rfc4456)。  
  

