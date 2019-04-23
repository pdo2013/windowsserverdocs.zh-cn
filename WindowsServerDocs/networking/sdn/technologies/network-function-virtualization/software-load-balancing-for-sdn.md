---
title: 用于 SDN 的软件负载平衡 (SLB)
description: 可以使用本主题以了解软件负载平衡适用于 Windows Server 2016 中软件定义的网络。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 97abf182-4725-4026-801c-122db96964ed
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 26fb4aa21e80618c4c63bd9edbf8731bf886db62
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853758"
---
# <a name="software-load-balancing-slb-for-sdn"></a>软件负载平衡\(SLB\)用于 SDN

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题以了解软件负载平衡适用于 Windows Server 2016 中软件定义的网络。  

云服务提供商 (Csp) 和部署软件定义网络 (SDN) Windows Server 2016 中的企业可以使用软件负载平衡 (SLB) 均匀分配租户和租户客户在虚拟网络资源之间的网络流量。 Windows Server SLB 允许多台服务器承载相同的工作负荷，具有较高的可用性和可扩展性。
  
Windows Server SLB 包括以下功能。  
  
-   第 4 (L4) 层负载均衡服务针对北-南和东-西 TCP/UDP 流量。  
  
-   公共和内部网络流量负载平衡。  
  
-   虚拟局域网 (Vlan) 和使用 HYPER-V 网络虚拟化创建的虚拟网络上支持动态 IP 地址 (Dip)。  
  
-   运行状况探测支持。  
  
-   云的规模，其中包括横向扩展功能，可供和纵向扩展为多路复用器和主机代理功能。  
  
有关详细信息，请参阅[软件负载平衡功能](#bkmk_features)本主题中。  
  
> [!NOTE]  
> 由网络控制器不支持 Vlan 为多租户，但是您可以使用 Vlan 与 SLB 服务提供程序管理工作负荷，例如数据中心基础结构和高密度 Web 服务器。  
  
使用 Windows Server SLB，你可以横向扩展您的负载平衡功能的其他 VM 工作负荷使用的相同的 HYPER-V 计算服务器上使用 SLB Vm。 正因为如此，SLB 支持快速创建和删除执行 CSP 操作所需的负载均衡终结点。 此外，Windows Server SLB 支持每个群集数十亿字节，提供了简单的预配模型，并轻松地扩大和缩小。  
  
**SLB 的工作原理**  
  
SLB 虚拟 IP 地址 (Vip) 映射到动态 IP 地址 (Dip) 的云服务的数据中心中的资源集的一部分工作。  
  
Vip 是负载的提供公共访问权限，对池均衡的 Vm 的单个 IP 地址。 例如，Vip 是在 Internet 公开，以便租户和租户客户可以连接到云数据中心中的租户资源的 IP 地址。  
  
Dip 是成员后面 VIP 负载均衡池中的 Vm 的 IP 地址。 Dip 被分配到的租户资源云基础结构中。  
  
Vip 位于在 SLB 多路复用器 (MUX)。  MUX 包含一个或多个虚拟机 (Vm)。  网络控制器提供了与每个 VIP，每个 MUX 和每个 MUX 反过来使用边界网关协议 (BGP) 播发到物理网络上路由器 / 32 作为每个 VIP 路由。  BGP 允许到物理网络路由器：  
  
-   了解 VIP 是在每个 MUX 上可用，即使多路复用器位于第 3 层网络中的不同子网。  
  
-   针对每个 VIP 的负载分布在所有可用多路复用器使用相等成本多路径 (ECMP) 路由。  
  
-   自动检测 MUX 故障或移除并停止将流量发送到失败 MUX。  
  
-   从失败的或已删除 MUX 负载分布到正常运行的多路复用器。  
  
当公共流量从 Internet 到达后时，SLB MUX 检查流量，后者包含作为目标，VIP 和映射，以便将到达单个 DIP，流量将重写。 对于入站网络流量，此事务执行中 MUX 虚拟机 (Vm) 和目标 DIP 所在的 HYPER-V 主机之间拆分一个两步过程：  
  
-   负载平衡-MUX 使用 VIP 以选择某个 DIP，封装数据包，并将流量转发到 DIP 所在的 HYPER-V 主机。  
  
-   网络地址转换 (NAT) 的 HYPER-V 主机从数据包移除封装，将转换到 DIP VIP、 将重新映射端口，并将数据包转发到 DIP 的 VM。  
  
MUX 知道如何将 Vip 映射到正确的 Dip，由于负载平衡使用网络控制器定义的策略。 这些规则包括协议、 前端端口、 后端端口和分发算法 （5、 3 或 2 元组）。  
  
当租户 Vm 响应并发送出站网络流量回 Internet 或远程租户位置，因为由 HYPER-V 主机，流量执行 NAT 绕过 MUX 并直接进入边缘路由器从 HYPER-V 主机。 此 MUX 跳过过程称为直接服务器返回 (DSR)。  
  
和建立初始的网络流量流后，入站的网络流量会绕过 SLB MUX 完全。  
  
在下图中，客户端计算机公司 SharePoint 站点-在这种情况下，名为 Contoso 的虚构公司的 IP 地址执行 DNS 查询。 发生以下过程。  
  
-   DNS 服务器返回到客户端的 VIP 107.105.47.60。  
  
-   客户端将 HTTP 请求发送到的 VIP。  
  
-   物理网络具有多个路径可用来访问位于任何 MUX 的 VIP。  在此过程的每个路由器使用 ECMP 直至请求到达 MUX 选取路径的下一个段。  
  
-   接收请求 MUX 检查配置的策略，并看到有两个 Dip 可用，10.10.10.5 和 10.10.20.5，来处理对 VIP 107.105.47.60 请求的虚拟网络上  
  
-   MUX 选择 DIP 10.10.10.5 并封装使用 VXLAN，因此它可以将其发送到包含托管使用 DIP 的主机物理网络地址的数据包。  
  
-   主机接收封装的数据包，并对其进行检查。  它会删除封装，且重写该数据包，因此目标现 DIP 10.10.10.5 而不是 VIP 并将流量发送到 DIP 的 VM。  
  
-   请求现在已达到服务器场 2 中的 Contoso SharePoint 站点。 在服务器生成一个响应，并将其发送到客户端，使用它自己的 IP 地址作为源。  
  
-   主机截获客户端，现在的目标，到 VIP 发出原始请求的传出数据包中的已保存的虚拟交换机。  在主机重写为 VIP，因此，向客户端看不到 DIP 地址的数据包的源。  
  
-   主机将直接向使用其标准的路由表将转发到客户端的最终接收的响应数据包的物理网络的默认网关数据包转发。  
  
![软件负载平衡过程](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**负载均衡的内部数据中心流量**  
  
何时负载均衡在数据中心内部的网络流量，如之间不同的服务器上运行且属于同一虚拟网络的租户资源，Vm 连接到 HYPER-V 虚拟交换机执行 nat。  
  
与内部流量负载平衡，第一个请求将发送到并处理的 MUX，其选择相应的 DIP，并将流量路由到 DIP。 从此时起，已建立的流量绕过 MUX 并直接从虚拟机转到 VM。  
  
**运行状况探测**  
  
SLB 包括运行状况探测，若要验证的网络基础结构，包括以下运行状况。  
  
-   为端口 TCP 探测  
  
-   HTTP 探测端口和 URL  
  
探测在设备上开始并通过网络传输到 DIP 的其中一个传统的负载均衡器设备，与 SLB 探测源自其中 DIP 是所在，直接从 SLB 主机代理转到 DIP，进一步分发在主机上对主机都有效。  
  
## <a name="bkmk_infrastructure"></a>软件负载平衡基础结构  
若要部署 Windows Server SLB，必须先部署 Windows Server 2016 中的网络控制器和一个或多个 SLB MUX Vm。  
  
此外，必须使用 SDN 启用的 HYPER-V 虚拟交换机中配置的 HYPER-V 主机，并确保 SLB 主机代理正在运行。  服务于主机的路由器必须支持相等成本多路径 (ECMP) 路由和边界网关协议 (BGP)，并且必须配置为接受来自 SLB Mux 的 BGP 对等互连请求。  
  
下面是 SLB 基础结构的概述。  

![软件负载平衡基础结构](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
以下部分提供有关这些元素 SLB 基础结构的详细信息。  
  
### <a name="scvmm"></a>SCVMM  
使用 System Center 2016，可以在 Windows Server 2016，包括 SLB 管理器和运行状况监视器上配置网络控制器。 部署 SLB MUXs 以及运行 Windows Server 2016 和 HYPER-V 的计算机上安装 SLB 主机代理，还可以使用 System Center。  
  
有关 System Center 2016 的详细信息，请参阅[System Center 2016](https://www.microsoft.com/server-cloud/products/system-center-2016/)。  
  
> [!NOTE]  
> 如果您不想要使用 System Center 2016，可以使用 Windows PowerShell 或另一个管理应用程序来安装和配置网络控制器和其他 SLB 基础结构。 有关详细信息，请参阅[使用 Windows PowerShell 部署网络控制器](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)。  
  
### <a name="network-controller"></a>网络控制器  
网络控制器托管 SLB 管理器，并针对 SLB 执行以下操作。  
  
-   处理通过 Northbound API 从 System Center、 Windows PowerShell 或其他网络管理应用程序显示的 SLB 命令。  
  
-   计算以分发给 HYPER-V 主机和 SLB Mux 的策略。  
  
-   提供 SLB 基础结构的运行状况状态。  
  
### <a name="slb-mux"></a>SLB MUX  
SLB MUX 处理入站的网络流量并将 Vip 映射到 Dip，然后将流量转发到正确的 DIP。 每个 MUX 还使用 BGP 来发布 VIP 路由到边缘路由器。 BGP 使保持活动状态通知 MUX 失败，它允许活动多路复用器来重新分配在发生 MUX-实质上为负载均衡器提供负载平衡负载时的多路复用器。  
  
### <a name="hosts-that-are-running-hyper-v"></a>运行 HYPER-V 的主机。  
SLB 可使用运行 Windows Server 2016 和 HYPER-V 的计算机。 HYPER-V 主机上的 Vm 可以运行的 HYPER-V 支持任何操作系统。  
  
### <a name="slb-host-agent"></a>SLB 主机代理  
当您部署 SLB 时，必须使用 System Center、 Windows PowerShell 或另一个管理应用程序部署的 HYPER-V 主机的每台计算机上的 SLB 主机代理。 您可以提供的 HYPER-V 支持，包括 Nano Server 的所有版本的 Windows Server 2016 上安装 SLB 主机代理。  
  
SLB 主机代理侦听的网络控制器中的 SLB 策略更新。 此外，主机代理程序规则的 SLB 到 SDN 启用的 HYPER-V 虚拟交换机配置本地计算机上。  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>SDN 启用的 HYPER-V 虚拟交换机  
虚拟交换机与 SLB，兼容必须使用的 HYPER-V 虚拟交换机管理器或 Windows PowerShell 命令以创建的交换机，然后您必须允许虚拟筛选平台 (VFP) 虚拟交换机。  
  
虚拟交换机上启用 VFP 的信息，请参阅 Windows PowerShell 命令[Get-vmsystemswitchextension](https://technet.microsoft.com/library/hh848603.aspx)并[Enable-vmswitchextension](https://technet.microsoft.com/library/hh848541.aspx?f=255&MSPPError=-2147217396)。  
  
SDN 启用的 HYPER-V 虚拟交换机的 SLB 将执行以下操作。  
  
-   对于 SLB 处理的数据路径。  
  
-   接收来自 MUX 的入站的网络流量。  
  
-   绕过出站网络流量，将其发送到路由器使用 DSR MUX。  
  
-   运行 HYPER-V 的 Nano Server 实例上。  
  
### <a name="bgp-enabled-router"></a>启用 BGP 的路由器  
BGP 路由器的 SLB 将执行以下操作。  
  
-   路由使用 ECMP MUX 的入站的流量。  
  
-   对于出站网络流量，使用由主机提供的路由。  
  
-   路由更新侦听从 SLB MUX 的 Vip。  
  
-   如果使保持活动状态失败，请从 SLB 旋转中删除 SLB 多路复用器。  
  
## <a name="bkmk_features"></a>软件负载平衡功能  
以下是一些功能和 SLB 的功能。  
  
**核心功能**  
  
-   SLB 提供第 4 层负载平衡北-南和东-西 TCP/UDP 通信的服务  
  
-   可以在基于 HYPER-V 网络虚拟化的网络上使用 SLB  
  
-   DIP 连接到 SDN 启用的 HYPER-V 虚拟交换机的 vm，可以使用基于 VLAN 的网络使用 SLB。  
  
-   一个 SLB 实例可以处理多个租户  
  
-   SLB 和 DIP 支持可缩放且低延迟返回路径，实现通过直接服务器返回 (DSR)  
  
-   SLB 函数还使用交换机嵌入式组合 (SET) 或单根输入/输出虚拟化 (SR-IOV) 时  
  
-   SLB 包括 Internet 协议版本 4 (IPv4) 支持  
  
-   对于站点到站点网关方案 SLB 提供了 NAT 功能以便利用单个公共 IP 的所有站点到站点连接  
  
-   你可以安装 SLB，包括主机代理和 MUX，Windows Server 2016 上的完全、 Core 和 Nano 安装。  
  
**规模和性能**  
  
-   云的规模，其中包括横向扩展功能，可供和纵向扩展为多路复用器和主机代理功能。  
  
-   一个活动 SLB 管理器网络控制器模块可支持 8 个 MUX 实例  
  
**高可用性**  
  
-   可以将 SLB 部署到采用主动/主动配置中的 2 个以上节点  
  
-   可以添加多路复用器，并将其从 MUX 池中删除而不会影响 SLB 服务。 这可以保持 SLB 可用性时   
    正在修补单个多路复用器。  
  
-   各 MUX 实例将具有 99%的运行时间  
  
-   运行状况监视数据是可用于管理实体  
  
**对齐方式**  
  
-   你可以部署和配置 SLB 与 SCVMM  
  
-   SLB 提供多租户的统一的 edge，通过与 Microsoft 设备，例如 RAS 多租户网关、 数据中心防火墙和路由反射器无缝集成。  
  

