---
title: 网络功能虚拟化
description: 你可以使用本主题了解网络功能虚拟化功能，它允许你在 Windows Server 2016 中部署数据中心防火墙、多租户 RAS 网关和软件负载平衡（SLB）等虚拟网络设备。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79df3bbe-48fd-4eff-8df6-35f6317566f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 338d5a285f2524932a91a66db186554cd0f50e2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355658"
---
# <a name="network-function-virtualization"></a>网络功能虚拟化

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题了解网络功能虚拟化功能，它允许你部署虚拟网络设备（例如数据中心防火墙）、多租户 RAS 网关和软件负载平衡 \(SLB @ @no__t no__t_t-3。
  
>[!NOTE]  
>除了本主题之外，还提供了以下网络功能虚拟化文档。  
> - [数据中心防火墙概述](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [用于 SDN 的 RAS 网关](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [用于 SDN 的软件负载均衡 (SLB)](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
在当今的软件定义数据中心，硬件设备（例如负载均衡器、防火墙、路由器、交换机等）正在执行的网络功能越来越多地虚拟化为虚拟设备。 服务器虚拟化和网络虚拟化自然而然地形成了这种“网络功能虚拟化”。 虚拟设备迅速涌现，并创建全新市场。 它们会在虚拟化平台和云服务中不断产生兴趣并获得动力。  
  
Microsoft 将独立网关作为虚拟设备包含在 Windows Server 2012 R2 的基础上。 有关详细信息，请参阅 [Windows Server 网关](https://technet.microsoft.com/library/dn313101.aspx)。 现在，在 Windows Server 2016 中，Microsoft 继续扩展并投入网络功能虚拟化市场。  
  
## <a name="virtual-appliance-benefits"></a>虚拟设备权益  
虚拟设备是一种预先构建的自定义虚拟机，因为它是一个预先构建的自定义虚拟机。 它可以是一个或多个作为一个单元打包、更新和维护的虚拟机。 与软件定义的网络（SDN）结合在一起，可以获得当今基于云的基础结构所需的灵活性和灵活性。 例如：  
  
-   SDN 显示网络作为共用和动态资源。  
  
-   SDN 简化了租户隔离。  
  
-   SDN 最大限度地提高了规模和性能。  
  
-   虚拟设备支持无缝容量扩展和工作负载移动性。  
  
-   虚拟设备将操作复杂性降到最低。  
  
-   虚拟设备让客户可以轻松地获取、部署和管理预先集成的解决方案。  
  
    -   客户可以轻松地将虚拟设备移到云中的任何位置。  
  
    -   客户可根据需要动态缩放虚拟设备。  
  
有关 Microsoft SDN 的详细信息，请参阅[软件定义的网络](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-)。  
  
### <a name="what-network-functions-are-being-virtualized"></a>正在虚拟化哪些网络函数？  
虚拟化网络功能的 marketplace 正在迅速增长。 正在虚拟化以下网络函数：  
  
-   **安全性**  
  
    -   防火墙  
  
    -   防病毒  
  
    -   DDoS （分布式拒绝服务）  
  
    -   IPS/IDS （入侵防御系统/入侵检测系统）  
  
-   **应用程序/WAN 优化器**  
  
-   **底端**  
  
    -   站点到站点网关  
  
    -   L3 网关  
  
    -   路由器  
  
    -   交换器  
  
    -   NAT  
  
    -   负载均衡器（不一定是边缘）  
  
    -   HTTP 代理  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>为什么 Microsoft 是适用于虚拟设备的绝佳平台  
![虚拟网络堆栈](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
Microsoft 平台旨在成为构建和部署虚拟设备的绝佳平台。 原因是：  
  
-   Microsoft 在 Windows Server 2016 中提供密钥虚拟网络功能。  
  
-   你可以从所选供应商处部署虚拟设备。  
  
-   你可以通过 Windows Server 2016 附带的 Microsoft 网络控制器部署、配置和管理虚拟设备。 有关网络控制器的详细信息，请参阅[网络控制器](../../../sdn/technologies/network-controller/Network-Controller.md)。  
  
-   Hyper-v 可承载所需的顶级来宾操作系统。  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Windows Server 2016 中的网络功能虚拟化  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Microsoft 提供的虚拟设备功能  
Windows Server 2016 附带了以下虚拟设备：  
  
**软件负载均衡器**  
  
数据中心规模的第4层负载均衡器。 这是 azure 环境中大规模部署的 Azure 负载均衡器的类似版本。 有关 Microsoft 软件负载平衡器的详细信息，请参阅[SDN 的软件负载平衡（SLB）](https://technet.microsoft.com/library/mt632286.aspx)。 有关 Microsoft Azure 负载平衡服务的详细信息，请参阅[Microsoft Azure 负载平衡服务](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/)。  
  
**网关**。 RAS 网关提供以下网关功能的所有组合。  
  
-   **站点到站点网关**  
  
    RAS 网关提供支持边界网关协议（BGP）的多租户网关，该网关使你的租户能够通过站点到站点 VPN 连接访问和管理其资源，并允许在虚拟资源之间流动网络流量在云和租户的物理网络中。 有关 RAS 网关的详细信息，请参阅[Ras 网关高可用性](https://technet.microsoft.com/library/mt631692.aspx)和[ras 网关](https://technet.microsoft.com/library/mt626650.aspx)。  
  
-   **转发网关**  
  
    RAS 网关在虚拟网络与托管提供商物理网络之间路由流量。 例如，如果租户创建一个或多个虚拟网络，并且需要访问托管提供商上的物理网络上的共享资源，则转发网关可以在虚拟网络与物理网络之间路由流量，以使用户能够工作具有所需服务的虚拟网络。 有关详细信息，请参阅[Ras 网关高可用性](https://technet.microsoft.com/library/mt631692.aspx)和[ras 网关](https://technet.microsoft.com/library/mt626650.aspx)。  
  
-   **GRE 隧道网关**  
  
    基于 GRE 的隧道可在租户虚拟网络与外部网络之间实现连接。 由于 GRE 协议是轻型协议，因此大多数网络设备上都提供了对 GRE 的支持，因此它成为不需要数据加密的隧道的理想选择。 站点到站点 (S2S) 隧道中的 GRE 支持可解决租户虚拟网络与使用多租户网关的租户外部网络之间的转发问题。 有关 GRE 隧道的详细信息，请参阅[Windows Server 2016 中的 Gre 隧道](https://technet.microsoft.com/library/dn765485.aspx)。  
  
**具有 BGP 的路由控制平面**  
  
Hyper-v 网络虚拟化（HNV）路由控制是控制平面中的逻辑集中实体，它携带所有客户地址平面路由，并动态学习并更新虚拟网络中的分布式 RAS 网关路由器。 有关详细信息，请参阅[Ras 网关高可用性](https://technet.microsoft.com/library/mt631692.aspx)和[ras 网关](https://technet.microsoft.com/library/mt626650.aspx)。  
  
**分布式多租户防火墙**  
  
防火墙保护虚拟网络的网络层。 在每个租户 VM 的 SDN-vSwitch 端口上强制实施这些策略。 它保护所有通信流：东-西和北-南。 策略通过租户门户推送，网络控制器将其分发给所有适用的主机。 有关分布式多租户防火墙的详细信息，请参阅[数据中心防火墙概述](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。  
  


