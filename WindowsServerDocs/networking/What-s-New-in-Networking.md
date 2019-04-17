---
title: 在网络中的新增功能
description: 本主题提供概述的新功能和信息技术 Windows Server 2016 中的网络
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: get-started-article
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 85b1a573e8ac724a2a9e22863f890db668423cad
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-networking"></a>在网络中的新增功能

>适用于：Windows Server 2016

以下是 Windows Server 2016 中的一些新的或改进的网络技术。  
  
本主题包含以下部分。  
  
-   [新的网络功能和技术](#bkmk_features)  
  
-   [其他网络技术的新功能](#bkmk_existing)  
  
## <a name="bkmk_features"></a>新的网络功能和技术

网络都是软件定义 Datacenter (SDDC) 平台基础部分和 Windows Server 2016 提供了新的和改进软件定义网络 (SDN) 技术来帮助你针对你的组织移动到实际完全 SDDC 解决方案。  
  
当你为软件定义资源管理网络时，你可以一次，描述应用程序的基础结构要求，然后选择该应用程序运行了-在本地或在云中。  此一致性意味着你应用现在能够更轻松地缩放，并且你可以无缝运行周围安全、性能和质量的服务和可用性相等确信的应用程序，随时随地。  
  
以下部分包含有关这些新功能和技术的网络。  
  
### <a name="software-defined-networking-infrastructure"></a>软件定义的网络的基础结构。

下面是最新的或改进的 SDN 基础结构技术。  
  
-   **网络控制器**。 新的 Windows Server 2016，网络控制器提供的集中、可编程点自动化管理、配置，监视器和虚拟疑难解答和您的数据中心中的物理网络基础结构。 使用网络控制器，你可以自动网络基础结构，而不是执行手动配置的网络的设备和服务的配置。 有关详细信息，请参阅[网络控制器](sdn/technologies/network-controller/Network-Controller.md)和[部署软件定义的网络使用脚本](https://technet.microsoft.com/library/mt427380.aspx)。  
  
-   **Hyper-V 虚拟交换机用来**。 Hyper-V 虚拟交换机用来在 Hyper-V 主机上运行，并允许您创建分布式切换并路由和对齐和兼容性的 Microsoft Azure 策略强制层。 有关详细信息，请参阅[Hyper-V 虚拟交换机用来](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。  
  
-   **网络虚拟化函数 (NFV)**。 在当今软件定义的数据中心，由硬件设备（如负载平衡防火墙、路由器，开关，以及等）的网络功能越来越正在部署为虚拟装置。 此"网络函数虚拟化"是的自然而然服务器虚拟化和网络虚拟化。 虚拟装置快速等新兴，并创建一个新市场中。 它们不断生成兴趣获得动力在这两个虚拟化平台和云服务。 以下 NFV 技术都可在 Windows Server 2016。  
  
    -   **Datacenter 防火墙**。 此分布式的防火墙提供具体访问 acl)，使你可以将防火墙策略，在 VM 接口级别或子网级别的应用。  
  
        有关详细信息，请参阅[Datacenter 防火墙概述](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。  
  
    -   **RAS 网关**。 你可以使用 RAS 网关路由虚拟网络和物理网络，其中包括云数据中心中的站点的 VPN 连接到远程租户的站点之间的交通。 特别是，你可以将 Internet 键 Exchange 版本 2 (IKEv2) 部署站点的虚拟专用网络 (Vpn)，层 3 平板电脑 (L3) VPN 和通用路由封装 (GRE) 网关。 此外，现在支持网关池和 M + N 冗余网关;并带有路线反射功能的边框网关协议 (BGP) 提供动态路由之间所有网关方案（IKEv2 VPN、GRE VPN 和 L3 VPN）的网络。  
  
        有关详细信息，请参阅[RAS 网关中的新增功能是什么](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)和[SDN RAS 网关](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)。  
          
    - **软件负载平衡 (SLB) 和网络地址翻译 (NAT)**。 北美南和东西分层显示 4 负载平衡和 NAT 通过支持直接服务器返回与其退货网络交通可以跳过负载平衡集线器增强吞吐量。  
       有关详细信息，请参阅[软件负载平衡 & #40;SLB & #41;对于 SDN](sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。  
  
    有关详细信息，请参阅[网络函数虚拟化](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)。  
  
-   **实现标准化协议**。 网络控制器上与 JavaScript 对象表示法 (JSON) 负载其 northbound 界面中使用具象传输状态 (REST)。 网络控制器 southbound 界面使用打开 vSwitch 数据库管理协议 (OVSDB)。  
  
-   **灵活封装技术**。 这些技术使用在数据平面，，并且支持虚拟可扩展 LAN (VxLAN) 和网络虚拟化泛型路由封装 (NVGRE)。 有关详细信息，请参阅[GRE 隧道在 Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
有关 SDN 的详细信息，请参阅[软件定义网络 & #40;SDN & #41;](sdn/software-defined-networking.md).  
  
### <a name="cloud-scale-fundamentals"></a>云缩放的基础知识
 
以下云比例基础现已可用。  
  
-   **集中网络接口卡 (NIC)**。 已合并好的 NIC 允许你的网络适配器可用于管理、 远程直接内存访问 RDMA 启用存储，以及租户交通。 这减少与您的数据中心中的每个服务器资金支出，因为需要更少的网络适配器管理不同类型的每个服务器的交通。  
  
-   **数据包直接**。  数据包直接提供高网络交通吞吐量和低延迟数据包处理基础结构。  
  
-   **切换嵌入组队（集）**。        设置是已集成 HYPER-V 虚拟交换机用来在 NIC 组合解决方案。 设置允许达八物理 NIC 分组到一个集团队，从而提高可用性并提供故障转移。 在 Windows Server 2016，可以创建仅限于使用服务器消息块 (SMB) 和 RDMA 的组团队。 此外，可以使用组团队为 Hyper-V 网络虚拟化分配网络流量。 有关详细信息，请参阅[远程直接内存访问 & #40;RDMA & #41;切换 Embedded 组队 & #40; 以及设置和 #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
## <a name="bkmk_existing"></a>其他网络技术的新功能

此部分中包含有关网络熟悉的技术的新功能的信息。
  
## <a name="bkmk_dhcp"></a>DHCP  
DHCP 是旨在减少管理负担和基于 IP 网络，如专用 intranet 配置主机复杂性工程工作组 Internet (IETF) 标准。 通过使用 DHCP 服务器的服务的客户端 DHCP 上配置 TCP/IP 的过程会自动。  
  
有关详细信息，请参阅[DHCP 中的新增](technologies/dhcp/What-s-New-in-DHCP.md)。  
  
## <a name="bkmk_dns"></a>DNS  
DNS 是一个系统，用于在 TCP/IP 网络命名的计算机和网络的服务。 DNS 命名查找计算机和用户友好名称通过的服务。 当用户进入应用程序中的 DNS 名称时，DNS 服务可以解决关联的名称，如 IP 地址的其他信息的名称。  
  
以下是有关 DNS 客户端和 DNS 服务器的信息。  
  
### <a name="bkmk_dnsc"></a>DNS 客户端  
下面是最新的或改进的 DNS 客户端技术。  
  
-   **DNS 客户端服务绑定**。 在 Windows 10 中，在 DNS 客户服务提供增强具有多个网络接口计算机的支持。  
  
有关详细信息，请参阅[什么是 Windows Server 2016 中的 DNS 客户端中的新增功能](dns/What-s-New-in-DNS-Client.md)  
  
### <a name="bkmk_dnss"></a>DNS 服务器  
下面是最新的或改进的 DNS 服务器技术。  
  
-   **DNS 策略**。  您可以配置 DNS 策略，若要指定 DNS 服务器响应 DNS 查询的方式。 客户端 IP 地址（的位置），可以基于 DNS 响应时间天，以及一些其他参数。 DNS 策略启用位置感知 DNS、交通管理、负载平衡、裂 DNS 和其他情况。  
  
-   **文件服务器 Nano 支持基于 DNS**，你可以将部署 Windows Server 2016 中的 DNS 服务器 Nano 服务器映像。 如果你使用的，则会向你提供此部署选项基于 DNS 文件。 按正在运行 Nano 服务器图像的 DNS 服务器，您可以使用减少占用情况，快速启动，在最小化修补运行 DNS 服务器。  
  
    > [!NOTE]   
    > 集成的 DNS 不支持 Nano 服务器的 Active Directory。  
  
-   **响应评价限制 (RRL)**。  你可以启用响应 DNS 服务器上的速率限制。 通过执行此操作，你避免恶意通过以下 DNS 服务器启动拒绝 DNS 客户端上服务攻击的系统。  
  
-   **基于 DNS 命名实体 (DANE) 的验证**。   你可以使用 TLSA（传输层安全性身份验证）记录以信息提供给他们应哪些证书颁发机构）期望的证书从你的域名状态的 DNS 客户端。 这可以防止某人可能会在此处损坏 DNS 缓存指向其赢得网站，并提供他们颁发的不同 CA 证书拦截中攻击。  
  
-   **未知的记录支持**。   
     你可以添加明确不支持使用未知的记录功能的 Windows DNS 服务器的记录。  
  
-   **IPv6 根提示**。   
     你可以使用原始 IPV6 根提示支持执行 Internet 名称分辨率使用 IPV6 根服务器。  
  
-   **改进了 Windows PowerShell 支持**。   
      新的 Windows PowerShell cmdlet 是适用于 DNS 服务器。  
  
有关详细信息，请参阅[什么是 Windows Server 2016 中的 DNS 服务器中的新增功能](dns/What-s-New-in-DNS-Server.md)  
  
## <a name="bkmk_GRE"></a>GRE 隧道  
RAS 网关现在的站点之间的连接和 M + N 冗余网关的支持可用性通用路由封装 (GRE) 隧道。 GRE 是一个简洁的隧道协议，可通过 Internet 协议网络封装各种网络层内虚拟点到点链接的协议。  
  
有关详细信息，请参阅[GRE 隧道在 Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
## <a name="HNV"></a>Hyper-V 网络虚拟化  
引入 Windows Server 2012、Hyper-V 网络虚拟化 (HNV) 使虚拟化客户在顶部共享物理网络基础结构的网络。 用降至最低更改物理网络结构必要，HNV 提供服务提供商的灵活性部署和跨三个云的任何位置迁移租户工作负载：服务提供商云、专用云中进行构建或 Microsoft Azure 公共云。  
  
有关详细信息，请参阅[什么是中 Hyper-V 网络虚拟化 Windows Server 2016 的新增功能](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)  
  
## <a name="bkmk_ipam"></a>IPAM  
IPAM 组织网络上提供的 IP 地址和 DNS 基础结构高度自定义管理和监控的功能。 使用 IPAM，可以监视、 审核，并管理动态主机配置协议 (DHCP) 和域名系统 (DNS) 运行的服务器。  
  
-   **增强的 IP 地址管理**。  
     如处理 IPv4/32 和 IPv6 /128 子网和 IP 地址块中查找 IP 地址的免费网和范围方案 IPAM 功能得到改进。  
  
-   **增强 DNS 服务管理**。  
     IPAM 支持两个已加入域的 Active Directory 集成和文件备份 DNS 服务器的资源 DNS 记录、 条件转发器，并 DNS 区域管理。  
  
-   **集成 DNS、 DHCP 和 IP 地址 (DDI) 管理**。  
     启用操作，如直观显示所有 DNS 资源记录属于 IP 地址，几个新的体验和集成生命周期管理自动 IP 地址基于 DNS 资源的记录，以及对 DNS 和 DHCP 操作的 IP 地址生命周期管理的清单。  
  
-   **多个主动目录森林支持**。  
     IPAM 可用于装有 IPAM 森林和每个远程林双向信任关系时管理多个 Active Directory 林 DHCP 和 DNS 服务器。  
  
-   **Windows PowerShell 角色基于访问控制支持**。  
     你可以使用 Windows PowerShell 设置 IPAM 对象访问范围。  
  
有关详细信息，请参阅[IPAM 中的新增功能是什么](technologies/ipam/What-s-New-in-IPAM.md)和[管理 IPAM](technologies/ipam/Manage-IPAM.md)。  
  

