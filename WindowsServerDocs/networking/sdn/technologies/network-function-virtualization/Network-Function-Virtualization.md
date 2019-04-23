---
title: 网络功能虚拟化
description: 可以使用本主题以了解有关网络功能虚拟化，可用于部署数据中心防火墙、 多租户 RAS 网关和软件负载平衡 (SLB) 在 Windows Server 2016 中的虚拟网络设备。
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
ms.openlocfilehash: 59474a13d1cbce6a607f025caf3f6c1b839c7eed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884548"
---
# <a name="network-function-virtualization"></a>网络功能虚拟化

>适用于：Windows 服务器 （半年频道），Windows Server 2016

你可以使用本主题以了解有关网络功能虚拟化，可用于部署虚拟网络设备，例如数据中心防火墙、 多租户 RAS 网关和软件负载平衡\(SLB\)多路复用器\(MUX\)。
  
>[!NOTE]  
>除了本主题提供了以下网络功能虚拟化文档。  
> - [数据中心防火墙概述](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [用于 SDN 的 RAS 网关](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [软件负载平衡 (SLB) 实现 SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
在当今的软件定义的数据中心，由硬件设备 （如负载均衡器、 防火墙、 路由器、 交换机等） 正在执行的网络功能是越来越多地被虚拟化作为虚拟设备。 服务器虚拟化和网络虚拟化自然而然地形成了这种“网络功能虚拟化”。 虚拟设备快速按新出现的和创建新的市场。 它们继续生成感兴趣和获取这两个虚拟化平台中的动量和云服务。  
  
Microsoft 从 Windows Server 2012 R2 的虚拟设备作为包含独立网关。 有关详细信息，请参阅 [Windows Server 网关](https://technet.microsoft.com/library/dn313101.aspx)。 现在使用 Windows Server 2016 Microsoft 继续展开和网络功能虚拟化市场上投资。  
  
## <a name="virtual-appliance-benefits"></a>虚拟设备的好处  
虚拟设备是动态且易于更改，因为它是预建的自定义虚拟机。 它可以是一个或多个虚拟机一起打包，更新，并作为一个单元进行维护。 一起使用软件定义的网络 (SDN)，则会获得敏捷性和今天的基于云的基础结构中所需的灵活性。 例如：  
  
-   SDN 提供网络为共用和动态资源。  
  
-   SDN 便于租户隔离。  
  
-   可最大化 SDN，规模和性能。  
  
-   虚拟设备启用无缝容量扩展和工作负荷移动性。  
  
-   虚拟设备最大程度减少操作复杂性。  
  
-   虚拟设备让客户轻松地获取、 部署和管理预集成的解决方案。  
  
    -   客户可以轻松移动虚拟设备任意位置在云中。  
  
    -   客户可以扩展虚拟设备或向下动态根据需求。  
  
有关 Microsoft SDN 的详细信息请参阅[软件定义的网络](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-)。  
  
### <a name="what-network-functions-are-being-virtualized"></a>哪些网络功能是被虚拟化？  
面向虚拟化的网络功能的市场正在快速增长。 以下网络功能是被虚拟化：  
  
-   **安全性**  
  
    -   防火墙  
  
    -   防病毒  
  
    -   DDoS （分布式拒绝服务）  
  
    -   IPS/IDS （入侵防护系统/入侵检测系统）  
  
-   **应用程序/WAN 优化器**  
  
-   **Edge**  
  
    -   站点到站点网关  
  
    -   L3 网关  
  
    -   路由器  
  
    -   开关  
  
    -   NAT  
  
    -   负载均衡器 （不一定是在边缘）  
  
    -   HTTP 代理  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>Microsoft 为什么是虚拟设备的强大平台  
![虚拟网络堆栈](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
Microsoft 平台就被设计为一个优秀的平台来构建和部署虚拟设备。 原因是：  
  
-   Microsoft 提供了与 Windows Server 2016 关键虚拟化的网络功能。  
  
-   你可以部署所选的虚拟设备供应商。  
  
-   你可以部署、 配置和管理你与 Microsoft 网络控制器，其中随附了 Windows Server 2016 的虚拟设备。 有关网络控制器的详细信息，请参阅[网络控制器](../../../sdn/technologies/network-controller/Network-Controller.md)。  
  
-   HYPER-V 可以主机上的来宾操作系统所需的。  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Windows Server 2016 中的网络功能虚拟化  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>由 Microsoft 提供的虚拟设备函数  
Windows Server 2016 提供了以下虚拟设备：  
  
**软件负载均衡器**  
  
第 4 层负载均衡器在数据中心大规模运行。 这是 Azure 的负载均衡器已在 Azure 环境中大规模部署的类似版本。 有关 Microsoft 软件负载均衡器的详细信息，请参阅[软件负载平衡 (SLB) 用于 SDN](https://technet.microsoft.com/library/mt632286.aspx)。 有关 Microsoft Azure 负载均衡服务的详细信息，请参阅[Microsoft Azure 负载均衡服务](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/)。  
  
**网关**。 RAS 网关提供以下网关函数的所有组合。  
  
-   **站点到站点网关**  
  
    RAS 网关提供了边界网关协议 (BGP) 的支持、 多租户网关，允许租户在访问和管理其资源通过站点到站点 VPN 连接从远程站点，这将允许虚拟资源之间的网络流量流中的云和租户的物理网络。 有关 RAS 网关的详细信息，请参阅[RAS 网关高可用性](https://technet.microsoft.com/library/mt631692.aspx)并[RAS 网关](https://technet.microsoft.com/library/mt626650.aspx)。  
  
-   **转发网关**  
  
    RAS 网关的虚拟网络和托管提供商物理网络之间路由流量。 例如，如果租户创建一个或多个虚拟网络，并且需要在托管提供商在物理网络上的共享资源的访问权限，转发网关可以路由虚拟网络与物理网络来提供工作的用户之间的流量具有所需的服务的虚拟网络。 有关详细信息，请参阅[RAS 网关高可用性](https://technet.microsoft.com/library/mt631692.aspx)并[RAS 网关](https://technet.microsoft.com/library/mt626650.aspx)。  
  
-   **GRE 隧道网关**  
  
    基于 GRE 的隧道可在租户虚拟网络与外部网络之间实现连接。 由于 GRE 协议是轻型和支持 GRE 是大多数网络设备上可用，它将成为不需要的数据加密的隧道的理想之选。 站点到站点 (S2S) 隧道中的 GRE 支持可解决租户虚拟网络与使用多租户网关的租户外部网络之间的转发问题。 GRE 隧道的详细信息，请参阅[Windows Server 2016 中的 GRE 隧道](https://technet.microsoft.com/library/dn765485.aspx)。  
  
**使用 BGP 路由控制平面**  
  
HYPER-V 网络虚拟化 (HNV) 路由控件是控制平面，其中包含所有客户地址平面路由和动态了解到，然后更新虚拟网络中的分布式的 RAS 网关路由器中的逻辑、 集中式实体。 有关详细信息，请参阅[RAS 网关高可用性](https://technet.microsoft.com/library/mt631692.aspx)并[RAS 网关](https://technet.microsoft.com/library/mt626650.aspx)。  
  
**分布式多租户防火墙**  
  
防火墙保护的网络层的虚拟网络。 在每个租户 VM 的 SDN vSwitch 端口强制实施策略。 它可以保护所有通信流： 东-西和北-南。 通过租户门户推送策略和网络控制器将将其分发到所有可用主机。 有关分布式的多租户防火墙的详细信息，请参阅[数据中心防火墙概述](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。  
  


