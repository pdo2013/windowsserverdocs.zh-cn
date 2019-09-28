---
title: 用于 SDN 的软件负载平衡 (SLB)
description: 你可以使用本主题来了解 Windows Server 2016 中软件定义的网络的软件负载平衡。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 97abf182-4725-4026-801c-122db96964ed
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 35743d9e1a25c71a35eed018a4a3882a3d094d76
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355574"
---
# <a name="software-load-balancing-slb-for-sdn"></a>用于 SDN 的软件负载平衡 \(SLB @ no__t-1

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题来了解 Windows Server 2016 中软件定义的网络的软件负载平衡。  

在 Windows Server 2016 中部署软件定义网络（SDN）的云服务提供商（Csp）和企业可以使用软件负载平衡（SLB）在虚拟网络资源之间均匀分配租户和租户客户网络流量。 Windows Server SLB 允许多台服务器承载相同的工作负荷，具有较高的可用性和可扩展性。
  
Windows Server SLB 包含以下功能。  
  
-   第4层（L4）用于 "北-南部" 和 "东-西" TCP/UDP 流量的负载均衡服务。  
  
-   公共和内部网络流量负载平衡。  
  
-   支持在虚拟局域网（Vlan）上以及使用 Hyper-v 网络虚拟化创建的虚拟网络上的动态 IP 地址（Dip）。  
  
-   运行状况探测支持。  
  
-   适用于云规模，包括横向扩展功能，以及 multiplexers (和主机代理的扩展功能。  
  
有关详细信息，请参阅本主题中的[软件负载平衡功能](#bkmk_features)。  
  
> [!NOTE]  
> 网络控制器不支持 Vlan 的多租户，但你可以将 Vlan 与 SLB 用于服务提供商管理的工作负荷，例如数据中心基础结构和高密度 Web 服务器。  
  
使用 Windows Server SLB，你可以在用于其他 VM 工作负荷的相同 Hyper-v 计算服务器上使用 SLB Vm 来横向扩展负载平衡功能。 因此，SLB 支持快速创建和删除 CSP 操作所需的负载均衡终结点。 此外，Windows Server SLB 支持每个群集有数十千兆字节，提供简单的预配模式，并且易于扩展和扩展。  
  
**SLB 的工作方式**  
  
SLB 的工作方式是将虚拟 IP 地址（Vip）映射到数据中心的一组云服务中的动态 IP 地址（Dip）。  
  
Vip 是单个 IP 地址，提供对负载平衡 Vm 池的公共访问。 例如，Vip 是在 Internet 上公开的 IP 地址，以便租户和租户客户可以连接到云数据中心内的租户资源。  
  
Dip 是 VIP 后面的负载均衡池的成员 Vm 的 IP 地址。 在云基础结构中将 Dip 分配给租户资源。  
  
Vip 位于 SLB 多路复用器（MUX）中。  MUX 由一个或多个虚拟机（Vm）组成。  网络控制器为每个 MUX 提供每个 VIP，每个 MUX 依次使用边界网关协议（BGP）将每个 VIP 播发到物理网络上的路由器，如/32 路由。  BGP 允许物理网络路由器：  
  
-   了解到每个 MUX 上都提供了 VIP，即使 Mux 位于第3层网络中不同的子网中。  
  
-   使用相等开销多路径（ECMP）路由跨所有可用 Mux 分布每个 VIP 的负载。  
  
-   自动检测 MUX 故障或删除，并停止将流量发送到失败的 MUX。  
  
-   跨正常 Mux 将负载从失败或删除的 MUX 分散。  
  
当公共流量从 Internet 接收时，SLB MUX 会检查流量，其中包含 VIP 作为目标，并映射和重写流量，使其到达单个 DIP。 对于入站网络流量，此事务在两个步骤中执行，该过程在 MUX 虚拟机（Vm）和目标 DIP 所在的 Hyper-v 主机之间进行拆分：  
  
-   负载平衡-MUX 使用 VIP 来选择 DIP，封装数据包，并将流量转发到 DIP 所在的 Hyper-v 主机。  
  
-   网络地址转换（NAT）-Hyper-v 主机会从数据包中删除封装，将 VIP 转换为 DIP，重新映射端口，然后将数据包转发到 DIP VM。  
  
MUX 知道如何将 Vip 映射到正确的 Dip，因为使用网络控制器定义的负载均衡策略。 这些规则包括协议、前端端口、后端端口和分发算法（5个、3个或2个元组）。  
  
当租户 Vm 响应并将出站网络流量发送回 Internet 或远程租户位置时，由于由 Hyper-v 主机执行 NAT，流量将绕开 MUX，并直接从 Hyper-v 主机转到边缘路由器。 此 MUX 绕过进程称为直接服务器返回（DSR）。  
  
在建立初始网络流量流后，入站网络流量会完全跳过 SLB MUX。  
  
在下图中，客户端计算机对公司 SharePoint 站点的 IP 地址（在本例中为 Contoso 的虚构公司）执行 DNS 查询。 将发生以下过程。  
  
-   DNS 服务器将 VIP 107.105.47.60 返回到客户端。  
  
-   客户端向 VIP 发送 HTTP 请求。  
  
-   物理网络有多个路径可用于连接到任何 MUX 上的 VIP。  在此过程中，每个路由器均使用 ECMP 选取路径的下一段，直到请求到达 MUX。  
  
-   用于接收请求的 MUX 检查配置的策略，并发现虚拟网络上有两个可用的 Dip，10.10.10.5 和10.10.20.5，用于处理 VIP 107.105.47.60 的请求  
  
-   MUX 选择 DIP 10.10.10.5，并使用 VXLAN 封装数据包，以便它可以使用主机物理网络地址将数据包发送到包含 DIP 的主机。  
  
-   主机接收封装的数据包并对其进行检查。  它删除了封装并重写数据包，以便目标现在是 DIP 10.10.10.5，而不是 VIP，并将流量发送到 DIP VM。  
  
-   请求现在已到达服务器场2中的 Contoso SharePoint 站点。 服务器生成响应，并使用其自己的 IP 地址作为源来将其发送到客户端。  
  
-   主机会截获虚拟交换机中的传出数据包，这会记住客户端（现在是目标）已将原始请求发送到 VIP。  主机将数据包的源重写为 VIP，以便客户端看不到 DIP 地址。  
  
-   主机会将数据包直接转发到物理网络的默认网关，该网络使用其标准路由表将数据包转发到最终接收响应的客户端。  
  
![软件负载平衡过程](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**对内部数据中心流量进行负载平衡**  
  
当对数据中心内部的网络流量进行负载平衡时（例如在不同服务器上运行的租户资源之间，并且是同一虚拟网络的成员）时，Vm 连接到的 Hyper-v 虚拟交换机会执行 NAT。  
  
使用内部流量负载平衡时，会将第一个请求发送到 MUX，并处理该请求，这将选择相应的 DIP 并将流量路由到 DIP。 然后，已建立的流量流会绕开 MUX，并直接从 VM 转到 VM。  
  
**运行状况探测**  
  
SLB 包括运行状况探测来验证网络基础结构的运行状况，包括以下各项。  
  
-   TCP 探测到端口  
  
-   HTTP 探测到端口和 URL  
  
不同于传统的负载均衡器设备，其中探测器出自设备，并通过线路传输到 DIP，SLB 探测是在 DIP 所在的主机上发出的，并直接从 SLB 主机代理转到 DIP，进一步分布跨主机工作。  
  
## <a name="bkmk_infrastructure"></a>软件负载平衡基础结构  
若要部署 Windows Server SLB，你必须首先在 Windows Server 2016 中部署网络控制器和一个或多个 SLB MUX Vm。  
  
此外，还必须将 Hyper-v 主机配置为启用了 SDN 的 Hyper-v 虚拟交换机，并确保 SLB 主机代理正在运行。  服务主机的路由器必须支持相等成本多路径（ECMP）路由和边界网关协议（BGP），并且必须配置为接受来自 SLB Mux 的 BGP 对等互连请求。  
  
下面是 SLB 基础结构的概述。  

![软件负载平衡基础结构](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
以下各节提供了有关 SLB 基础结构的这些元素的详细信息。  
  
### <a name="scvmm"></a>SCVMM  
借助 System Center 2016，你可以在 Windows Server 2016 上配置网络控制器，包括 SLB 管理器和运行状况监视器。 你还可以使用 System Center 部署 SLB MUXs，并在运行 Windows Server 2016 和 Hyper-v 的计算机上安装 SLB 主机代理。  
  
有关 System Center 2016 的详细信息，请参阅[System center 2016](https://www.microsoft.com/server-cloud/products/system-center-2016/)。  
  
> [!NOTE]  
> 如果你不想使用 System Center 2016，则可以使用 Windows PowerShell 或其他管理应用程序来安装和配置网络控制器和其他 SLB 基础结构。 有关详细信息，请参阅[使用 Windows PowerShell 部署网络控制器](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)。  
  
### <a name="network-controller"></a>网络控制器  
网络控制器托管 SLB 管理器并为 SLB 执行以下操作。  
  
-   处理从 System Center、Windows PowerShell 或其他网络管理应用程序通过 Northbound API 传入的 SLB 命令。  
  
-   计算分发到 Hyper-v 主机和 SLB Mux 的策略。  
  
-   提供 SLB 基础结构的运行状况状态。  
  
### <a name="slb-mux"></a>SLB MUX  
SLB MUX 处理入站网络流量，并将 Vip 映射到 Dip，然后将流量转发到正确的 DIP。 每个 MUX 还使用 BGP 将 VIP 路由发布到边缘路由器。 当 MUX 发生故障时，BGP 保持活动状态会通知 Mux，这允许活动的 Mux 在发生 MUX 故障时重新发布负载-实质上是为负载均衡器提供负载均衡。  
  
### <a name="hosts-that-are-running-hyper-v"></a>运行 Hyper-v 的主机  
可以将 SLB 用于运行 Windows Server 2016 和 Hyper-v 的计算机。 Hyper-v 主机上的 Vm 可以运行 Hyper-v 支持的任何操作系统。  
  
### <a name="slb-host-agent"></a>SLB 主机代理  
部署 SLB 时，必须使用 System Center、Windows PowerShell 或其他管理应用程序在每个 Hyper-v 主机上部署 SLB 主机代理。 你可以在提供 Hyper-v 支持的所有版本的 Windows Server 2016 上安装 SLB 主机代理，包括 Nano Server。  
  
SLB 主机代理从网络控制器侦听 SLB 策略更新。 此外，主机代理会将 SLB 规则用于在本地计算机上配置的启用了 SDN 的 Hyper-v 虚拟交换机。  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>已启用 SDN 的 Hyper-v 虚拟交换机  
要使虚拟交换机与 SLB 兼容，你必须使用 Hyper-v 虚拟交换机管理器或 Windows PowerShell 命令来创建交换机，然后必须为虚拟交换机启用虚拟筛选平台（VFP）。  
  
有关在虚拟交换机上启用 VFP 的信息，请参阅 Windows PowerShell 命令[get-vmsystemswitchextension](https://technet.microsoft.com/library/hh848603.aspx)和[VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx?f=255&MSPPError=-2147217396)。  
  
启用了 SDN 的 Hyper-v 虚拟交换机为 SLB 执行以下操作。  
  
-   处理 SLB 的数据路径。  
  
-   从 MUX 接收入站网络流量。  
  
-   绕过用于出站网络流量的 MUX，使用 DSR 将其发送到路由器。  
  
-   在 Hyper-v 的 Nano Server 实例上运行。  
  
### <a name="bgp-enabled-router"></a>启用 BGP 的路由器  
BGP 路由器对 SLB 执行以下操作。  
  
-   使用 ECMP 将入站流量路由到 MUX。  
  
-   对于出站网络流量，使用主机提供的路由。  
  
-   侦听来自 SLB MUX 的 Vip 的路由更新。  
  
-   如果保持活动失败，则从 SLB 轮换中删除 SLB Mux。  
  
## <a name="bkmk_features"></a>软件负载平衡功能  
下面是 SLB 的一些特性和功能。  
  
**核心功能**  
  
-   SLB 为 "北-南部" 和 "东-西" TCP/UDP 流量提供第4层负载均衡服务  
  
-   可以在基于 Hyper-v 网络虚拟化的网络上使用 SLB  
  
-   对于连接到启用了 SDN 的 Hyper-v 虚拟交换机的 DIP Vm，可以将 SLB 与基于 VLAN 的网络结合使用。  
  
-   一个 SLB 实例可以处理多个租户  
  
-   SLB 和 DIP 支持可缩放且低延迟的返回路径，该路径由直接服务器返回（DSR）实现  
  
-   使用交换机嵌入组合（SET）或单根输入/输出虚拟化（SR-IOV）时的 SLB 函数  
  
-   SLB 包含 Internet 协议版本4（IPv4）支持  
  
-   对于站点到站点网关方案，SLB 提供了 NAT 功能，以允许所有站点到站点连接使用单个公共 IP  
  
-   你可以在 Windows Server 2016、Full、Core 和 Nano 安装上安装 SLB，包括主机代理和 MUX。  
  
**规模和性能**  
  
-   适用于云规模，包括横向扩展功能，以及 Mux 和主机代理的扩展功能。  
  
-   一个活动的 SLB 管理器网络控制器模块可以支持8个 MUX 实例  
  
**高可用性**  
  
-   可以在主动/主动配置中将 SLB 部署到2个以上的节点  
  
-   可以在 MUX 池中添加和删除 Mux，而不会影响 SLB 服务。 这会在以下情况下维护 SLB 可用性   
    正在修补单个 Mux。  
  
-   各个 MUX 实例的运行时间为 99%  
  
-   运行状况监视数据可用于管理实体  
  
**关联**  
  
-   可以通过 SCVMM 部署和配置 SLB  
  
-   SLB 通过与 Microsoft 设备（如 RAS 多租户网关、数据中心防火墙和路由反射器）无缝集成，提供多租户统一边缘。  
  

