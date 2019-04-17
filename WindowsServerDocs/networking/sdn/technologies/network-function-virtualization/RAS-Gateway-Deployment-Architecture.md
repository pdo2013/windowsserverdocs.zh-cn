---
title: RAS 网关部署建筑
description: 你可以使用本主题以了解有关云服务提供商 (CSP) 部署 Windows Server 2016，包括 RAS 网关池，路线反射器，并为单个租户部署多个网关 RAS 网关。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d46e4e91-ece0-41da-a812-af8ab153edc4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 21b101e10dba3d3b9578d6804b4fd92fbbcd2167
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-deployment-architecture"></a>RAS 网关部署建筑

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解有关包括 RAS 网关池，路线反射器，并为单个租户部署多个网关 RAS 网关的云服务提供商 (CSP) 部署。  
  
以下部分提供一些 RAS 网关的新功能的简短概述，以便你可以了解如何使用这些功能在网关部署设计。  
  
此外，提供部署示例，其中包括添加新租户、路线同步和数据平面路由、网关和路线反射故障转移，以及更多的进程信息。  
  
本主题包含以下部分。  
  
-   [使用 RAS 网关新功能设计部署](#bkmk_new)  
  
-   [示例部署](#bkmk_example)  
  
-   [添加新租户和客户地址（加拿大）空间 EBGP 对等](#bkmk_tenant)  
  
-   [路线同步和数据平面路由](#bkmk_route)  
  
-   [网络控制器响应 RAS 网关和路线反射故障转移](#bkmk_failover)  
  
-   [使用 RAS 网关的新功能的几点优势](#bkmk_advantages)  
  
## <a name="bkmk_new"></a>使用 RAS 网关新功能设计部署  
RAS 网关有一些更改和改进的方式在其中你部署您网关的基础结构在您的数据中心中的多个新功能。  
  
### <a name="bgp-route-reflector"></a>BGP 路线反射  
边框网关协议 (BGP) 路线反射功能现已附带 RAS 网关，并提供是通常所必需的路线同步路由器之间的 BGP 交错拓扑另一种方法。 具有完整网格同步所有 BGP 路由器必须都联系路由拓扑中所有其他路由器。 当你使用路线反射时，但是，路线反射是唯一路由器使用所有其他路由器，称为 BGP 路线反射客户端，从而简化了路线同步和减少流量网络连接。 路线反射了解所有路线，计算最佳的路线，并将重新分发给其 BGP 客户端最佳的路线。  
  
有关详细信息，请参阅[RAS 网关中的新增功能是什么](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)。  
  
### <a name="bkmk_pools"></a>网关池  
在 Windows Server 2016，可以创建不同类型的许多网关的池。 网关池包含许多情况下的 RAS 网关，并路由物理和虚拟网络之间的网络通信。  
  
有关详细信息，请参阅[RAS 网关中的新增功能是什么](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)和[RAS 网关高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
### <a name="bkmk_gps"></a>网关池可扩展性  
你可以轻松调整网关池向上或向下通过添加或删除池中网关虚拟机的功能。 删除或网关加不会中断池提供服务。 你还可以添加和删除网关整个池。  
  
有关详细信息，请参阅[RAS 网关中的新增功能是什么](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)和[RAS 网关高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
### <a name="bkmk_m"></a>M + N 网关池冗余  
每个网关池是 M + N 冗余。 这意味着是数目活动网关虚拟机的功能由待机网关虚拟机的功能 n 数备份。 M + N 冗余为您提供更大的灵活性决定级别部署 RAS 网关时，你需要的可靠性。  
  
有关详细信息，请参阅[RAS 网关中的新增功能是什么](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)和[RAS 网关高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_example"></a>示例部署  
下图 eBGP 对等站点的 VPN 连接配置之间两个租户、Contoso 和 Woodgrove，以及 Fabrikam CSP datacenter 上与提供一个示例。  
  
![对等站点的 VPN 轻 eBGP](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
在此示例中，Contoso 需要其他网关带宽导致终止 Contoso 洛杉矶站点上而不是 GW2 GW3 网关基础结构设计的决定。 出于此原因，来自不同的站点的 Contoso VPN 连接终止在两个不同网关 CSP 数据中心中。  
  
这两个 GW2 和 GW3，这些网关了第一个 RAS 网关 CSP 添加到他们的基础结构对 Contoso 和 Woodgrove 租户时，通过网络控制器配置。 出于此原因，这些两个网关配置为路线反射器这些相应的客户（或租户）。 GW2 Contoso 路线反射，并且 GW3 Woodgrove 路线反射-除了正在与 Contoso 洛杉矶总部网站的 VPN 连接 CSP RAS 网关终止点。  
  
> [!NOTE]  
> 一个 RAS 网关可以为 100 最多个不同租户，具体取决于每个租户带宽要求路由虚拟和物理网络流量。  
  
作为路线反射器，GW2 将 Contoso CA 空间的路线发送到网络控制器、，并 GW3 将 Woodgrove CA 空间的路线发送到网络控制器。  
  
网络控制器 Contoso 和 Woodgrove 虚拟网络，以及 RAS 策略 RAS 网关和负载平衡配置为软件负载平衡池路复 (MUXes) 的策略于推送 Hyper-V 网络虚拟化策略。  
  
## <a name="bkmk_tenant"></a>添加新租户和客户地址（加拿大）空间 eBGP Peering  
当你登录一个新客户，并将作为新租户客户添加您的数据中心中时，你可以使用下面的过程，其中的许多自动由网络控制器和 RAS 网关 eBGP 路由器。  
  
1.  设置新的虚拟网络和根据你租户要求的工作负载。  
  
2.  如有必要，配置远程远程租户企业站点和其虚拟网络之间的连接，在您的数据中心。 租户部署站点的 VPN 连接时，网络控制器将自动选中从可用网关池可用的 RAS 网关虚拟机和配置连接。  
  
3.  在新租户配置 RAS 网关 VM，请网络控制器还配置为 BGP 路由器 RAS 网关，并将其指定为租户路线反射。 其中 RAS 网关的性质网关作为或网关和路线反射其他租户也是如此，即使是在情况。  
  
4.  具体取决于是否 CA 空间路由配置为使用的静态配置了网络或动态 BGP 路由、网络控制器配置相应的静态路线、BGP 邻居或两者都上 RAS 网关 VM 和路线反射。  
  
    > [!NOTE]  
    > -   网络控制器配置 RAS 网关和路线反射为租户后，每当相同租户需要新站点的 VPN 连接、网络控制器上检查此 RAS 网关虚拟机上的可用容量。 如果原始网关可以服务所需的容量，新的网络连接还配置在同一 RAS 网关 VM。 如果 RAS 网关 VM 无法处理其他容量、网络控制器选择新的可用 RAS 网关 VM 和配置上的新连接。 与租户此新 RAS 网关 VM 成为的原始租户 RAS 网关路线反射路线反射客户端。  
    > -   由于 RAS 网关池背后软件负载平衡 (SLBs)，对租户的站点之间 VPN 解决每次使用一个公用 IP 地址，称为虚拟的 IP 地址 (VIP)，这 SLBs 转为 datacenter 内部的 IP 地址，并将交通路由为企业租户的 RAS 网关称为动态的 IP 地址（冲动）。 通过 SLB 此公众到专用 IP 地址映射确保该站点的 VPN 隧道正确之间企业站点 CSP RAS 网关和路线反射器建立。  
    >   
    >     有关 SLB、VIPs，以及 Dip 的详细信息，请参阅[软件负载平衡 & #40;SLB & #41;对于 SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。  
  
5.  后企业站点和 CSP 数据中心的新租户建立 RAS 网关之间的站点的 VPN 隧道，与隧道静态路线自动设置企业版和 CSP 两侧隧道上。  
  
6.  加拿大空间 BGP 路由、对企业站点和 CSP RAS 网关路线反射之间等 eBGP 还建立。  
  
## <a name="bkmk_route"></a>路线同步和数据平面路由  
对 eBGP 等企业站点和 CSP RAS 网关路线反射之间建立后，路线反射通过动态 BGP 路由了解所有企业路线。 路线反射同步所有路线反射客户端之间这些路线，以便它们都配置使用相同的路线集。  
  
反射路线也会更新这些统一的路线使用路线同步，为网络控制器。 网络控制器然后路线转化 Hyper-V 网络虚拟化策略和配置结构网络，以确保 End-to-End 数据路径路由已预配。 此过程使租户虚拟网络租户企业访问的站点。  
  
数据平面路由，到达 RAS 网关虚拟机的功能的数据包直接会路由到租户虚拟网络，，因为所需的路线现已适用于所有加盟 RAS 网关虚拟机的功能。  
  
同样，位置 Hyper-V 网络虚拟化策略，租户虚拟网络将数据包企业站点直接到 RAS 网关虚拟机（而不需要了解关于路线反射），然后在这些站点的 VPN 隧道。  
  
另外。 远程租户企业站点从租户虚拟网络退货通信可绕过 SLBs，此过程称为直接 Server 返回 (DSR)。  
  
## <a name="bkmk_failover"></a>网络控制器响应 RAS 网关和路线反射故障转移  
下面是两个可能的故障转移情形的一个 RAS 网关路线反射客户端-，一个用于 RAS 网关路线反射器包括有关如何网络控制器处理信息故障转移虚拟机的功能的任何配置中。  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>VM 失败的 RAS 客户端网关 BGP 路线反射  
RAS 网关路线反射客户端失败时，网络控制器执行下列操作。  
  
> [!NOTE]  
> 当 RAS 网关不可路线反射租户 BGP 基础结构时，它是路线反射客户端中租户 BGP 基础结构。  
  
-   网络控制器选择适用的待机 RAS 网关 VM，并提供新的 RAS 网关 VM 与失败 RAS 网关 VM 配置。  
  
-   网络控制器更新相应的 SLB 配置，以确保对失败 RAS 网关租户站点从这些站点的 VPN 隧道正确用新 RAS 网关建立。  
  
-   网络控制器上的新网关配置 BGP 路线反射客户端。  
  
-   网络控制器配置为活动新 RAS 网关 BGP 路线反射客户端。 RAS 网关立即开始对租户路线反射共享路由信息并启用相应企业站点对等 eBGP 与等。  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>对于 RAS 网关 BGP 路线反射 VM 失败  
RAS 网关 BGP 路线反射失败时，网络控制器执行下列操作。  
  
-   网络控制器选择适用的待机 RAS 网关 VM，并提供新的 RAS 网关 VM 与失败 RAS 网关 VM 配置。  
  
-   网络控制器上 RAS 网关 VM 配置路线反射和分配的新虚拟机相同失败 VM 中，从而提供尽管 VM 失败的路线完整性所使用的 IP 地址。  
  
-   网络控制器更新相应的 SLB 配置，以确保对失败 RAS 网关租户站点从这些站点的 VPN 隧道正确用新 RAS 网关建立。  
  
-   网络控制器将新 RAS 网关 BGP 路线反射 VM 配置为活动。  
  
-   路线反射立即生效。 建立站点的 VPN 隧道到企业版，则和路线反射使用 eBGP 对等并交换与企业站点路由器的路线。  
  
-   BGP 路线选择后，RAS 网关 BGP 路线反射更新租户数据中心中的路线反射客户端并在同步网络控制器、使 End-to-End 数据路径其租户交通可用的路线。  
  
## <a name="bkmk_advantages"></a>使用 RAS 网关的新功能的几点优势  
以下是几个设计 RAS 网关部署时，使用这些新 RAS 网关功能的优势。  
  
**RAS 网关可扩展性**  
  
因为你可以添加多个 RAS 网关 Vm 根据需要 RAS 网关池，你可以轻松调整 RAS 网关部署优化性能和容量。 当你添加到池的虚拟机的功能时，可以将这些 RAS 网关的站点的 VPN 连接的任何类型 (IKEv2，L3，GRE)，消除容量瓶颈时间未配置为。  
  
**简化的企业站点网关管理**  
  
当你租户具有多个企业站点时，具有一个远程的站点之间 VPN IP 地址的所有站点和单个远程邻居的 IP 地址-该租户你 CSP datacenter RAS 网关 BGP 路线反射 VIP 可以配置租户。 这简化了有关你租户网关管理。  
  
**网关失败的 fast 补救**  
  
为了确保快速故障转移响应，您可以配置之间 edge 路线和短的时间间隔，如小于或等于 10 秒钟到控制路由器的 BGP 保持连接参数时间。 使用此短保持活动状态的时间间隔，如果 RAS 网关 BGP edge 路由器失败，失败很快检测到并网络控制器遵循以前部分中提供的步骤。 此利用可能降低需要单独的故障的检测协议，如双向转发检测 (BFD) 协议。  
  
 
  


