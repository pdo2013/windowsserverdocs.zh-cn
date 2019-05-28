---
title: SDN 技术
description: 在本部分中的主题提供概述和有关包括在 Windows Server 2016 中软件定义的网络技术的技术信息。
manager: dougkim
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: pashort
author: shortpatti
ms.date: 02/14/2019
ms.openlocfilehash: 0fc076eaeacc98c5554f44ae6177a3a8b8286647
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034644"
---
# <a name="sdn-technologies"></a>SDN 技术

>适用于：Windows Server 2019，Windows Server 2016 中，Windows Server （半年频道）

在本部分中的主题提供概述和有关包括在 Windows Server 2016 中软件定义的网络技术的技术信息。  

## <a name="network-controllernetwork-controllernetwork-controllermd"></a>[网络控制器](network-controller/Network-Controller.md)

网络控制器提供集中的可编程点，自动化来管理、 配置、 监视和故障排除虚拟和物理网络基础结构中你的数据中心。 与网络控制器可以自动执行网络基础结构而不是执行手动配置网络设备和服务的配置。 

网络控制器是高度可用且可伸缩的服务器，并提供两个应用程序编程接口 (Api):

1. **Southbound API** – 使网络控制器可以与网络通信。
2. **Northbound API** – 可以与网络控制器进行通信。

可以使用 Windows PowerShell、 具象状态传输 (REST) API 或管理应用程序来管理以下物理和虚拟网络基础结构：

- Hyper-V VM 和虚拟交换机 
- 物理网络交换机 
- 物理网络路由器 
- 防火墙软件 
- VPN 网关，包括远程访问服务 (RAS) 多租户网关 
- 负载平衡器 
  
## <a name="hyper-v-network-virtualizationhyper-v-network-virtualizationhyper-v-network-virtualizationmd"></a>[HYPER-V 网络虚拟化](hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

HYPER-V 网络虚拟化 (HNV) 可帮助你通过使用虚拟网络抽象应用程序和物理网络中的工作负荷。 在共享物理网络结构中运行时，虚拟网络提供所需的多租户隔离，从而提高资源利用率。 若要确保，你可以推进现有投资，你可以设置现有网络设备上的虚拟网络。 此外，虚拟网络是与虚拟局域网 (Vlan) 兼容。
  
## <a name="hyper-v-virtual-switchvirtualizationhyper-v-virtual-switchhyper-v-virtual-switchmd"></a>[Hyper-V 虚拟交换机](../../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md) 

HYPER-V 虚拟交换机是基于软件的第 2 层以太网网络交换机已安装了 HYPER-V 服务器角色后在 Hyper-v 管理器中提供的。 交换机包括以编程方式管理的功能和扩展功能，可将虚拟机同时连接到虚拟网络和物理网络。 此外，HYPER-V 虚拟交换机提供安全、 隔离和服务级别的策略执行。
  
您还可以部署使用交换机嵌入式组合 (SET) 和远程直接内存访问 (RDMA) 的 HYPER-V 虚拟交换机。 有关详细信息，请参阅明[远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)](#remote-direct-memory-access-rdma-and-switch-embedded-teaming-set)本主题中。

## <a name="internal-dns-service-idns-for-sdnidns-for-sdnmd"></a>[用于 SDN 的内部 DNS 服务 (Idn)](Idns-for-Sdn.md)

托管的虚拟机 (Vm) 和应用程序需要 DNS 其网络中以及与 Internet 上的外部资源进行通信。 使用 Idn，您可以提供为独立的本地命名空间和 Internet 资源的 DNS 名称解析服务的租户。 
  
## <a name="network-function-virtualizationnetwork-function-virtualizationnetwork-function-virtualizationmd"></a>[网络功能虚拟化](network-function-virtualization/Network-Function-Virtualization.md)

硬件设备，例如负载均衡器、 防火墙、 路由器和交换机正在逐渐成为虚拟设备。 Microsoft 具有虚拟网络、 交换机、 网关、 Nat、 负载均衡器和防火墙。 服务器虚拟化和网络虚拟化自然而然地形成了这种“网络功能虚拟化”。 虚拟设备快速按新出现的和创建新的市场。 它们继续生成感兴趣和获取这两个虚拟化平台中的动量和云服务。 
  
提供以下网络功能虚拟化技术。  
  
-   **软件负载均衡器 (SLB) 和网络地址转换 (NAT)** 。 通过支持直接服务器返回在其中返回网络流量可以绕过负载均衡多路复用器来提高吞吐量。 有关更多详细信息，请参阅[用于 SDN 软件负载平衡 /(SLB/)](network-function-virtualization/software-load-balancing-for-sdn.md)。
  
-   **数据中心防火墙**。 提供精细的访问控制列表 (Acl)，使你能够将应用在 VM 接口级别或子网级别的防火墙策略。 有关更多详细信息，请参阅[数据中心防火墙概述](network-function-virtualization/Datacenter-Firewall-Overview.md)。
  
-   **用于 SDN 的 RAS 网关**。 物理网络和 VM 网络资源，而不考虑位置之间路由网络流量。 你可以路由相同的物理位置或多个不同位置的网络流量。 有关更多详细信息，请参阅[用于 SDN 的 RAS 网关](network-function-virtualization/RAS-Gateway-for-SDN.md)。

## <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)  
在 Windows Server 2016 中，可以绑定到 HYPER-V 虚拟交换机或无需切换嵌入组合 (SET) 的网络适配器上启用 RDMA。 因此，可以使用更少的网络适配器，当你想要使用 RDMA 和 SET 使在同一时间。  
  
集是一个可以在 Windows Server 2016 中包括的 HYPER-V 和软件定义网络 (SDN) 堆栈的环境中使用的替代 NIC 组合解决方案。 组将某些 NIC 组合功能集成到 HYPER-V 虚拟交换机。  
  
组可以 1 至 8 物理以太网网络适配器之间到一个或多个基于软件的虚拟网络适配器组。 这些虚拟网络适配器可以提高性能，并在网络适配器发生故障时提供容错能力。  
组成员的网络适配器必须全部安装在同一物理 HYPER-V 主机来放置在一个组。  
  
此外，可以使用 Windows PowerShell 命令以启用数据中心桥接 (DCB)、 使用 RDMA 虚拟 NIC (vNIC)，创建 HYPER-V 虚拟交换机和使用 SET 和 RDMA Vnic 创建 HYPER-V 虚拟交换机。 有关详细信息，请参阅[远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md)。

## <a name="border-gateway-protocol-bgpremoteremote-accessbgpborder-gateway-protocol-bgpmd"></a>[边界网关协议 (BGP)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)
  
边界网关协议 (BGP) 是自动学习使用站点到站点 VPN 连接的站点之间的路由的动态路由协议。 因此，BGP 可减少手动配置路由器。   在配置 RAS 网关时，BGP 允许你管理的租户的 VM 网络和远程站点之间的网络流量的路由。  
  
## <a name="software-load-balancing-slb-for-sdnnetwork-function-virtualizationsoftware-load-balancing-for-sdnmd"></a>[用于 SDN 的软件负载均衡 (SLB)](network-function-virtualization/software-load-balancing-for-sdn.md)
云服务提供商 (Csp) 和企业部署 SDN 可以使用软件负载平衡 (SLB) 均匀分配租户和租户客户在虚拟网络资源之间的网络流量。 Windows Server SLB 允许多台服务器承载相同的工作负荷，具有较高的可用性和可扩展性。 

## <a name="windows-server-containerscontainerscontainer-networking-overviewmd"></a>[Windows Server 容器](Containers/Container-networking-overview.md)

Windows Server 容器是轻量操作系统虚拟化方法，将应用程序或服务从运行在同一容器主机上的其他服务分离出来。 每个容器具有其自己的操作系统、 进程、 文件系统、 注册表和 IP 地址，你可以连接到虚拟网络。 

## <a name="system-center"></a>System Center

部署和管理 SDN 基础结构与[虚拟机管理 (VMM)](https://docs.microsoft.com/system-center/vmm/)并[Operations Manager](https://docs.microsoft.com/system-center/scom/)。 使用 VMM，可以预配和管理进行创建和部署到私有云的虚拟机和服务所需的资源。  借助 Operations Manager，以确定立即采取措施的问题在企业范围内监视服务、 设备和操作。 


---