---
title: 网络的新增功能
description: 本主题提供有关 Windows Server 2016 中用于网络的新功能和技术的概述信息
ms.prod: windows-server
ms.technology: networking
ms.topic: get-started-article
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: da2166d28edda5662797824d9b26ad930f51083c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406759"
---
# <a name="whats-new-in-networking"></a>网络的新增功能

>适用于：Windows Server 2016

下面是 Windows Server 2016 中新增或增强的网络技术。  
  Upd 本主题包含以下各节。  
  
-   [新的网络功能和技术](#bkmk_features)  
  
-   [其他网络技术的新增功能](#bkmk_existing)  
  
## <a name="bkmk_features"></a>新的网络功能和技术

网络是软件定义数据中心（SDDC）平台的基础部分，而 Windows Server 2016 提供了新的和经过改进的软件定义网络（SDN）技术，可帮助你迁移到适用于你的组织的完全实现的 SDDC 解决方案。  
  
作为软件定义的资源管理网络时，可以一次描述应用程序的基础结构要求，然后选择应用程序在本地或云中运行的位置。  这种一致性意味着你的应用程序现在更易于缩放，你可以在任何地方无缝地运行应用程序，并且可以在安全性、性能、服务质量和可用性方面提供信心。  
  
以下部分包含有关这些新的网络功能和技术的信息。  
  
### <a name="software-defined-networking-infrastructure"></a>软件定义的网络基础结构

下面是新增或改进的 SDN 基础结构技术。  
  
-   **网络控制器**。 Windows Server 2016 中的新增功能，网络控制器提供集中的可编程点，可用于管理、配置、监视和排查数据中心中的虚拟和物理网络基础结构。 使用网络控制器可以自动配置网络基础结构，而无需手动执行网络设备和服务的配置。 有关详细信息，请参阅[网络控制器](sdn/technologies/network-controller/Network-Controller.md)和[使用脚本部署软件定义的网络](https://technet.microsoft.com/library/mt427380.aspx)。  
  
-   **Hyper-v 虚拟交换机**。 Hyper-v 虚拟交换机在 Hyper-v 主机上运行，并允许你创建分布式切换和路由，以及与 Microsoft Azure 对齐和兼容的策略实施层。 有关详细信息，请参阅 [Hyper-V 虚拟交换机](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。  
  
-   **网络功能虚拟化（NFV）** 。 在当今的软件定义数据中心，硬件设备（例如负载均衡器、防火墙、路由器、交换机等）正在执行的网络功能越来越多地部署为虚拟设备。 服务器虚拟化和网络虚拟化自然而然地形成了这种“网络功能虚拟化”。 虚拟设备迅速涌现，并创建全新市场。 它们会在虚拟化平台和云服务中不断产生兴趣并获得动力。 Windows Server 2016 中提供了以下 NFV 技术。  
  
    -   **数据中心防火墙**。 此分布式防火墙提供粒度访问控制列表（Acl），使你能够在 VM 接口级别或子网级别应用防火墙策略。  
  
        有关详细信息，请参阅[数据中心防火墙概述](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。  
  
    -   **RAS 网关**。 可以使用 RAS 网关在虚拟网络与物理网络之间路由流量，包括从云数据中心到租户远程站点之间的站点到站点 VPN 连接。 具体而言，可以部署 Internet 密钥交换版本2（IKEv2）站点到站点虚拟专用网络（Vpn）、第3层（L3） VPN 和通用路由封装（GRE）网关。 此外，现在支持网关池和 M + N 冗余的网关;带有路由反射器功能的和边界网关协议（BGP）为所有网关方案（IKEv2 VPN、GRE VPN 和 L3 VPN）提供网络之间的动态路由。  
  
        有关详细信息，请参阅[Ras 网关中的新增功能](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)和[SDN 的 ras 网关](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)。  
          
    - **软件负载平衡器（SLB）和网络地址转换（NAT）** 。 北南部和东-西第4层负载均衡器和 NAT 通过支持直接服务器返回来增强吞吐量，使返回的网络流量可以绕过负载平衡多路复用器。  
       有关详细信息，请参阅用于[SDN &#40;的&#41;软件负载平衡 SLB](sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。  
  
    有关详细信息，请参阅[网络功能虚拟化](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)。  
  
-   **标准化协议**。 网络控制器将其 northbound 接口上的具象状态传输（REST）用于 JavaScript 对象表示法（JSON）负载。 网络控制器 southbound 接口使用开放式 vSwitch 数据库管理协议（OVSDB）。  
  
-   **灵活的封装技术**。 这些技术在数据平面运行，同时支持虚拟可扩展 LAN （VxLAN）和网络虚拟化通用路由封装（NVGRE）。 有关详细信息，请参阅[Windows Server 2016 中的 GRE 隧道](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
有关 SDN 的详细信息，请参阅[软件定义&#40;的&#41;网络 SDN](sdn/software-defined-networking.md)。  
  
### <a name="cloud-scale-fundamentals"></a>云规模基础知识
 
现在提供了以下云缩放基础知识。  
  
-   **聚合网络接口卡（NIC）** 。 聚合 NIC 允许使用单个网络适配器进行管理、远程直接内存访问（RDMA）启用的存储和租户通信。 这减少了与数据中心中的每个服务器关联的资本支出，因为需要较少的网络适配器来管理每个服务器的不同类型的流量。  
  
-   **Packet Direct**。  Packet Direct 提供高网络流量吞吐量和低延迟数据包处理基础结构。  
  
-   **切换嵌入组合（SET）** 。        集是在 Hyper-v 虚拟交换机中集成的 NIC 组合解决方案。 集允许将最多8个物理 NIC 组合成单个集组，从而提高可用性并提供故障转移。 在 Windows Server 2016 中，你可以创建限制为使用服务器消息块（SMB）和 RDMA 的集团队。 此外，你可以使用 "设置团队" 为 Hyper-v 网络虚拟化分发网络流量。 有关详细信息，请[参阅远程直接内存&#40;访问&#41; RDMA 和交换机嵌入式&#40;组合&#41;集](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。  
  
## <a name="bkmk_existing"></a>其他网络技术的新增功能

本部分包含有关熟悉的网络技术的新功能的信息。
  
## <a name="bkmk_dhcp"></a>台  
DHCP 是一项 Internet 工程任务组 (IETF) 标准，旨在减轻在基于 TCP/IP 的网络（如私人内部网）上配置主机的管理负担并降低复杂度。 使用 DHCP 服务器服务，在 DHCP 客户端上配置 TCP/IP 的过程是自动进行的。  
  
有关详细信息，请参阅[DHCP 中的新增功能](technologies/dhcp/What-s-New-in-DHCP.md)。  
  
## <a name="bkmk_dns"></a>DNS  
DNS 是 TCP/IP 网络中用于命名计算机和网络服务的系统。 DNS 命名通过用户友好的名称定位计算机和服务。 当用户在应用程序中输入 DNS 名称时，DNS 服务可以将该名称解析为与此名称关联的其他信息，如 IP 地址。  
  
下面是有关 DNS 客户端和 DNS 服务器的信息。  
  
### <a name="bkmk_dnsc"></a>DNS 客户端  
下面是新的或改进的 DNS 客户端技术。  
  
-   **DNS 客户端服务绑定**。 在 Windows 10 中，DNS 客户端服务为具有多个网络接口的计算机提供了增强的支持。  
  
有关详细信息，请参阅[Windows Server 2016 中 DNS 客户端的新增功能](dns/What-s-New-in-DNS-Client.md)  
  
### <a name="bkmk_dnss"></a>DNS 服务器  
下面是新的或改进的 DNS 服务器技术。  
  
-   **DNS 策略**。  你可以配置 DNS 策略来指定 DNS 服务器如何响应 DNS 查询。 DNS 响应可以基于客户端 IP 地址（位置）、当天的时间和多个其他参数。 DNS 策略启用位置感知的 DNS、流量管理、负载平衡、裂脑 DNS 和其他方案。  
  
-   **Nano server 支持基于文件的 dns**，你可以在 Windows Server 2016 中的 nano server 映像上部署 DNS 服务器。 如果你使用的是基于文件的 DNS，则可以使用此部署选项。 通过在 Nano Server 映像上运行 DNS 服务器，你可以在占用空间减少、快速启动和最小化修补的情况下运行 DNS 服务器。  
  
    > [!NOTE]   
    > Nano Server 上不支持 Active Directory 集成 DNS。  
  
-   **响应速率限制（RRL）** 。  可以在 DNS 服务器上启用响应速率限制。 这样做可以避免恶意系统在 DNS 客户端上使用 DNS 服务器启动拒绝服务攻击的可能性。  
  
-   **命名实体的基于 DNS 的身份验证（窗格会）** 。   您可以使用 TLSA （传输层安全身份验证）记录向 DNS 客户端提供信息，这些客户端会从您的域名中获得证书所需的证书颁发机构（CA）。 这可以防止中间人攻击，有人可能会损坏 DNS 缓存以指向自己的网站，并提供从其他 CA 颁发的证书。  
  
-   **未知记录支持**。   
     你可以使用未知的记录功能添加 Windows DNS 服务器未显式支持的记录。  
  
-   **IPv6 根提示**。   
     可以使用本地 IPV6 根提示支持通过 IPV6 根服务器执行 internet 名称解析。  
  
-   **改进的 Windows PowerShell 支持**。   
      新的 Windows PowerShell cmdlet 可用于 DNS 服务器。  
  
有关详细信息，请参阅[Windows server 2016 中 DNS 服务器的新增功能](dns/What-s-New-in-DNS-Server.md)  
  
## <a name="bkmk_GRE"></a>GRE 隧道  
RAS 网关现在支持站点到站点连接的高可用性通用路由封装（GRE）隧道和网关的 M + N 冗余。 GRE 是一种轻型隧道协议，可以在 Internet 协议网间上的虚拟点对点链路内封装各种网络层协议。  
  
有关详细信息，请参阅[Windows Server 2016 中的 GRE 隧道](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
## <a name="HNV"></a>Hyper-v 网络虚拟化  
Hyper-v 网络虚拟化（HNV）是在 Windows Server 2012 中引入的，它支持基于共享物理网络基础结构的客户网络虚拟化。 由于物理网络结构上只需进行少量的更改，HNV 为服务提供商提供灵活的灵活性，可在三个云中的任何位置部署和迁移租户工作负荷：服务提供商云、私有云或 Microsoft Azure 公有云。  
  
有关详细信息，请参阅[Windows Server 2016 中 Hyper-v 网络虚拟化的新增功能](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)  
  
## <a name="bkmk_ipam"></a>IPAM  
IPAM 为组织网络上的 IP 地址和 DNS 基础结构提供可高度自定义的管理和监视功能。 使用 IPAM，你可以监视、审核和管理运行动态主机配置协议（DHCP）和域名系统（DNS）的服务器。  
  
-   **增强了 IP 地址管理**。  
     在处理 IPv4/32 和 IPv6/128 子网以及查找 IP 地址块中的可用 IP 地址子网和范围时，会改进 IPAM 功能。  
  
-   **增强的 DNS 服务管理**。  
     IPAM 支持 DNS 资源记录、条件转发器和 DNS 区域管理，适用于已加入域的 Active Directory 集成和支持文件的 DNS 服务器。  
  
-   **集成的 DNS、DHCP 和 IP 地址（DDI）管理**。  
     启用了几种新体验和集成生命周期管理操作，例如，可视化所有与 IP 地址相关的 DNS 资源记录，基于 DNS 资源记录自动列出 IP 地址，以及 IP 地址生命周期管理用于 DNS 和 DHCP 操作。  
  
-   **多 Active Directory 林支持**。  
     当安装 IPAM 的林与每个远程林之间存在双向信任关系时，可以使用 IPAM 管理多个 Active Directory 林的 DNS 服务器和 DHCP 服务器。  
  
-   **支持基于角色的访问控制的 Windows PowerShell**。  
     可以使用 Windows PowerShell 设置 IPAM 对象上的访问作用域。  
  
有关详细信息，请参阅[ipam 中的新增功能](technologies/ipam/What-s-New-in-IPAM.md)和[管理 ipam](technologies/ipam/Manage-IPAM.md)。  
  

