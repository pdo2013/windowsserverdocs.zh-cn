---
title: 网络的新增功能
description: 本主题提供有关新功能和技术的 Windows Server 2016 中的网络的概述信息
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: get-started-article
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 43ce6290f6559be7cb078032b79519d1681506d4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829188"
---
# <a name="whats-new-in-networking"></a>网络的新增功能

>适用于：Windows Server 2016

以下是 Windows Server 2016 中新增或增强的网络技术。  
  Upd 本主题包含以下各节。  
  
-   [新的联网功能和技术](#bkmk_features)  
  
-   [其他网络技术的新功能](#bkmk_existing)  
  
## <a name="bkmk_features"></a>新的联网功能和技术

网络是软件定义数据中心 (SDDC) 平台的基础部分和 Windows Server 2016 提供了新的和改进软件定义网络 (SDN) 技术来帮助你为你的组织将移动到完全实现的 SDDC 解决方案。  
  
当您管理网络作为软件定义资源时，您可以描述应用程序的基础结构要求一次，然后选择应用程序运行所在的本地或云中。  这种一致性意味着，您的应用程序现在更容易进行缩放，并且可以无缝地运行应用程序，任何位置，且其安全性、 性能、 服务和可用性的质量。  
  
以下部分包含有关这些信息的新增网络功能和技术。  
  
### <a name="software-defined-networking-infrastructure"></a>软件定义的网络基础结构

以下是新的或改进 SDN 基础结构技术。  
  
-   **网络控制器**。 新 Windows Server 2016 中，在网络控制器提供集中的可编程点，可用于管理、 配置、 监视和故障排除你的数据中心中的虚拟和物理网络基础结构自动。 使用网络控制器可以自动配置网络基础结构，而无需手动执行网络设备和服务的配置。 有关详细信息，请参阅[网络控制器](sdn/technologies/network-controller/Network-Controller.md)并[部署软件定义的网络使用脚本](https://technet.microsoft.com/library/mt427380.aspx)。  
  
-   **HYPER-V 虚拟交换机**。 HYPER-V 虚拟交换机的 HYPER-V 主机上运行，并允许您创建分布式切换和路由、 策略强制层这也对齐且与 Microsoft Azure 兼容。 有关详细信息，请参阅 [Hyper-V 虚拟交换机](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。  
  
-   **网络功能虚拟化 (NFV)**。 在当今的软件中定义的数据中心，由硬件设备 （如负载均衡器、 防火墙、 路由器、 交换机等） 正在执行的网络功能越来越多地被部署为虚拟设备。 服务器虚拟化和网络虚拟化自然而然地形成了这种“网络功能虚拟化”。 虚拟设备快速按新出现的和创建新的市场。 它们继续生成感兴趣和获取这两个虚拟化平台中的动量和云服务。 Windows Server 2016 中提供了以下 NFV 技术。  
  
    -   **数据中心防火墙**。 此分布式的防火墙提供了精细的访问控制列表 (Acl)，您可以将防火墙策略级别的 VM 的接口或子网级别应用。  
  
        有关详细信息，请参阅[数据中心防火墙概述](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。  
  
    -   **RAS 网关**。 RAS 网关可用于虚拟网络与物理网络，包括从云数据中心的站点到站点 VPN 连接到租户的远程站点之间路由流量。 具体而言，可以部署 Internet 密钥交换版本 2 (IKEv2) 站点到站点虚拟专用网络 (Vpn)，第 3 层 (L3) VPN 和通用路由封装 (GRE) 网关。 此外，现在支持网关池和 M + N 冗余的网关;和边界网关协议 (BGP) 路由反射器功能提供了适用于所有网关方案 （IKEv2 VPN、 GRE VPN 和 L3 VPN） 网络之间的动态路由。  
  
        有关详细信息，请参阅[What's New in RAS 网关](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)并[用于 SDN 的 RAS 网关](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)。  
          
    - **软件负载均衡器 (SLB) 和网络地址转换 (NAT)**。 北-南和东-西层 4 个负载均衡器和 NAT 支持直接服务器返回与返回的网络流量可以绕过负载均衡多路复用器，从而增强了吞吐量。  
       有关详细信息，请参阅[软件负载平衡&#40;SLB&#41;用于 SDN](sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。  
  
    有关详细信息，请参阅[网络功能虚拟化](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)。  
  
-   **标准化协议**。 网络控制器使用 JavaScript 对象表示法 (JSON) 的有效负载其 northbound 接口上使用具象状态传输 (REST)。 网络控制器 southbound 接口使用打开 vSwitch 数据库管理协议 (OVSDB)。  
  
-   **灵活的封装技术**。 这些技术在数据平面操作和支持虚拟可扩展 LAN (VxLAN) 和网络虚拟化通用路由封装 (NVGRE)。 有关详细信息，请参阅[Windows Server 2016 中的 GRE 隧道](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
SDN 的详细信息，请参阅[软件定义的网络&#40;SDN&#41;](sdn/software-defined-networking.md)。  
  
### <a name="cloud-scale-fundamentals"></a>云规模基础知识
 
以下云规模基础知识现已推出。  
  
-   **聚合网络接口卡 (NIC)**。 聚合的 NIC，可使用单个网络适配器进行管理，启用远程直接内存访问 RDMA 的存储和租户通信。 这可以减少与数据中心，每个服务器相关联的资本支出，因为需要更少的网络适配器，以管理不同类型的每个服务器的流量。  
  
-   **Packet Direct**。  数据包直接提供了高网络流量吞吐量和低延迟数据包处理基础结构。  
  
-   **交换机嵌入式组合 (SET)**。        集是在 HYPER-V 虚拟交换机中集成的 NIC 组合解决方案。 设置允许的最多八个物理 NIC 组合到单个集团队，这提高了可用性，并提供故障转移。 在 Windows Server 2016 中，可以创建仅限于使用服务器消息块 (SMB) 和 RDMA 的集团队。 此外，可以使用组团队为 HYPER-V 网络虚拟化分配网络流量。 有关详细信息，请参阅[远程直接内存访问&#40;RDMA&#41;和交换机嵌入式组合&#40;设置&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。  
  
## <a name="bkmk_existing"></a>其他网络技术的新功能

本部分包含有关熟悉网络技术的新功能的信息。
  
## <a name="bkmk_dhcp"></a>DHCP  
DHCP 是一项 Internet 工程任务组 (IETF) 标准，旨在减轻在基于 TCP/IP 的网络（如私人内部网）上配置主机的管理负担并降低复杂度。 使用 DHCP 服务器服务，在 DHCP 客户端上配置 TCP/IP 的过程是自动进行的。  
  
有关详细信息，请参阅[What's New in DHCP](technologies/dhcp/What-s-New-in-DHCP.md)。  
  
## <a name="bkmk_dns"></a>DNS  
DNS 是 TCP/IP 网络中用于命名计算机和网络服务的系统。 DNS 命名通过用户友好的名称定位计算机和服务。 当用户在应用程序中输入 DNS 名称时，DNS 服务可以将该名称解析为与此名称关联的其他信息，如 IP 地址。  
  
下面是有关 DNS 客户端和 DNS 服务器的信息。  
  
### <a name="bkmk_dnsc"></a>DNS 客户端  
以下是新的或改进了 DNS 客户端技术。  
  
-   **DNS 客户端服务绑定**。 在 Windows 10 中，DNS 客户端服务提供的增强的支持具有多个网络接口的计算机。  
  
有关详细信息，请参阅[What's New in Windows Server 2016 中 DNS 客户端](dns/What-s-New-in-DNS-Client.md)  
  
### <a name="bkmk_dnss"></a>DNS 服务器  
以下是新的或改进了 DNS 服务器技术。  
  
-   **DNS 策略**。  可以配置 DNS 策略来指定 DNS 服务器响应 DNS 查询的方式。 DNS 响应可以在客户端 IP 地址 （位置） 上基于时间的天和几个其他参数。 DNS 策略启用位置感知型 DNS、 流量管理、 负载平衡、 拆分式 DNS 和其他方案。  
  
-   **对文件的 Nano Server 支持基于 DNS**，可以部署在 Windows Server 2016 Nano Server 映像上的 DNS 服务器。 此部署选项可供你如果使用的基于文件的 DNS。 通过在 Nano Server 映像上的 DNS 服务器正在运行，可以使用减少占用空间、 快速启动，和最小化修补运行你的 DNS 服务器。  
  
    > [!NOTE]   
    > Active Directory 集成的 DNS 不支持 Nano Server 上。  
  
-   **响应速率限制 (RRL)**。  可以让你的 DNS 服务器上的响应速率限制。 通过执行此操作，可以避免恶意使用你的 DNS 服务器来启动拒绝服务攻击，DNS 客户端上的系统中的可能性。  
  
-   **基于 DNS 的身份验证的命名实体 （窗格会）**。   可以使用 TLSA （传输层安全身份验证） 记录向哪些证书颁发机构 (CA)，它们应期望从你的域名的证书状态的 DNS 客户端提供的信息。 这样可以防止拦截的攻击，有人可能会损坏 DNS 缓存，以指向其自己的网站，并提供它们从不同的 CA 颁发的证书。  
  
-   **未知的记录支持**。   
     您可以添加记录不显式支持的使用未知的记录功能的 Windows DNS 服务器。  
  
-   **IPv6 根提示**。   
     可以使用根提示支持执行使用 IPV6 根服务器的 internet 名称解析的本机 IPV6。  
  
-   **改进了 Windows PowerShell 支持**。   
      为 DNS 服务器提供了新的 Windows PowerShell cmdlet。  
  
有关详细信息，请参阅[What's New in Windows Server 2016 中 DNS 服务器](dns/What-s-New-in-DNS-Server.md)  
  
## <a name="bkmk_GRE"></a>GRE 隧道  
RAS 网关现在支持站点到站点连接和网关的 M + N 冗余的高可用性通用路由封装 (GRE) 隧道。 GRE 是一种轻型隧道协议，可以在 Internet 协议网间上的虚拟点对点链路内封装各种网络层协议。  
  
有关详细信息，请参阅[Windows Server 2016 中的 GRE 隧道](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
## <a name="HNV"></a>HYPER-V 网络虚拟化  
在 Windows Server 2012 中引入的 HYPER-V 网络虚拟化 (HNV) 可以实现在共享物理网络基础结构之上的客户网络的虚拟化。 需在物理网络 fabric 上的最小更改，HNV 使服务提供商可以部署和任何位置跨三个云迁移的租户工作负荷的灵活性： 服务提供商云、 私有云或 Microsoft Azure 公有云。  
  
有关详细信息，请参阅[What's New in Windows Server 2016 中的 HYPER-V 网络虚拟化](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)  
  
## <a name="bkmk_ipam"></a>IPAM  
IPAM 提供组织网络上的 IP 地址和 DNS 基础结构的高度可自定义管理和监视能力。 使用 IPAM，可以监视、 审核，并管理运行动态主机配置协议 (DHCP) 和域名系统 (DNS) 服务器。  
  
-   **增强的 IP 地址管理**。  
     IPAM 功能被改进的方案，例如处理/32 IPv4 和 IPv6 为/128 子网和 IP 地址块中查找可用的 IP 地址子网和范围。  
  
-   **增强的 DNS 服务管理**。  
     IPAM 支持 DNS 资源记录、 条件转发器和 DNS 区域管理这两个已加入域的 Active Directory 集成和支持文件的 DNS 服务器。  
  
-   **集成的 DNS、 DHCP 和 IP 地址 (DDI) 管理**。  
     多个新体验和已启用集成的生命周期管理操作，例如可视化所有 DNS 资源记录适用于 IP 地址，基于 DNS 资源记录和 IP 地址生命周期管理的 IP 地址的自动化的清单有关 DNS 和 DHCP 的操作。  
  
-   **多个 Active Directory 林支持**。  
     IPAM 可用于管理多个 Active Directory 林的 DNS 和 DHCP 服务器时安装 IPAM 的林和每个远程林之间具有双向信任关系。  
  
-   **基于角色的访问控制的 Windows PowerShell 支持**。  
     可以使用 Windows PowerShell IPAM 对象上设置访问作用域。  
  
有关详细信息，请参阅[What's New in IPAM](technologies/ipam/What-s-New-in-IPAM.md)并[管理 IPAM](technologies/ipam/Manage-IPAM.md)。  
  

