---
title: 网络函数虚拟化
description: 你可以使用本主题以了解网络函数虚拟化，这允许你部署虚拟网络设备，如 Datacenter 防火墙、租户 RAS 关和软件负载平衡 (SLB) 在 Windows Server 2016。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79df3bbe-48fd-4eff-8df6-35f6317566f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7caa9eef42cb7ab95a13d64c1dcd3639b1132eb3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="network-function-virtualization"></a>网络函数虚拟化

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解网络函数虚拟化，这允许你部署虚拟网络设备，如 Datacenter 防火墙 multitenant RAS 关和软件负载平衡 \(SLB\) 集线器 \(MUX\)。
  
>[!NOTE]  
>本主题中，除了以下网络函数虚拟化文档才可用。  
> - [Datacenter 防火墙概述](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [SDN RAS 网关](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [软件负载平衡 SDN (SLB)](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
在当今的软件定义的数据中心，由硬件设备（如负载平衡防火墙、路由器，开关，以及等）的网络功能已越来越正在虚拟化为虚拟装置。 此"网络函数虚拟化"是的自然而然服务器虚拟化和网络虚拟化。 虚拟装置快速等新兴，并创建一个新市场中。 它们不断生成兴趣获得动力在这两个虚拟化平台和云服务。  
  
Microsoft 为虚拟装置开始与 Windows Server 2012 R2 包含独立网关。 有关详细信息，请参阅[Windows Server 网关](https://technet.microsoft.com/library/dn313101.aspx)。 现在与 Windows Server 2016 Microsoft 会继续展开和在网络函数虚拟化市场投资。  
  
## <a name="virtual-appliance-benefits"></a>虚拟装置权益  
虚拟装置就动态轻松更改，因为它是一个预先生成的自定义虚拟机。 它可以是一个或多个虚拟机打包、更新及作为一个整体保留。 一起的方式使用软件定义的网络 (SDN)，你获取的灵活性和需要当今的基于云的基础结构的灵活性。 例如：  
  
-   SDN 作为共用和动态资源提供该网络。  
  
-   SDN 便于租户隔离。  
  
-   SDN 最大化比例和性能。  
  
-   虚拟装置启用无缝容量扩展和工作负载移动。  
  
-   虚拟装置最小化操作复杂性。  
  
-   虚拟装置让客户轻松地获取、部署和管理预集成的解决方案。  
  
    -   客户可以轻松地将移动虚拟装置任意在云中。  
  
    -   客户可以虚拟装置或规模下动态基于需求。  
  
有关 Microsoft SDN 的详细信息，请参阅[软件定义网络](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-)。  
  
### <a name="what-network-functions-are-being-virtualized"></a>哪些网络功能已被虚拟化？  
对于虚拟化的网络函数商城增长迅速。 下面的网络功能已被虚拟化：  
  
-   **安全**  
  
    -   防火墙  
  
    -   防病毒软件  
  
    -   DDoS（分布式拒绝服务）  
  
    -   IPS/ID（入侵防护/的侵扰检测系统）  
  
-   **应用程序/WAN 优化程序**  
  
-   **Edge**  
  
    -   向站点网关  
  
    -   L3 网关  
  
    -   路由器  
  
    -   切换  
  
    -   NAT  
  
    -   负载平衡（不一定是在 edge 中)  
  
    -   HTTP 代理服务器  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>为什么 Microsoft 是一个出色的平台，虚拟装置  
![虚拟网络堆栈](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
Microsoft 平台已经设计是一个出色的平台，生成并部署虚拟装置。 下面介绍了原因：  
  
-   Microsoft 提供与 Windows Server 2016 的关键虚拟化的网络功能。  
  
-   你可以将你选择的供应商提供的虚拟装置部署。  
  
-   你可以部署、配置，并管理你虚拟设备与它附带 Windows Server 2016 Microsoft 网络控制器。 有关网络控制器的详细信息，请参阅[网络控制器](../../../sdn/technologies/network-controller/Network-Controller.md)。  
  
-   Hyper-V 可以主机你需要顶部的来宾操作系统。  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Windows Server 2016 中的网络功能虚拟化  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>由 Microsoft 提供的虚拟装置函数  
Windows Server 2016 将提供下列虚拟装置：  
  
**软件负载平衡**  
  
4 层负载平衡 datacenter 比例在操作。 这是已在比例 Azure 环境中部署的 Azure 的负载平衡类似版本。 有关 Microsoft 软件负载平衡的详细信息，请参阅[软件负载平衡 (SLB) 的 SDN](https://technet.microsoft.com/library/mt632286.aspx)。 有关 Microsoft Azure 负载平衡服务的详细信息，请参阅[Microsoft Azure 负载平衡服务](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/)。  
  
**网关**。 RAS 网关提供以下网关功能的所有组合。  
  
-   **向站点网关**  
  
    RAS 网关提供边框网关协议 (BGP) 的支持、租户网关，这允许你租户访问，以及通过从远程的站点的站点的 VPN 连接管理他们的资源并允许之间的云和租户物理网络资源虚拟网络流量。 有关 RAS 网关的详细信息，请参阅[RAS 网关高可用性](https://technet.microsoft.com/library/mt631692.aspx)和[RAS 网关](https://technet.microsoft.com/library/mt626650.aspx)。  
  
-   **转移网关**  
  
    RAS 网关路线虚拟网络宿主的提供商物理网络之间的交通。 例如，如果租户创建一个或多个虚拟网络，并且需要对共享资源宿主的提供商处物理网络上的访问权限，转移网关可以路由虚拟网络之间的物理网络提供的工作，虚拟网络提供所需的服务上的用户的通信。 有关详细信息，请参阅[RAS 网关高可用性](https://technet.microsoft.com/library/mt631692.aspx)和[RAS 网关](https://technet.microsoft.com/library/mt626650.aspx)。  
  
-   **GRE 隧道网关**  
  
    GRE 基于隧道启用租户虚拟网络和外部网络之间的连接。 由于 GRE 协议轻型和支持 GRE 是适用于大多数网络设备，它将成为的适合隧道不需要数据加密。 GRE 支持站点 (S2S) 隧道解决租户虚拟网络和租户外部网络使用多租户网关之间的转移的问题。 有关 GRE 隧道的详细信息，请参阅[GRE 隧道在 Windows Server 2016](https://technet.microsoft.com/library/dn765485.aspx)。  
  
**与 BGP 路由控制平面**  
  
Hyper-V 网络虚拟化 (HNV) 路由控制是逻辑、集中实体控制平面，这将继续提供的所有客户地址平面路线和动态了解然后更新中的虚拟网络分布式的 RAS 网关路由器中。 有关详细信息，请参阅[RAS 网关高可用性](https://technet.microsoft.com/library/mt631692.aspx)和[RAS 网关](https://technet.microsoft.com/library/mt626650.aspx)。  
  
**分布式多租户防火墙**  
  
防火墙保护的网络层虚拟网络。 在每个租户 VM SDN vSwitch 端口强制策略。 还可以防止所有通信流：东西和北美南。 通过租户门户推送策略和网络控制器的所有主机适用于分发它们。 分布式多租户防火墙有关详细信息，请参阅[Datacenter 防火墙概述](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。  
  


