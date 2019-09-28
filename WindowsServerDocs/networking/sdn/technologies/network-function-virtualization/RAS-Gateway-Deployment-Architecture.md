---
title: RAS 网关部署体系结构
description: 你可以使用本主题来了解 Windows Server 2016 中 RAS 网关的云服务提供商（CSP）部署，包括 RAS 网关池、路由发送程序，以及为各个租户部署多个网关。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d46e4e91-ece0-41da-a812-af8ab153edc4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7b3bd47052e482b562e84d5c44b928c0744b223c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405914"
---
# <a name="ras-gateway-deployment-architecture"></a>RAS 网关部署体系结构

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题来了解 RAS 网关的云服务提供商（CSP）部署，包括 RAS 网关池、路由发送程序，以及为各个租户部署多个网关。  
  
以下部分简要概述了一些 RAS 网关新功能，以便你可以了解如何在网关部署的设计中使用这些功能。  
  
此外，还提供了一个示例部署，包括有关添加新租户、路由同步和数据平面路由、网关和路由反射器故障转移等过程的信息。  
  
本主题包含以下部分。  
  
-   [使用 RAS 网关的新功能设计部署](#bkmk_new)  
  
-   [部署示例](#bkmk_example)  
  
-   [添加新租户和客户地址（CA）空间 EBGP 对等互连](#bkmk_tenant)  
  
-   [路由同步和数据平面路由](#bkmk_route)  
  
-   [网络控制器对 RAS 网关和路由反射器故障转移的响应方式](#bkmk_failover)  
  
-   [使用新 RAS 网关功能的优点](#bkmk_advantages)  
  
## <a name="bkmk_new"></a>使用 RAS 网关的新功能设计部署  
RAS 网关包含多项新功能，这些功能可更改和改进你的数据中心内部署网关基础结构的方式。  
  
### <a name="bgp-route-reflector"></a>BGP 路由反射器  
边界网关协议（BGP）路由反射器功能现在包含在 RAS 网关中，它提供了另一种方法，即在路由器之间路由同步通常需要的 BGP 完整网格拓扑。 对于完全网格同步，所有 BGP 路由器必须与路由拓扑中的所有其他路由器连接。 然而，使用路由反射器时，路由反射器是唯一连接到其他所有路由器的路由器（称为 BGP 路由反射器客户端），因此可以简化路由同步并减少网络流量。 路由反射器学习所有路由，计算最佳路由，并将最佳路由重新分发到其 BGP 客户端。  
  
有关详细信息，请参阅[RAS 网关中的新增功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)。  
  
### <a name="bkmk_pools"></a>网关池  
在 Windows Server 2016 中，可以创建多个不同类型的网关池。 网关池包含 RAS 网关的多个实例，并在物理网络与虚拟网络之间路由网络流量。  
  
有关详细信息，请参阅 RAS 网关和[Ras 网关高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)[中的新增功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)。  
  
### <a name="bkmk_gps"></a>网关池可伸缩性  
可以通过在池中添加或删除网关 Vm 来轻松地向上或向下缩放网关池。 删除或添加网关不会中断池提供的服务。 你还可以添加和删除整个网关池。  
  
有关详细信息，请参阅 RAS 网关和[Ras 网关高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)[中的新增功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)。  
  
### <a name="bkmk_m"></a>M + N 网关池冗余  
每个网关池均为 M + N 冗余。 这意味着，"N" 个备用网关 vm 数由 "N" 个备用网关 vm 备份。 通过 M + N 冗余，可以更灵活地确定部署 RAS 网关时所需的可靠性级别。  
  
有关详细信息，请参阅 RAS 网关和[Ras 网关高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)[中的新增功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)。  
  
## <a name="bkmk_example"></a>部署示例  
下图提供了一个示例，其中包含在两个租户、Contoso 和 Woodgrove 之间以及 Fabrikam CSP 数据中心之间配置的站点到站点 VPN 连接上的 eBGP 对等互连。  
  
![通过站点到站点 VPN 的 eBGP 对等互连](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
在此示例中，Contoso 需要额外的网关带宽，从而导致在 GW3 而不是 GW2 上终止 Contoso 洛杉矶站点的网关基础结构设计决策。 因此，来自不同站点的 Contoso VPN 连接将在两个不同的网关的 CSP 数据中心终止。  
  
这两个网关（GW2 和 GW3）是网络控制器在 CSP 将 Contoso 和 Woodgrove 租户添加到其基础结构时配置的第一个 RAS 网关。 因此，这两个网关配置为这些相应客户（或租户）的路由发送程序。 GW2 是 Contoso 路由反射器，GW3 是 Woodgrove 路由反射器，除了是与 Contoso 洛杉矶总部站点的 VPN 连接的 CSP RAS 网关终止点。  
  
> [!NOTE]  
> 一个 RAS 网关可根据每个租户的带宽要求，将虚拟网络和物理网络流量路由至多达100个不同的租户。  
  
作为 Route 发送程序，GW2 将 Contoso CA Space 路由发送到网络控制器，GW3 将 Woodgrove CA Space 路由发送到网络控制器。  
  
网络控制器将 Hyper-v 网络虚拟化策略推送到 Contoso 和 Woodgrove 虚拟网络，并将 RAS 策略推送到 RAS 网关，并将负载均衡策略推送到配置为软件负载平衡的 Multiplexers (（Mux）池子.  
  
## <a name="bkmk_tenant"></a>添加新租户和客户地址（CA）空间 eBGP 对等互连  
当你签署新客户并将客户添加为你的数据中心中的新租户时，你可以使用以下过程，其中许多操作由网络控制器和 RAS 网关 eBGP 路由器自动执行。  
  
1.  根据租户的要求预配新的虚拟网络和工作负载。  
  
2.  如果需要，请在你的数据中心内配置远程租户企业站点与他们的虚拟网络之间的远程连接。 当你为租户部署站点到站点 VPN 连接时，网络控制器将从可用的网关池中自动选择可用的 RAS 网关 VM，并配置连接。  
  
3.  为新租户配置 RAS 网关 VM 时，网络控制器还会将 RAS 网关配置为 BGP 路由器，并将其指定为租户的路由反射器。 即使 RAS 网关用作网关，或者作为网关和路由反射器（对于其他租户），也是如此。  
  
4.  根据是否将 CA 空间路由配置为使用静态配置的网络或动态 BGP 路由，网络控制器将在 RAS 网关 VM 和路由反射器中配置相应的静态路由、BGP 邻居或两者。  
  
    > [!NOTE]  
    > -   网络控制器为租户配置了 RAS 网关和路由反射器后，每当同一租户需要新的站点到站点 VPN 连接时，网络控制器就会在此 RAS 网关 VM 上检查可用容量。 如果原始网关可以为所需的容量提供服务，则还会在同一 RAS 网关 VM 上配置新的网络连接。 如果 RAS 网关 VM 无法处理额外容量，网络控制器将选择新的可用 RAS 网关 VM，并在其上配置新连接。 与该租户关联的新 RAS 网关 VM 成为原始租户 RAS 网关路由反射器的路由反射器客户端。  
    > -   由于 RAS 网关池位于软件负载平衡器（SLBs）之后，因此租户的站点到站点 VPN 地址都使用单个公共 IP 地址，称为虚拟 IP 地址（VIP），由 SLBs 转换为数据中心内部 IP 地址，称为动态 IP 地址（DIP），适用于路由企业租户流量的 RAS 网关。 通过 SLB 进行的公共到专用 IP 地址映射可确保在企业站点和 CSP RAS 网关和路由发送程序之间正确建立站点到站点 VPN 隧道。  
    >   
    >     有关 SLB、Vip 和 Dip 的详细信息，请参阅用于[SDN 的&#40;软件&#41;负载平衡 SLB](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。  
  
5.  为新租户建立企业站点与 CSP 数据中心 RAS 网关之间的站点到站点 VPN 隧道后，与隧道关联的静态路由将在隧道的企业和 CSP 端自动预配.  
  
6.  对于 CA 空间 BGP 路由，还会建立企业站点和 CSP RAS 网关路由反射器之间的 eBGP 对等互连。  
  
## <a name="bkmk_route"></a>路由同步和数据平面路由  
在企业站点和 CSP RAS 网关路由反射器之间建立 eBGP 对等互连后，路由反射器使用动态 BGP 路由来了解所有企业路由。 路由反射器会在所有路由反射器客户端之间同步这些路由，以便所有这些路由都是用同一组路由配置的。  
  
路由反射器还使用路由同步将这些合并的路由更新到网络控制器。 然后，网络控制器将路由转换为 Hyper-v 网络虚拟化策略，并配置结构网络以确保已设置端到端数据路径路由。 此过程使租户虚拟网络可从租户企业站点访问。  
  
对于数据平面路由，到达 RAS 网关 Vm 的数据包将直接路由到租户的虚拟网络，因为所需的路由现在可用于所有参与的 RAS 网关 Vm。  
  
同样，在设置了 Hyper-v 网络虚拟化策略的情况下，租户虚拟网络将数据包直接路由到 RAS 网关 Vm （无需知道路由反射器），然后通过站点到站点 VPN 隧道路由到企业站点.  
  
另外。 从租户虚拟网络将流量返回到远程租户企业站点将绕过 SLBs，这是一个称为直接服务器返回（DSR）的进程。  
  
## <a name="bkmk_failover"></a>网络控制器对 RAS 网关和路由反射器故障转移的响应方式  
下面是两种可能的故障转移方案-一种用于 RAS 网关路由反射器客户端，一个用于 RAS 网关路由发送程序，包括有关网络控制器在任一配置中如何处理 Vm 的故障转移的信息。  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>RAS 网关 BGP 路由反射器客户端的 VM 故障  
当 RAS 网关路由反射器客户端失败时，网络控制器将执行以下操作。  
  
> [!NOTE]  
> 当 RAS 网关不是租户 BGP 基础结构的路由反射器时，它是租户的 BGP 基础结构中的路由反射器客户端。  
  
-   网络控制器选择可用的备用 RAS 网关 VM，并使用配置为失败的 RAS 网关 VM 来预配新的 RAS 网关 VM。  
  
-   网络控制器更新相应的 SLB 配置，以确保将站点到站点 VPN 隧道从租户站点升级到失败的 RAS 网关。  
  
-   网络控制器在新网关上配置 BGP 路由反射器客户端。  
  
-   网络控制器将新的 RAS 网关 BGP 路由反射器客户端配置为活动。 RAS 网关立即与租户的路由反射器建立对等互连，以共享路由信息并为相应的企业站点启用 eBGP 对等互连。  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>RAS 网关 BGP 路由反射器的 VM 故障  
当 RAS 网关 BGP 路由反射器失败时，网络控制器将执行以下操作。  
  
-   网络控制器选择可用的备用 RAS 网关 VM，并使用配置为失败的 RAS 网关 VM 来预配新的 RAS 网关 VM。  
  
-   网络控制器在新的 RAS 网关 VM 上配置路由反射器，并为新的 VM 分配已失败 VM 使用的相同 IP 地址，从而提供路由完整性（尽管 VM 发生故障）。  
  
-   网络控制器更新相应的 SLB 配置，以确保将站点到站点 VPN 隧道从租户站点升级到失败的 RAS 网关。  
  
-   网络控制器将新的 RAS 网关 BGP 路由反射器 VM 配置为活动。  
  
-   路由反射器立即变为活动状态。 与企业建立了站点到站点 VPN 隧道，并且路由反射器使用 eBGP 对等互连以及与企业站点路由器交换路由。  
  
-   选择 BGP 路由后，RAS 网关 BGP 路由反射器会更新数据中心内的租户路由反射器客户端，并将路由与网络控制器同步，使端到端数据路径可用于租户通信。  
  
## <a name="bkmk_advantages"></a>使用新 RAS 网关功能的优点  
下面是在设计 RAS 网关部署时使用这些新 RAS 网关功能的几个优点。  
  
**RAS 网关可伸缩性**  
  
由于可以根据需要将任意数量的 RAS 网关 Vm 添加到 RAS 网关池，因此可以轻松地扩展 RAS 网关部署以优化性能和容量。 将 Vm 添加到池时，可以使用任何类型的站点到站点 VPN 连接（IKEv2、L3、GRE）来配置这些 RAS 网关，消除不停机的容量瓶颈。  
  
**简化的企业站点网关管理**  
  
当你的租户有多个企业站点时，租户可以为所有站点配置一个远程站点到站点 VPN IP 地址和一个远程邻居 IP 地址-你的 CSP 数据中心 RAS 网关 BGP 路由反射器 VIP 用于该租户。 这简化了租户的网关管理。  
  
**网关故障快速修正**  
  
若要确保快速故障转移响应，可以将边缘路由和控制路由器之间的 BGP Keepalive 参数时间配置为短时间间隔，例如小于或等于10秒。 使用此短暂的 keep-alive 间隔，如果 RAS 网关 BGP 边缘路由器发生故障，则会快速检测到故障，并按前面部分中提供的步骤进行操作。 此优势可能会减少单独的故障检测协议（如双向转发检测（BFD）协议）的需要。  
  
 
  


