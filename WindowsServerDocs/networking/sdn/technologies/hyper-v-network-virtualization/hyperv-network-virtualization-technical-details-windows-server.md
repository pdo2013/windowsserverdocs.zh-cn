---
title: Windows Server 2016 中的 hyper-v 网络虚拟化技术详细信息
description: 本主题提供有关 Windows Server 2016 中的 Hyper-v 网络虚拟化的技术信息
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9efe0231-94c1-4de7-be8e-becc2af84e69
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e692384e9416e21e00556af6ada9af8df1713a03
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405861"
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>Windows Server 2016 中的 hyper-v 网络虚拟化技术详细信息

>适用于：Windows Server 2016

服务器虚拟化能让多个服务器实例在同一台物理主机上同步运行，但各个服务器实例都是相互独立的。 每台虚拟机的运作本质上就像是只有这一台服务器在该物理计算机上运行。  

网络虚拟化提供了类似的功能，其中，多个虚拟网络（可能包含重叠的 IP 地址）在同一个物理网络基础结构上运行，每个虚拟网络的运行方式就像是在共享的网络基础结构。 图 1 显示了此关系。  

![服务器虚拟化与网络虚拟化](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)  

图 1：服务器虚拟化与网络虚拟化  

## <a name="hyper-v-network-virtualization-concepts"></a>Hyper-V 网络虚拟化概念  
在 Hyper-v 网络虚拟化（HNV）中，客户或租户定义为部署在企业或数据中心的一组 IP 子网的 "所有者"。 客户可以是一个公司或企业，其中包含多个部门或业务单位的专用数据中心，该数据中心需要网络隔离或由服务提供商托管的公共数据中心中的租户。 每个客户都可以在数据中心有一个或多个[虚拟网络](#VirtualNetworks)，并且每个虚拟网络由一个或多个[虚拟子网](#VirtualSubnets)组成。  

Windows Server 2016 中提供了两个 HNV 实现：HNVv1 和 HNVv2。  

-   **HNVv1**  

    HNVv1 与 Windows Server 2012 R2 和 System Center 2012 Virtual Machine Manager R2 （VMM）兼容。 HNVv1 的配置依赖于 WMI 管理和 Windows PowerShell cmdlet （通过 System Center VMM 加速）来定义隔离设置和客户地址（CA）-虚拟网络到物理地址（PA）映射和路由。 在 Windows Server 2016 中没有添加到 HNVv1 的其他功能，并且未计划任何新功能。  

    •集组合和 HNV V1 与平台不兼容。

    o 若要使用 HA NVGRE 网关，用户需要使用 LBFO 团队或无需团队。 或

    o 将网络控制器部署的网关与组分组交换机一起使用。


-   **HNVv2**  

    HNVv2 中包含了大量的新功能，这些功能是使用 Hyper-v 交换机中的 Azure 虚拟筛选平台（VFP）转发扩展实现的。 HNVv2 完全与 Microsoft Azure Stack，其中包括软件定义的网络（SDN）堆栈中的新网络控制器。  虚拟网络策略通过 Microsoft[网络控制器](../../../sdn/technologies/network-controller/Network-Controller.md)使用 RESTful NORTHBOUND （NB） API 来定义，并通过多个 SouthBound INTEFACES （SBI）（包括 OVSDB）查明主机代理。 主机代理会在强制执行它的 Hyper-v 交换机的 VFP 扩展中计划策略。  

    > [!IMPORTANT]  
    > 本主题重点介绍 HNVv2。  

### <a name="VirtualNetworks"></a>虚拟网络  

-   每个虚拟网络由一个或多个虚拟子网组成。 虚拟网络构成隔离边界，在该边界内虚拟网络中的虚拟机只能相互通信。 传统上，这种隔离是使用具有隔离 IP 地址范围和 802.1 q 标记或 VLAN ID 的 Vlan 强制实施的。 但对于 HNV，将使用 NVGRE 或 VXLAN 封装来创建覆盖网络，从而能够在客户或租户之间重叠 IP 子网。  

-   每个虚拟网络在主机上都有一个唯一的路由域 ID （RDID）。 此 RDID 大致映射到资源 ID 以标识网络控制器中的虚拟网络 REST 资源。 使用带有追加的资源 ID 的统一资源标识符（URI）命名空间引用虚拟网络 REST 资源。  

### <a name="VirtualSubnets"></a>虚拟子网  

-   虚拟子网为同一个虚拟子网中的虚拟机实行第三层 IP 子网语义。 虚拟子网形成一个广播域（与 VLAN 类似），并使用 "NVGRE 租户网络 ID" （TNI）或 "VXLAN 网络标识符（VNI）" 字段强制实施隔离。  

-   每个虚拟子网都属于单个虚拟网络（RDID），并使用封装的数据包标头中的 TNI 或 VNI 密钥为其分配唯一的虚拟子网 ID （VSID）。 VSID 在数据中心内必须唯一，并且在4096到 2 ^ 24-2 的范围内。  

虚拟网络和路由域的主要优点是它允许客户将自己的网络拓扑（例如 IP 子网）引入到云中。 在图 2 的示例中，Contoso 公司有两个不同的网络：研发网络和销售网络。 由于这些网络有不同的路由域 ID，因此彼此之间不能进行交互。 也就是说，Contoso 研发网络与 Contoso 销售网络相互独立，尽管它们都属于 Contoso 公司。Contoso 研发网络包含三个虚拟子网。 注意：一个数据中心内的 RDID 和 VSID 都是唯一的。  

![客户网络和虚拟子网](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)  

图 2：客户网络和虚拟子网  

**第2层转发**  

在图2中，VSID 5001 中的虚拟机可将其数据包转发到也在 VSID 5001 通过 Hyper-v 交换机的虚拟机。 将 VSID 5001 中虚拟机传入的数据包发送到 Hyper-v 交换机上的特定 VPort。 Hyper-v 交换机将入口规则（例如 encap）和映射（例如封装标头）应用于这些数据包。 然后，将数据包转发到 Hyper-v 交换机上的不同 VPort （如果目标虚拟机连接到同一主机）或其他主机上的不同 Hyper-v 交换机（如果目标虚拟机位于不同的主机上）。  

**第3层路由**  

同样，VSID 5001 中的虚拟机可将其数据包路由到 VSID 5002 或 VSID 5003 中的虚拟机，每个 Hyper-v 主机的 VSwitch 中都有 HNV 分布式路由器。 将数据包传递到 Hyper-v 交换机后，HNV 会将传入数据包的 VSID 更新为目标虚拟机的 VSID。 这只有在两个 VSID 都属于一个 RDID 的情况下才能实现。  因此，具有 RDID1 的虚拟网络适配器无法通过网关将数据包发送到带有 RDID2 的虚拟网络适配器。  

> [!NOTE]  
> 在上述的数据包流描述中，术语 "虚拟机" 实际上指的是虚拟机上的虚拟网络适配器。 通常一台虚拟机只有一个虚拟网络适配器。 在这种情况下，单词 "虚拟机" 和 "虚拟网络适配器" 在概念上是相同的。  

每个虚拟子网定义一个第三层 IP 子网以及一个类似于 VLAN 的第二层 (L2) 广播域边界。 当虚拟机广播数据包时，HNV 使用单播复制（UR）复制原始数据包，并将目标 IP 和 MAC 替换为同一 VSID 中的每个 VM 的地址。  

> [!NOTE]  
> Windows Server 2016 出厂后，将使用单播复制来实现广播和子网多播。 不支持跨子网多播路由和 IGMP。  

除作为一个广播域外，VSID 也能提供隔离。 HNV 中的虚拟网络适配器连接到一个 Hyper-v 交换机端口，该端口会将 ACL 规则直接应用到端口（virtualNetworkInterface REST 资源）或它所属的虚拟子网（VSID）。  

Hyper-v 交换机端口必须应用 ACL 规则。 此 ACL 可以允许所有、全部拒绝或更具体，只允许基于5元组（源 IP、目标 IP、源端口、目标端口、协议）匹配的特定类型的流量。  

> [!NOTE]  
> Hyper-v 交换机扩展不适用于新的软件定义的网络（SDN）堆栈中的 HNVv2。 HNVv2 是使用 Azure 虚拟筛选平台（VFP）交换机扩展实现的，不能与任何其他第三方交换机扩展结合使用。  

## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>Hyper-v 网络虚拟化中的切换和路由  
HNVv2 实现了正确的第2层（L2）切换和第3层（L3）路由语义，作为物理交换机或路由器的工作方式。 当连接到 HNV 虚拟网络的虚拟机尝试与同一虚拟子网（VSID）中的另一台虚拟机建立连接时，它首先需要了解远程虚拟机的 CA MAC 地址。 如果源虚拟机的 ARP 表中存在目标虚拟机的 IP 地址的 ARP 条目，则使用此条目中的 MAC 地址。 如果某个条目不存在，则源虚拟机将发送一条 ARP 广播，其中包含与目标虚拟机的 IP 地址相对应的 MAC 地址的请求。 Hyper-v 交换机将截获此请求，并将其发送到主机代理。 主机代理会在其本地数据库中查找所请求目标虚拟机的 IP 地址的相应 MAC 地址。  

> [!NOTE]  
> 用作 OVSDB 服务器的主机代理使用 VTEP 架构的变体来存储 CA-PA 映射、MAC 表等。  

如果 MAC 地址可用，主机代理会注入 ARP 响应并将其发送回虚拟机。 虚拟机的网络堆栈包含所有必需的 L2 标头信息之后，会将该帧发送到 V-交换机上相应的 Hyper-v 端口。 Hyper-v 交换机在内部针对分配给 V 端口的 N 元组匹配规则测试此帧，并根据这些规则对帧应用某些转换。 最重要的是，使用 NVGRE 或 VXLAN 来构造封装标头的一组封装转换，具体取决于网络控制器上定义的策略。 使用 CA-PA 映射来确定目标虚拟机所在的 Hyper-v 主机的 IP 地址，这取决于主机代理所设置的策略。 Hyper-v 交换机确保将正确的路由规则和 VLAN 标记应用到外部数据包，使其达到远程 PA 地址。  

如果连接到 HNV 虚拟网络的虚拟机要创建与不同虚拟子网（VSID）中的虚拟机的连接，则需要相应地路由数据包。 HNV 假设有一个星型拓扑，其中在 CA 空间中只有一个 IP 地址用作下一跃点，以达到所有 IP 前缀（即，一个默认路由/网关）。 目前，这会对单个默认路由强制实施限制，不支持非默认路由。  

### <a name="routing-between-virtual-subnets"></a>虚拟子网之间的路由  
在物理网络中，IP 子网是第2层（L2）域，其中的计算机（虚拟和物理）可以直接相互通信。 L2 域是一个广播域，其中的 ARP 条目（IP： MAC 地址映射）通过在所有接口上广播的 ARP 请求进行了解，ARP 响应发送回发出请求的主机。 计算机使用从 ARP 响应中获知的 MAC 信息来完全构造 L2 帧（包括以太网标头）。 但是，如果 IP 地址在不同的 L3 子网中，则 ARP 请求不会跨越此 L3 边界。 相反，具有源子网中 IP 地址的 L3 路由器接口（下一个跃点或默认网关）必须使用其自己的 MAC 地址对这些 ARP 请求做出响应。  

在标准 Windows 网络中，管理员可以创建静态路由，并将其分配到网络接口。 此外，通常会将 "默认网关" 配置为接口上的下一个跃点 IP 地址，其中发送到默认路由（0.0.0.0/0）的数据包。 如果不存在特定的路由，则会将数据包发送到此默认网关。 通常，此网关就是物理网络的路由器。  HNV 使用属于每个主机的内置路由器，并在每个 VSID 中提供一个接口，以创建虚拟网络的分布式路由器。  

由于 HNV 采用星型拓扑，HNV 分布式路由器将充当同一 VSID 网络的一部分的虚拟子网之间的所有流量的单个默认网关。 用作默认网关的地址默认为 VSID 中的最低 IP 地址，并分配给 HNV 分布式路由器。 对于 VSID 网络内的所有流量，此分布式路由器都可以进行适当地路由，因为每个主机可以直接将流量路由到相应的主机，而无需使用中介。  当 VM 网络相同但虚拟子网不同的两台虚拟机位于同一台物理主机上时，尤其能够看到这种优势。  本节稍后你将会看到，数据包永远不用离开物理主机。  

### <a name="routing-between-pa-subnets"></a>PA 子网之间的路由  
与为每个虚拟子网（VSID）分配一个 PA IP 地址的 HNVv1 相比，HNVv2 现在使用每个交换机嵌入组合（SET） NIC 团队成员的一个 PA IP 地址。 默认部署假定两个 NIC 组，并为每个主机分配两个 PA IP 地址。 单个主机具有从同一 VLAN 上的同一提供程序（PA）逻辑子网分配的 PA Ip。 同一虚拟子网中的两个租户 Vm 实际上可能位于连接到两个不同的提供商逻辑子网的两个不同主机上。 HNV 将根据 CA-PA 映射构造封装的数据包的外部 IP 标头。 但是，它依赖于默认 PA 网关的主机 TCP/IP 堆栈到 ARP，然后基于 ARP 响应生成外部以太网标头。 通常，此 ARP 响应来自主机所连接的物理交换机或 L3 路由器上的 SVI 接口。 因此，HNV 依赖于 L3 路由器在提供程序逻辑子网/Vlan 之间路由封装的数据包。  

### <a name="routing-outside-a-virtual-network"></a>虚拟网络外部的路由  
大多数客户部署将需要 HNV 环境与不属于 HNV 环境的资源进行通信。 这就需要网络虚拟化网关来实现两种环境之间的通信。 需要 HNV 网关的基础结构包括私有云和混合云。 基本上，在使用 IPSec VPN 或 GRE 隧道的内部和外部（物理）网络（包括 NAT）或不同站点和/或云（专用或公用）之间进行第3层路由需要 HNV 网关。  

网关可以有不同的实体构成元素。 它们可以构建在 Windows Server 2016 上，合并到充当 VXLAN 网关的顶级架（TOR）交换机，可通过负载均衡器公布的虚拟 IP （VIP）访问，放入其他现有的网络设备，也可以是新的独立网络设备.  

有关 Windows RAS 网关选项的详细信息，请参阅[RAS 网关](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md)。  

## <a name="packet-encapsulation"></a>数据包封装  
在 HNV 中的每个虚拟网络适配器都与两个 IP 地址相关：  

-   **客户地址**（CA）客户基于其 intranet 基础结构分配的 IP 地址。 该地址允许客户与虚拟机交换网络流量，如同该虚拟机尚未移至私有云或公有云一样。 CA 对虚拟机是可见的，并且可由客户访问。  

-   **提供商地址**（PA）由托管提供商或数据中心管理员基于其物理网络基础结构分配的 IP 地址。 PA 出现在网络上的数据包中，这些数据包可与托管虚拟机的、运行 Hyper-V 的服务器进行交换。 PA 在物理网络上是可见的，但在虚拟机上不可见。  

CA 维护着客户的网络拓扑，该网络拓扑经过虚拟化，可采用 PA 的实施方式，从实际的基础物理网络拓扑中脱离出来。 下图展示了因网络虚拟化而在虚拟机 CA 与网络基础设施 PA 之间所建立的概念关系。  

![物理基础结构的网络虚拟化概念图](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)  

图 6：物理基础结构的网络虚拟化概念图  

在此图中，客户虚拟机在 CA 空间中发送数据包，该数据包通过其自身的虚拟网络或 "隧道" 遍历物理网络基础结构。 在上面的示例中，可以将隧道视为 Contoso 和 Fabrikam 数据包的 "信封"，并将绿色发货标签（PA 地址）从源主机从源主机传递到右侧的目标主机。 主要是指主机如何确定与 Contoso 和 Fabrikam CA 相对应的 "发货地址" （PA）、"信封" 如何环绕数据包，以及目标主机如何将数据包解包并传送到 Contoso 和 Fabrikam正确的目标虚拟机。  

这一简单的类比突显出网络虚拟化的重要方面：  

-   将每台虚拟机的 CA 映射到物理主机的 PA。 同一个 PA 可能有多个关联的 CA。  

-   虚拟机在 CA 空间中发送数据包，并根据映射将其放入 "信封"，其中包含 PA 源和目标对。  

-   CA-PA 映射必须允许主机为不同的客户虚拟机区分数据包。  

因此，网络虚拟化机制旨在将虚拟机使用的网络地址虚拟化。 网络控制器负责地址映射，主机代理使用 MS_VTEP 架构维护映射数据库。 下一部分将描述地址虚拟化的实际机制。  

## <a name="network-virtualization-through-address-virtualization"></a>通过地址虚拟化实现网络虚拟化  
HNV 使用网络虚拟化通用路由封装（NVGRE）或虚拟可扩展局域网（VXLAN）实现覆盖型租户网络。  默认值为 VXLAN。  

### <a name="virtual-extensible-local-area-network-vxlan"></a>虚拟可扩展局域网（VXLAN）  
虚拟可扩展局域网（VXLAN）（[RFC 7348](https://www.rfc-editor.org/info/rfc7348)）协议在市场上广泛采用，并支持来自 Cisco、织锦、Arista、DELL、HP 和其他供应商的供应商。 VXLAN 协议使用 UDP 作为传输。 IANA 为 VXLAN 分配的 UDP 目标端口为4789，UDP 源端口应为要用于 ECMP 传播的内部数据包的信息哈希。 在 UDP 标头之后，VXLAN 标头将追加到数据包，其中包含一个保留的4字节字段，后跟一个3字节字段，其中包含 VXLAN 网络标识符（VNI）-VSID-后跟另一个保留的1字节字段。 VXLAN 标头后，将追加原始 CA L2 帧（不带 CA 以太网帧 FCS）。  

![VXLAN 数据包标头](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)  

### <a name="generic-routing-encapsulation-nvgre"></a>通用路由封装（NVGRE）  
此网络虚拟化机制使用通用路由封装（NVGRE）作为隧道标头的一部分。 在 NVGRE 中，虚拟机的数据包封装在另一个数据包中。 如图 7 所示，新的数据包报头含有合适的源和目标 PA IP 地址，另外还有存储在 GRE 报头密钥字段中的虚拟子网 ID。  

![NVGRE 封装](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)  

图 7：网络虚拟化 - NVGRE 封装  

虚拟子网 ID 允许主机标识任何给定包的客户虚拟机，即使数据包上的 PA 和 CA 可能会重叠。 这可让同一台主机上的所有虚拟机分享一个 PA（如图 7 所示）。  

共享 PA 对网络可扩展性产生很大的影响。 网络基础设施必须知悉的 IP 和 MAC 地址数量得以大幅减少。 例如，如果每台终端主机平均有 30 台虚拟机，网络基础设施需要知悉的 IP 和 MAC 地址数量将减少到三十分之一，数据包中嵌入式虚拟子网 ID 还能轻易地将数据包与实际客户联系起来。  

Windows Server 2012 R2 的 PA 共享方案为每个主机每个 VSID 一个 PA。 对于 Windows Server 2016，方案是每个 NIC 团队成员的一个 PA。  

对于 Windows Server 2016 及更高版本，HNV 完全支持 NVGRE 和 VXLAN。它不需要升级或购买新的网络硬件，如 Nic （网络适配器）、交换机或路由器。 这是因为网络上的这些数据包是 PA 空间中的常规 IP 数据包，与当前的网络基础结构兼容。  但是，若要获得最佳性能，请使用支持任务卸载的最新驱动程序所支持的 Nic。  

## <a name="multi-tenant-deployment-example"></a>多租户部署示例  
下图显示了一个部署示例，其中包含由网络策略定义的 CA-PA 关系的云数据中心中的两个客户。  

![多租户部署示例](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)  

图 8：多租户部署示例  

思考图 8 中的示例。 在转到托管提供商的共享 IaaS 服务之前：  

-   Contoso 公司运行的是一台 SQL Server（名为 **SQL**），IP 地址为 10.1.1.11，以及一台 Web 服务器（名为 **Web**）IP 地址为 10.1.1.12，并用其 SQL Server 处理数据库事务。  

-   Fabrikam 公司运行的是一台 SQL Server（也名为 **SQL** ），IP 地址为 10.1.1.11，以及一台 Web 服务器（也名为 **Web** ），IP 地址为 10.1.1.12，并用其 SQL Server 处理数据库事务。  

我们假设托管服务提供商之前通过网络控制器创建了提供程序（PA）逻辑网络，以对应其物理网络拓扑。 网络控制器从连接主机的逻辑子网的 IP 前缀分配两个 PA IP 地址。 网络控制器还指示用于应用 IP 地址的相应 VLAN 标记。  

使用网络控制器，Contoso Corp 和 Fabrikam 公司创建由托管服务提供商指定的提供者（PA）逻辑网络支持的虚拟网络和子网。 Contoso 公司和 Fabrikam 公司将其各自的 SQL Server 和 Web 服务器移至同一托管提供商的共享 IaaS 服务，而巧合的是，这些服务器在 Hyper-V 主机 1 中运行 **SQL** 虚拟机，在 Hyper-V 主机 2 中运行 **Web** (IIS7) 虚拟机。 所有虚拟机都保持其原有的内部网 IP 地址（其 CA）。  

网络控制器为两家公司都分配了以下虚拟子网 ID （VSID），如下所示。  每个 Hyper-v 主机上的主机代理从网络控制器接收分配的 PA IP 地址，并在非默认网络隔离舱中创建两个 PA 主机 Vnic。 将向其中分配 PA IP 地址的每个主机 Vnic 分配一个网络接口，如下所示：  

-   Contoso 公司的虚拟机 VSID 和 PAs：**VSID**为5001， **SQL pa**为192.168.1.10， **Web pa**为192.168.2.20  

-   Fabrikam 公司的虚拟机 VSID 和 PAs：**VSID**为6001， **SQL pa**为192.168.1.10， **Web pa**为192.168.2.20  

网络控制器将所有网络策略（包括 CA-PA 映射）联结到 SDN 主机代理，该代理将在持久存储区（OVSDB 数据库表中）维护策略。  

当 Hyper-v 主机2上的 Contoso 公司 Web 虚拟机（10.1.1.12）与10.1.1.11 上的 SQL Server 建立 TCP 连接时，会发生以下情况：  

-   10.1.1.11 的目标 MAC 地址的 VM ARPs  

-   VSwitch 中的 VFP 扩展会截获此数据包，并将其发送给 SDN 主机代理  

-   SDN 主机代理会在其策略存储中查找10.1.1.11 的 MAC 地址  

-   如果找到 MAC，主机代理会将 ARP 响应注入回 VM  

-   如果未找到 MAC，则不会发送响应，并将10.1.1.11 中的 ARP 条目标记为无法访问。  

-   VM 现在使用正确的 CA 以太网和 IP 标头构造 TCP 数据包，并将其发送给 vSwitch  

-   VSwitch 中的 VFP 转发扩展可通过分配给接收数据包的源 vSwitch 端口的 VFP 层处理此数据包，并在 VFP 统一流表中创建新的流输入  

-   VFP 引擎基于 IP 和以太网标头对每个层（例如虚拟网络层）执行规则匹配或流表查找。  

-   虚拟网络层中的匹配规则引用 CA-PA 映射空间并执行封装。  

-   在 VNet 层中，将封装类型（VXLAN 或 NVGRE）与 VSID 一起指定。  

-   对于 VXLAN 封装，将使用 VXLAN 标头中的 VSID 5001 构造外部 UDP 标头。  
    外部 IP 标头是使用分配给 Hyper-v 主机2（192.168.2.20）和 Hyper-v 主机1（192.168.1.10）的源和目标 PA 地址，分别基于 SDN 主机代理的策略存储来构造。  

-   然后，此数据包流向 VFP 中的 PA 路由层。  

-   VFP 中的 PA 路由层将引用用于 PA 空间流量和 VLAN ID 的网络隔离舱，并使用主机的 TCP/IP 堆栈正确地将 PA 数据包转发给 Hyper-v 主机1。  

-   收到封装的数据包后，Hyper-v 主机1会接收 PA 网络隔离舱中的数据包，并将其转发给 vSwitch。  

-   VFP 通过其 VFP 层处理数据包，并在 VFP 统一 flow 表中创建新的流条目。  

-   VFP 引擎匹配虚拟网络层中的 ingres 规则，并去除外部封装的以太网、IP 和 VXLAN 标头。  

-   然后，VFP 引擎将数据包转发到目标 VM 连接到的 vSwitch 端口。  

Fabrikam 公司 **Web** 与 **SQL** 虚拟机之间用于通信的类似进程针对 Fabrikam 公司使用 HNV 策略设置。因此，在使用 HNV 时，Fabrikam 公司和 Contoso 公司虚拟机的交互就像在其原有的 Intranet 中进行一样。 尽管两家公司虚拟机使用的是相同的 IP 地址，也不会互相影响。  

不同地址（Ca 和 PAs）、Hyper-v 主机的策略设置以及用于入站和出站虚拟机通讯的 CA 与 PA 之间的地址转换使用 NVGRE 键或 VLXAN VNID 隔离这些服务器集。 同时，虚拟化映射以及转化将虚拟网络构架从物理网络基础结构中脱离出来。 尽管 Contoso **SQL** 和 **Web** 以及 Fabrikam **SQL** 和**Web** 位于其自己的 CA IP 子网 (10.1.1/24)，两者的物理部署在两台有着不同的 PA 子网（分别为 192.168.1/24 和 192.168.2/24）的主机上进行。 这意味着通过 HNV，可以进行跨子网虚拟机配置和实时迁移。  

## <a name="hyper-v-network-virtualization-architecture"></a>Hyper-V 网络虚拟化架构  
在 Windows Server 2016 中，HNVv2 是使用 Azure 虚拟筛选平台（VFP）实现的，该平台是 Hyper-v 交换机内的 NDIS 筛选扩展。 VFP 的主要概念是匹配操作流引擎，其中包含一个向 SDN 主机代理公开的内部 API，用于编程网络策略。 SDN 主机代理本身通过 OVSDB 和 WCF SouthBound 通信通道从网络控制器接收网络策略。 不只是使用 VFP 编程的虚拟网络策略（如 CA-PA 映射），而是使用 Acl、QoS 等其他策略。  

VSwitch 和 VFP 转发扩展的对象层次结构如下所示：  

-   vSwitch  

    -   外部 NIC 管理  

    -   NIC 硬件卸载  

    -   全局转发规则  

    -   Port  

        -   用于头发的出口转发层  

        -   映射和 NAT 池的空间列表  

        -   统一流表  

        -   VFP 层  

            -   Flow 表  

            -   Group  

            -   规则  

                -   规则可以引用空格  

在 VFP 中，每个策略类型（例如，"虚拟网络"）创建一个层，并为一组通用的规则/流表。 在将特定规则分配给该层以实现此类功能之前，它不具有任何内部功能。 为每个层分配一个优先级，并按升序优先级将层分配给某个端口。 规则根据方向和 IP 地址系列主要分为组。 还会为组分配一个优先级，最多可以有一个组中的一个规则与给定的流匹配。  

VFP 扩展的 vSwitch 的转发逻辑如下所示：  

-   入口处理（进出传入端口的数据包的观点）  

-   转发  

-   传出处理（来自数据包离开端口的位置的出口）  

VFP 支持 NVGRE 和 VXLAN 封装类型以及基于外部 MAC VLAN 的转发的内部 MAC 转发。  

VFP 扩展具有一个慢速路径和一个用于数据包遍历的快速路径。 流中的第一个数据包必须遍历每个层中的所有规则组，并进行规则查找，这是一项成本高昂的操作。 但是，在使用操作列表（基于匹配的规则）将流注册到统一流表中后，将根据统一流表项来处理所有后续数据包。  

HNV 策略由主机代理进行编程。 每个虚拟机网络适配器都配置有一个 IPv4 地址。 这些是虚拟机用来相互交流的 CA，并置于虚拟机的 IP 包中。 HNV 基于主机代理的数据库中存储的网络虚拟化策略，将 CA 帧封装在 PA 帧中。  

![HNV 体系结构](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)  

图 9：HNV 体系结构  

## <a name="summary"></a>总结  
基于云的数据中心能带来许多好处，如改善可扩展性和资源使用情况。 为了实现这些潜在的好处，需要能基本解决动态环境中多租户可扩展性问题的技术。 HNV 旨在通过分离物理网络拓扑的虚拟网络拓拟来解决这些问题，并提高数据中心的运营效率。 基于现有标准，HNV 在当前的数据中心运行，并使用现有的 VXLAN 基础结构进行操作。 使用 HNV 的客户现在可以将其数据中心整合到私有云中，或者将其数据中心无缝扩展到使用混合云的托管服务器提供商环境。  

## <a name="BKMK_LINKS"></a>另请参阅  
若要了解有关 HNVv2 的详细信息，请参阅以下链接：  


|       内容类型       |                                                                                                                                              参考资料                                                                                                                                              |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **社区资源**  |                                                                -   [私有云体系结构博客](https://blogs.technet.com/b/privatecloud)<br />-提出问题： [cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)                                                                |
|         **RFC**          |                                                                   @no__t[NVGRE 草案 RFC](https://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />@NO__T[VXLAN-RFC 7348](https://www.rfc-editor.org/info/rfc7348)                                                                    |
| **相关技术** | -有关 Windows Server 2012 R2 中的 Hyper-v 网络虚拟化技术详细信息，请参阅[Hyper-v 网络虚拟化技术详细信息](https://technet.microsoft.com/library/jj134174.aspx)<br />-   [网络控制器](../../../sdn/technologies/network-controller/Network-Controller.md) |

