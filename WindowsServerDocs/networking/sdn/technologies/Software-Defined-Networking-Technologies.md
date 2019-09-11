---
title: SDN 技术
description: 本节中的主题提供有关 Windows Server 2016 中包含的软件定义的网络技术的概述和技术信息。
manager: dougkim
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: pashort
author: shortpatti
ms.date: 02/14/2019
ms.openlocfilehash: 040568dd696c4dee665de415d23ce9a7cdc8a711
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870075"
---
# <a name="sdn-technologies"></a>SDN 技术

>适用于：Windows Server 2019、Windows Server 2016、Windows Server（半年频道）

本节中的主题提供有关 Windows Server 2016 中包含的软件定义的网络技术的概述和技术信息。  

## <a name="network-controllernetwork-controllernetwork-controllermd"></a>[网络控制器](network-controller/Network-Controller.md)

网络控制器提供集中的可编程点，可用于管理、配置、监视和诊断数据中心中的虚拟和物理网络基础结构。 通过网络控制器，可以自动配置网络基础结构，而不是执行网络设备和服务的手动配置。 

网络控制器是高度可用且可缩放的服务器，提供两个应用程序编程接口（Api）：

1. **SOUTHBOUND API** –允许网络控制器与网络通信。
2. **NORTHBOUND API** –允许与网络控制器通信。

你可以使用 Windows PowerShell、具象状态传输（REST） API 或管理应用程序来管理以下物理和虚拟网络基础结构：

- Hyper-V VM 和虚拟交换机 
- 物理网络交换机 
- 物理网络路由器 
- 防火墙软件 
- VPN 网关，包括远程访问服务（RAS）多租户网关 
- 负载平衡器 
  
## <a name="hyper-v-network-virtualizationhyper-v-network-virtualizationhyper-v-network-virtualizationmd"></a>[Hyper-v 网络虚拟化](hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

Hyper-v 网络虚拟化（HNV）可帮助你使用虚拟网络从物理网络中抽象应用程序和工作负荷。 在共享物理网络结构中运行时，虚拟网络提供所需的多租户隔离，从而提高资源利用率。 若要确保可以继续现有的投资，可以在现有网络设备上设置虚拟网络。 此外，虚拟网络与虚拟局域网（Vlan）兼容。
  
## <a name="hyper-v-virtual-switchvirtualizationhyper-v-virtual-switchhyper-v-virtual-switchmd"></a>[Hyper-V 虚拟交换机](../../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md) 

Hyper-v 虚拟交换机是基于软件的第2层以太网网络交换机，安装 Hyper-v 服务器角色后，Hyper-v 管理器中提供了该交换机。 交换机包括以编程方式管理的功能和扩展功能，可将虚拟机同时连接到虚拟网络和物理网络。 此外，Hyper-v 虚拟交换机提供安全、隔离和服务级别的策略实施。
  
你还可以使用交换机嵌入式组合（SET）和远程直接内存访问（RDMA）部署 Hyper-v 虚拟交换机。 有关详细信息，请参阅本主题中的[远程直接内存访问（RDMA）和交换机嵌入式组合（SET）](#remote-direct-memory-access-rdma-and-switch-embedded-teaming-set)部分。

## <a name="internal-dns-service-idns-for-sdnidns-for-sdnmd"></a>[SDN 的内部 DNS 服务（Idn）](Idns-for-Sdn.md)

托管的虚拟机（Vm）和应用程序需要 DNS 在其网络内以及 Internet 上的外部资源进行通信。 借助 Idn，你可以为租户提供 DNS 名称解析服务，用于隔离的本地命名空间和 Internet 资源。 
  
## <a name="network-function-virtualizationnetwork-function-virtualizationnetwork-function-virtualizationmd"></a>[网络功能虚拟化](network-function-virtualization/Network-Function-Virtualization.md)

硬件设备（例如负载均衡器、防火墙、路由器和交换机）逐渐成为虚拟设备。 Microsoft 已经虚拟化网络、交换机、网关、Nat、负载均衡器和防火墙。 服务器虚拟化和网络虚拟化自然而然地形成了这种“网络功能虚拟化”。 虚拟设备迅速涌现，并创建全新市场。 它们会在虚拟化平台和云服务中不断产生兴趣并获得动力。 
  
以下网络功能虚拟化技术可用。  
  
-   **软件负载平衡器（SLB）和网络地址转换（NAT）** 。 通过支持直接服务器返回，使返回的网络流量可以绕过负载平衡多路复用器来提高吞吐量。 有关更多详细信息，请参阅[SDN 的软件负载平衡/（SLB/）](network-function-virtualization/software-load-balancing-for-sdn.md)。
  
-   **数据中心防火墙**。 提供粒度访问控制列表（Acl），使你能够在 VM 接口级别或子网级别应用防火墙策略。 有关更多详细信息，请参阅[数据中心防火墙概述](network-function-virtualization/Datacenter-Firewall-Overview.md)。
  
-   **用于 SDN 的 RAS 网关**。 在物理网络与 VM 网络资源之间路由网络流量，而不考虑位置。 您可以将网络流量路由到同一物理位置或多个不同的位置。 有关更多详细信息，请参阅[SDN 的 RAS 网关](network-function-virtualization/RAS-Gateway-for-SDN.md)。

## <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)  
在 Windows Server 2016 中，可以在绑定到 Hyper-v 虚拟交换机的网络适配器上启用 RDMA，而无需切换嵌入组合（SET）。 这使你可以使用更少的网络适配器，同时想要使用 RDMA 并进行设置。  
  
SET 是一种备用 NIC 组合解决方案，可用于在 Windows Server 2016 中包含 Hyper-v 的环境和软件定义的网络（SDN）堆栈。 将一些 NIC 组合功能集成到 Hyper-v 虚拟交换机。  
  
集允许在一个或多个基于软件的虚拟网络适配器之间分组到8个物理以太网网络适配器。 这些虚拟网络适配器可以提高性能，并在网络适配器发生故障时提供容错能力。  
设置成员网络适配器必须全部安装在要放置在团队中的同一物理 Hyper-v 主机中。  
  
此外，还可以使用 Windows PowerShell 命令启用数据中心桥接（DCB）、创建具有 RDMA 虚拟 NIC （vNIC）的 Hyper-v 虚拟交换机，并使用 SET 和 RDMA Vnic 创建 Hyper-v 虚拟交换机。 有关详细信息，请参阅[远程直接内存访问（RDMA）和交换机嵌入式组合（SET）](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md)。

## <a name="border-gateway-protocol-bgpremoteremote-accessbgpborder-gateway-protocol-bgpmd"></a>[边界网关协议 (BGP)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)
  
边界网关协议（BGP）是一种动态路由协议，可自动了解使用站点到站点 VPN 连接的站点之间的路由。 因此，BGP 会减少路由器的手动配置。   当你配置 RAS 网关时，BGP 使你能够管理租户的 VM 网络和远程站点之间的网络流量路由。  
  
## <a name="software-load-balancing-slb-for-sdnnetwork-function-virtualizationsoftware-load-balancing-for-sdnmd"></a>[用于 SDN 的软件负载均衡 (SLB)](network-function-virtualization/software-load-balancing-for-sdn.md)
云服务提供商（Csp）和部署 SDN 的企业可以使用软件负载平衡（SLB）在虚拟网络资源之间均匀分配租户和租户客户网络流量。 Windows Server SLB 允许多台服务器承载相同的工作负荷，具有较高的可用性和可扩展性。 

## <a name="windows-server-containerscontainerscontainer-networking-overviewmd"></a>[Windows Server 容器](Containers/Container-networking-overview.md)

Windows Server 容器是一种轻型操作系统虚拟化方法，用于将应用程序或服务与相同容器主机上运行的其他服务区分开来。 每个容器都有自己的操作系统、进程、文件系统、注册表和 IP 地址，可以连接到虚拟网络。 

## <a name="system-center"></a>System Center

通过[虚拟机管理（VMM）](https://docs.microsoft.com/system-center/vmm/)和[Operations Manager](https://docs.microsoft.com/system-center/scom/)部署和管理 SDN 基础结构。 可以使用 VMM 预配和管理所需资源，以便创建虚拟机和服务并将其部署到私有云。  可以使用 Operations Manager 监视整个企业的服务、设备和运营，在确定问题后立即行动。 


---