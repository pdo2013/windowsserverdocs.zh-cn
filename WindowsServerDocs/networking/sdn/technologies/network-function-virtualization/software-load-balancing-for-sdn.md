---
title: 软件负载平衡 SDN (SLB)
description: 你可以使用本主题以了解软件负载平衡软件定义网络在 Windows Server 2016 的。
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
ms.openlocfilehash: 5e08afddde9c7be8d955a0cfdaf44f0fc31b8155
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="software-load-balancing-slb-for-sdn"></a>软件负载平衡 SDN \(SLB\)

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解软件负载平衡软件定义网络在 Windows Server 2016 的。  

云服务提供商 (Csp) 和部署软件定义网络 (SDN) 在 Windows Server 2016 的企业可以使用软件负载平衡 (SLB) 激增，均匀地分布租户和虚拟网络资源的租户客户网络通信。 Windows Server SLB 使主持相同的工作负载，多个服务器高可用性和可扩展性提供。
  
Windows Server SLB 包含以下功能。  
  
-   第 4 (第 4) 层加载北美南和东西 ' TCP/UDP 交通平衡服务。  
  
-   公开并内部网络交通负载平衡。  
  
-   在你使用 HYPER-V 网络虚拟化创建的虚拟网络上的虚拟本地网络 (Vlan) 和支持动态 IP 地址 (Dip)。  
  
-   健康探测支持。  
  
-   准备好迎接云比例，包括规模功能和路复和主机代理扩大功能。  
  
有关详细信息，请参阅[软件平衡功能](#bkmk_features)本主题中。  
  
> [!NOTE]  
> 有关 Vlan 不支持网络控制器多租赁，但是你可以使用 Vlan SLB 与服务提供商管理工作负载，如 datacenter 基础结构和高密度 Web 服务器。  
  
使用 Windows Server SLB，你可以放大你负载平衡用于其他 VM 工作负载的同一 HYPER-V 计算服务器上使用 SLB 虚拟机的功能。 出于此原因，SLB 支持快速创建和删除负载平衡端点所需的 CSP 操作。 此外，Windows Server SLB 支持数十亿每个群集字节，提供一种简单配置模型，并且易于缩放和查看。  
  
**SLB 的工作原理**  
  
SLB 的工作原理是映射到动态 IP 地址 (Dip) 的云服务的一组资源数据中心中的虚拟 IP 地址 (VIPs)。  
  
VIPs 是一个提供公众加载的池访问平衡虚拟机的功能的 IP 地址。 例如，VIPs 是，使其租户和租户客户可以连接到云数据中心中的租户资源公开 Internet 的 IP 地址。  
  
Dip 是成员负载平衡池 VIP 背后的虚拟机的功能的 IP 地址。 在云中的基础结构对租户资源分配 Dip。  
  
VIPs 位于中 SLB 集线器 （输入混合）。  输入混合由一个或多个虚拟机 (Vm)。  网络控制器提供的每个 VIP，每个输入混合和每个输入混合反过来使用边框网关协议 (BGP) 公布到路由器上的物理网络 / 32 为每个 VIP 路线。  BGP 允许到物理网络路由器：  
  
-   了解 VIP 是适用于每一输入混合，，即使 MUXes 在上一层 3 网络中的不同个子网。  
  
-   对于每个 VIP 加载跨所有可用的 MUXes 使用路由相等成本多路径 (ECMP)。  
  
-   自动检测输入混合故障或删除并停止向失败输入混合发送交通。  
  
-   跨健康 MUXes 失败的或已删除的输入混合从负载。  
  
在公共交通到达从 Internet 时，SLB 输入混合检查通信，这包含为目标，VIP 和地图，以便将到达个别冲动重写的交通。 对于传入网络通信，此交易执行需要两个进程的分隔输入混合虚拟机 (Vm) 和 HYPER-V 主机目标冲动所在的位置中：  
  
-   加载余额-输入混合使用 VIP 选择冲动，封装数据包，并将交通转发给 HYPER-V 主机冲动所在的位置。  
  
-   网络地址翻译 (NAT)-HYPER-V 主机封装删除数据包、 译为冲动 VIP、 重新映射端口，并将数据包转发到冲动 VM。  
  
输入混合知道如何映射到正确的 Dip VIPs，由于负载平衡使用网络控制器定义的策略。 这些规则包括协议、 前端端口、 后台端口和 distribution 算法 （5、 3，或者 2 元）。  
  
租户响应虚拟机的功能和发送站网络时返回到 Internet 或远程租户地点的交通因为 NAT 由 HYPER-V 主机，交通跳过输入混合并进入直接 edge 路由器从 HYPER-V 主机。 此输入混合绕过过程称为直接 Server 返回 (DSR)。  
  
并的入站的网络通信的初始网络通信流量建立后，跳过 SLB 输入混合，完全。  
  
在下图中的客户端计算机对 DNS 查询执行公司 Sharepoint 站点-在此情况下，一个名为 Contoso 虚构的公司的 IP 地址。 以下过程。  
  
-   DNS 服务器返回给客户端 VIP 107.105.47.60。  
  
-   客户端发送到 VIP HTTP 请求。  
  
-   物理网络有可供到达位于任何输入混合 VIP 的多个路径。  在此过程中每个路由器使用 ECMP 直到请求到达输入混合选取路径下一段。  
  
-   输入混合接收请求检查配置的策略，并可以看到有两个 Dip 可用，10.10.10.5 和 10.10.20.5，虚拟网络处理到 VIP 107.105.47.60 请求  
  
-   输入混合选择冲动 10.10.10.5 和封装使用 VXLAN，以便它可以将其发送到主机包含使用主机冲动物理网络地址数据包。  
  
-   主机接收封装的数据包，然后对其进行检查。  它会删除封装并以便目标现已而不是 VIP 冲动 10.10.10.5，并将交通发送冲动 VM 到重写数据包。  
  
-   请求现在已达到 Contoso Sharepoint 网站服务器电场的日落 2 中。 服务器生成一个响应，并将其发送到作为源使用其自己的 IP 地址的客户端。  
  
-   主机拦截传出数据包在虚拟交换机用来这会记住的客户端，现在目标对 VIP 原始的请求。  主机重写数据包是 VIP，因此这对于客户端看不到冲动地址的源。  
  
-   主机将直接向其标准路由表用于登录到的最终接收响应客户端数据包转发中的物理网络默认网关数据包转发。  
  
![软件负载平衡进程](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**加载平衡内部 datacenter 通信**  
  
何时加载平衡网络通信的内部 datacenter，如之间租户资源，在不同的服务器上运行，并且都相同的虚拟网络中的成员，HYPER-V 虚拟交换机用来 Vm 连接到执行 nat。  
  
与内部交通负载平衡的第一个请求已发送到和由输入混合，选择相应的冲动并将交通路由到冲动处理。 从该点前进已建立的流量跳过输入混合并直接从 VM 转到 VM。  
  
**健康探测**  
  
SLB 包括健康探测验证网络基础结构，其中包括以下的运行状况。  
  
-   TCP 探测端口  
  
-   HTTP 探测端口和 URL  
  
与探测其中来自设备，通过线缆前往冲动传统负载平衡装置，不同 SLB 探测产生了冲动所在位置和直接从 SLB 主机代理转到冲动，进一步跨主机分发工作在主机上。  
  
## <a name="bkmk_infrastructure"></a>软件负载平衡基础结构  
部署 Windows Server SLB，必须首先部署 Windows Server 2016 中的网络控制器和一个或多个 SLB 输入混合虚拟机的功能。  
  
此外，你必须 SDN 启用 HYPER-V 虚拟交换机用来配置 HYPER-V 主机，并确保 SLB 主机专员正在运行。  服务于主机路由器相等成本多路径 (ECMP) 路由和边框网关协议 (BGP)，必须支持，并且必须配置接受来自 SLB MUXes BGP 等请求。  
  
以下是概述 SLB 基础结构。  

![软件负载平衡基础结构](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
以下部分提供有关 SLB 基础结构的这些内容的详细信息。  
  
### <a name="scvmm"></a>SCVMM  
与系统中心 2016，可以配置 Windows Server 2016 年，包括 SLB 管理器和健康监视器网络控制器。 你还可以使用 System Center 部署 SLB MUXs 并运行 Windows Server 2016 和 HYPER-V 的计算机上安装 SLB 主机代理。  
  
有关系统中心 2016。 有关详细信息，请参阅[系统中心 2016年](https://www.microsoft.com/en-us/server-cloud/products/system-center-2016/)。  
  
> [!NOTE]  
> 如果你不希望使用系统中心 2016年，可以使用 Windows PowerShell 或其他管理应用程序安装和配置了网络控制器和其他 SLB 基础结构。 有关详细信息，请参阅[部署网络控制器使用 Windows PowerShell](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)。  
  
### <a name="network-controller"></a>网络控制器  
网络控制器承载 SLB 管理器，并为 SLB 执行以下操作。  
  
-   进程 SLB 来自通过 Northbound API System Center、 Windows PowerShell 或其他网络管理应用程序的命令。  
  
-   计算分发到 HYPER-V 主机和 SLB MUXes 的策略。  
  
-   提供的运行状况 SLB 基础结构。  
  
### <a name="slb-mux"></a>SLB 输入混合  
SLB 输入混合处理站的网络路况和地图 VIPs 到 Dip，，然后将交通转发到正确的冲动。 每个输入混合也使用 BGP 发布到 edge 路由器 VIP 路线。 BGP 保持通知输入混合失败时，这使活动 MUXes 要重新分发出现输入混合故障-本质上提供负载平衡负载平衡加载 MUXes。  
  
### <a name="hosts-that-are-running-hyper-v"></a>运行的 HYPER-V 的主机  
你可以使用 SLB 与运行 Windows Server 2016 和 HYPER-V 的计算机。 HYPER-V 主机上的 Vm 可以运行任何 HYPER-V 支持的操作系统。  
  
### <a name="slb-host-agent"></a>SLB 主机代理  
部署 SLB 时，你必须使用 System Center、 Windows PowerShell 或其他管理应用程序部署 SLB 主机代理 HYPER-V 主机的每台计算机上。 你可以安装 SLB 主机代理上提供了 HYPER-V 支持，包括 Nano 服务器的所有版本的 Windows Server 2016。  
  
SLB 主机代理侦听从网络控制器 SLB 策略更新。 此外，主机代理程序规则为 SLB 到 SDN 启用 HYPER-V 虚拟交换机配置本地计算机上。  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>SDN 启用 Hyper V 虚拟交换机用来  
一个虚拟交换机用来为可与 SLB，兼容的必须使用 Windows PowerShell 或 HYPER-V 虚拟切换管理器命令创建此开关，，然后你必须允许虚拟筛选平台 (VFP) 虚拟交换机用来。  
  
虚拟交换机上启用 VFP 信息，请参阅 Windows PowerShell 命令[获取 VMSystemSwitchExtension](https://technet.microsoft.com/en-us/library/hh848603.aspx)和[启用 VMSwitchExtension](https://technet.microsoft.com/en-us/library/hh848541.aspx?f=255&MSPPError=-2147217396)。  
  
SDN 启用 HYPER-V 虚拟交换机用来为 SLB 执行以下操作。  
  
-   对于 SLB 处理数据的路径。  
  
-   接收来自输入混合的入站的网络通信。  
  
-   绕过输入混合站网络交通，将其发送到使用 DSR 路由器。  
  
-   运行于 HYPER-V Nano Server 实例。  
  
### <a name="bgp-enabled-router"></a>BGP 启用路由器  
BGP 路由器 SLB 执行以下操作。  
  
-   归巢的交通到使用 ECMP 输入混合路线。  
  
-   对于站网络通信，使用主机提供的路线。  
  
-   对于从 SLB 输入混合 VIPs 侦听路线更新。  
  
-   如果保持失败，请移除 SLB MUXes 从 SLB 旋转。  
  
## <a name="bkmk_features"></a>软件平衡功能  
下面是一些共同功能和 SLB 的功能。  
  
**核心功能**  
  
-   SLB 提供一层 4 负载平衡 '北美南和东西' TCP/UDP 交通服务  
  
-   你可以使用上的 HYPER-V 网络虚拟化基于网络 SLB  
  
-   你可以与基于 VLAN 网络对冲动 Vm 连接到一个 SDN 启用 HYPER-V 虚拟交换机用来使用 SLB。  
  
-   一个 SLB 实例可以处理多个租户  
  
-   SLB 和冲动支持可缩放和低延迟返回路径，当实现通过直接 Server 返回 (DSR)  
  
-   SLB 函数还使用切换 Embedded 组队 （集） 或单根输入/输出虚拟化 (SR IOV)  
  
-   SLB 包含 Internet 协议版本 4 (IPv4) 支持  
  
-   对于站点的网关方案 SLB 提供 NAT 功能，以允许所有站点的连接利用单个公共 IP  
  
-   你可以安装 SLB，其中主机代理输入混合，Windows Server 2016 年，包括完整、 核心以及 Nano 安装。  
  
**缩放和性能**  
  
-   准备好迎接云比例，包括规模功能和 MUXes 和主机代理扩大功能。  
  
-   一个活动的 SLB 管理器网络控制器模块可以支持 8 输入混合实例  
  
**高可用性**  
  
-   你可以将 SLB 部署到超过 2 个节点在主动/主动配置  
  
-   可以添加 MUXes，并从输入混合池中删除不会影响 SLB 服务。 这将保持 SLB 可用性时   
    修补个别 MUXes。  
  
-   各个输入混合实例有 99%的运行时间  
  
-   健康监视数据是对管理实体可用  
  
**校准**  
  
-   你可以部署和 SCVMM 用来配置 SLB  
  
-   SLB 提供通过与 RAS Multitenant 网关、 数据中心防火墙和路线反射等 Microsoft 设备无缝集成的租户统一的边缘。  
  

