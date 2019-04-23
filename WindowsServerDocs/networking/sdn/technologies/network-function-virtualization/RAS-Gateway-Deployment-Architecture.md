---
title: RAS 网关部署体系结构
description: 可以使用本主题，了解如何在 Windows Server 2016，包括 RAS 网关池、 路由反射器，并为单个租户部署多个网关的 RAS 网关云服务提供商 (CSP) 部署。
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
ms.openlocfilehash: a3895e25a2af0437deb9eebe4ad89b110cfc9f2b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870268"
---
# <a name="ras-gateway-deployment-architecture"></a>RAS 网关部署体系结构

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题以了解有关云服务提供商 (CSP) 部署 RAS 网关，包括 RAS 网关池、 路由反射器，并为单个租户部署多个网关的信息。  
  
以下各节提供的一些 RAS 网关新功能的简要概述，以便您可以了解如何在你的网关部署的设计中使用这些功能。  
  
此外，已提供的示例部署，包括添加新租户、 路由同步和数据平面的路由、 网关和路由反射器故障转移和的详细信息的进程的信息。  
  
本主题包含以下部分。  
  
-   [使用 RAS 网关新功能来设计您的部署](#bkmk_new)  
  
-   [部署示例](#bkmk_example)  
  
-   [添加新租户和客户地址 (CA) 空间 EBGP 对等互连](#bkmk_tenant)  
  
-   [路由同步和数据平面路由](#bkmk_route)  
  
-   [RAS 网关和路由反射器故障转移到网络控制器的响应方式](#bkmk_failover)  
  
-   [使用 RAS 网关的新功能的优点](#bkmk_advantages)  
  
## <a name="bkmk_new"></a>使用 RAS 网关新功能来设计您的部署  
RAS 网关包含多个新功能的更改和改进的部署网关基础结构在你的数据中心中的方式。  
  
### <a name="bgp-route-reflector"></a>BGP 路由反射器  
边界网关协议 (BGP) 路由反射器功能现已包含在 RAS 网关，提供 BGP 是通常所必需的路由的路由器之间的同步的完整网格拓扑的替代方法。 完整网格同步时，必须与所有其他路由器路由拓扑中连接所有 BGP 路由器。 当使用路由反射器时，但是，路由反射器是唯一的路由器将与所有其他路由器，名为 BGP 路由反射器客户端，从而简化了路由同步并减少网络流量。 路由反射器了解所有路由、 计算最佳路线，并将向其 BGP 客户端的最佳路由重新分发。  
  
有关详细信息，请参阅[What's New in RAS 网关](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)。  
  
### <a name="bkmk_pools"></a>网关池  
在 Windows Server 2016 中，可以创建不同类型的多个网关池。 网关池包含 RAS 网关的多个实例和物理和虚拟网络之间路由网络流量。  
  
有关详细信息，请参阅[What's New in RAS 网关](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)并[RAS 网关高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
### <a name="bkmk_gps"></a>网关池可伸缩性  
您可以轻松调整网关池向上或向下通过添加或删除网关 Vm 的池中。 删除或添加的网关不会中断以池提供服务。 您还可以添加和删除整个池的网关。  
  
有关详细信息，请参阅[What's New in RAS 网关](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)并[RAS 网关高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
### <a name="bkmk_m"></a>M + N 网关池冗余  
每个网关池是 M + N 冗余。 这意味着，将按 n 键数备用网关 Vm 备份的活动网关 Vm 数。 M + N 冗余为您提供更灵活地确定部署 RAS 网关时，您需要的可靠性级别。  
  
有关详细信息，请参阅[What's New in RAS 网关](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)并[RAS 网关高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_example"></a>部署示例  
下图与 eBGP 对等互连通过站点到站点 VPN 连接配置两个租户、 Contoso 和 Woodgrove，和 Fabrikam CSP 数据中心之间提供一个示例。  
  
![eBGP 对等互连通过站点到站点 VPN](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
在此示例中，Contoso 需要附加的网关带宽，从而导致终止而不是 GW2 GW3 上的 Contoso 洛杉矶站点网关基础结构的设计决策。 因此，不同站点中的 Contoso VPN 连接上两个不同的网关的 CSP 数据中心中终止。  
  
它们都 GW2 和 GW3，这些网关是第一个 RAS 网关配置由网络控制器，CSP 将 Contoso 和 Woodgrove 租户添加到其基础结构时。 因此，这些两个网关配置为路由反射器为这些相应的客户 （或租户）。 GW2 是 Contoso 路由反射器，并 GW3 是 Woodgrove 路由反射器-除了与 Contoso Los Angeles 总部站点的 VPN 连接的 CSP RAS 网关终结点。  
  
> [!NOTE]  
> 一个 RAS 网关可以将虚拟和物理网络流量路由用于最多 100 个不同的租户，具体取决于每个租户的带宽要求。  
  
路由反射器作为 GW2 将 Contoso CA 空间路由发送到网络控制器并 GW3 将 Woodgrove CA 空间路由发送到网络控制器。  
  
网络控制器将 HYPER-V 网络虚拟化策略推送到 Contoso 和 Woodgrove 的虚拟网络，以及到 RAS 网关和负载平衡到多路复用器 （多路复用器） 配置为软件负载平衡策略的 RAS 策略池。  
  
## <a name="bkmk_tenant"></a>添加新租户和客户地址 (CA) 空间 eBGP 对等互连  
时登录的新客户和你的数据中心中将客户添加为新租户，可以使用以下过程中，大部分都由网络控制器和 RAS 网关 eBGP 路由器自动执行。  
  
1.  预配新的虚拟网络和你的租户要求根据工作负荷。  
  
2.  如有必要，在你的数据中心配置的远程租户企业站点和其虚拟网络之间的远程连接。 为租户部署站点到站点 VPN 连接时，网络控制器将自动从可用的网关池选择可用的 RAS 网关 VM，并会将连接配置。  
  
3.  在 RAS 网关 VM 配置为新租户时，网络控制器还将 RAS 网关配置为 BGP 路由器，并将其指定为该路由反射器租户。 这是甚至在情况下，则返回 true，RAS 网关用作网关，或作为网关和路由反射器其他租户。  
  
4.  具体取决于是否 CA 空间路由配置为使用静态配置的网络或动态 BGP 路由，网络控制器配置相应的静态路由、 BGP 邻居或这两者上的 RAS 网关 VM 和路由反射器。  
  
    > [!NOTE]  
    > -   网络控制器已为租户配置 RAS 网关和路由反射器后，每当在相同的租户需要新的站点到站点 VPN 连接，网络控制器将检查此 RAS 网关 VM 上的可用容量。 如果原始网关可以提供服务所需的容量，还相同 RAS 网关 VM 上配置新的网络连接。 如果 RAS 网关 VM 不能处理更多的容量，网络控制器选择新的可用 RAS 网关 VM，并在其上配置新的连接。 这个新的与租户相关联的 RAS 网关 VM 将成为原始租户 RAS 网关路由反射器的路由反射器客户端。  
    > -   因为 RAS 网关池软件负载均衡器 (SLBs) 后，调用每个使用单个公共 IP 地址，称为虚拟的 IP 地址 (VIP)，这将通过 SLBs 转化为数据中心内部的 IP 地址，租户的站点到站点 VPN 地址动态 IP 地址 (DIP) 将流量路由为企业租户 RAS 网关。 通过 SLB 此公共 / 专用 IP 地址映射可确保企业站点和 CSP RAS 网关和路由反射器之间的站点到站点 VPN 隧道正确建立连接。  
    >   
    >     有关 SLB、 Vip 和 Dip 的详细信息，请参阅[软件负载平衡&#40;SLB&#41;用于 SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。  
  
5.  企业站点和 RAS 网关建立为新租户的 CSP 数据中心之间的站点到站点 VPN 隧道后，与隧道相关联的静态路由自动配置上的企业和 CSP 的隧道的两侧.  
  
6.  与 CA 空间 BGP 路由、 eBGP 对等互连企业站点和 CSP RAS 网关路由反射器之间还建立。  
  
## <a name="bkmk_route"></a>路由同步和数据平面路由  
企业网站和 CSP RAS 网关路由反射器之间 eBGP 对等互连建立后，该路由反射器了解到所有企业路由通过使用动态 BGP 路由。 路由反射器同步所有路由反射器客户端之间的这些路由，以便它们都配置与一组相同的路由。  
  
路由反射器还会更新这些合并的路由使用路由同步到网络控制器。 然后，网络控制器路由转换为 HYPER-V 网络虚拟化策略，并配置光纤网络以确保在配置端到端数据路径的路由。 此过程使租户虚拟网络可访问从租户企业站点。  
  
对于数据平面路由，达到 RAS 网关 Vm 的数据包将直接转到租户的虚拟网络，因为所需的路由现可使用的所有参与的 RAS 网关 Vm。  
  
同样，使用 HYPER-V 网络虚拟化策略后，租户虚拟网络路由数据包直接到 RAS 网关 Vm （而不需要了解的有关该路由反射器），再到企业站点通过站点到站点 VPN 隧道.  
  
另外。 从租户虚拟网络到远程租户企业站点的返回流量会绕过 SLBs，名为直接服务器返回 (DSR) 的过程。  
  
## <a name="bkmk_failover"></a>RAS 网关和路由反射器故障转移到网络控制器的响应方式  
以下是两个可能的故障转移方案的另一个用于 RAS 网关路由反射器客户端-，另一个用于 RAS 网关路由反射器包括有关如何网络控制器处理故障转移的 Vm 在配置的信息。  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>RAS 网关 BGP 路由反射器客户端的 VM 故障  
RAS 网关路由反射器客户端失败时，网络控制器将执行以下操作。  
  
> [!NOTE]  
> 当 RAS 网关不是租户的 BGP 基础结构路由反射器时，它是租户的 BGP 基础结构中的路由反射器客户端。  
  
-   网络控制器选择可用的备用 RAS 网关 VM，并预配新的 RAS 网关 VM 的失败的 RAS 网关 VM 配置。  
  
-   网络控制器更新相应的 SLB 配置，以确保站点到站点 VPN 隧道用于从租户站点到失败的 RAS 网关已正确建立与新的 RAS 网关。  
  
-   网络控制器上新的网关配置 BGP 路由反射器客户端。  
  
-   网络控制器将新的 RAS 网关 BGP 路由反射器客户端配置为活动状态。 RAS 网关会立即启动对等互连与租户的路由反射器共享的路由信息和启用 eBGP 对等互连为相应的企业站点。  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>RAS 网关 BGP 路由反射器的 VM 故障  
RAS 网关 BGP 路由反射器失败时，网络控制器将执行以下操作。  
  
-   网络控制器选择可用的备用 RAS 网关 VM，并预配新的 RAS 网关 VM 的失败的 RAS 网关 VM 配置。  
  
-   网络控制器上新的 RAS 网关 VM，配置路由反射器，并将新的 VM 分配失败的 VM，从而提供不考虑 VM 失败路由完整性使用相同的 IP 地址。  
  
-   网络控制器更新相应的 SLB 配置，以确保站点到站点 VPN 隧道用于从租户站点到失败的 RAS 网关已正确建立与新的 RAS 网关。  
  
-   网络控制器配置为活动状态的新的 RAS 网关 BGP 路由反射器 VM。  
  
-   路由反射器立即将变为活动状态。 建立站点到站点 VPN 隧道向企业，以及路由反射器使用 eBGP 对等互连和交换与企业站点路由器的路由。  
  
-   BGP 路由在选择后，RAS 网关 BGP 路由反射器更新租户路由反射器客户端在数据中心，并与网络控制器，用于租户通信提供端到端数据路径同步的路由。  
  
## <a name="bkmk_advantages"></a>使用 RAS 网关的新功能的优点  
以下是几个设计 RAS 网关部署时使用这些新的 RAS 网关功能的优点。  
  
**RAS 网关可伸缩性**  
  
因为您可以添加到 RAS 网关池需要上的 RAS 网关 Vm，你可以轻松地缩放 RAS 网关部署，以优化性能和容量。 时将 Vm 添加到池时，你可以配置这些 RAS 网关使用站点到站点 VPN 连接的任何类型 (IKEv2，L3，GRE)，无需停机时间与消除容量瓶颈。  
  
**简化的企业站点网关管理**  
  
当你的租户具有多个企业站点时，租户可以配置与一个远程站点到站点 VPN IP 地址的所有站点和单个远程邻居 IP 地址-为该租户在 CSP 数据中心 RAS 网关 BGP 路由反射器 VIP。 这简化了你的租户的网关管理。  
  
**快速修正的网关故障**  
  
若要确保快速故障转移响应，可以配置边缘路由和短时间间隔，如小于或等于 10 秒，控制路由器之间的 BGP Keepalive 参数时间。 使用此短保持活动状态的间隔，如果 RAS 网关 BGP 边缘路由器失败，快速检测到故障，网络控制器遵循前面的部分中提供的步骤。 这种优势可能会减少为单独的故障检测协议，如双向转发检测 (BFD) 协议的需要。  
  
 
  


