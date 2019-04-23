---
title: Windows Server 2016 中的 HYPER-V 网络虚拟化技术细节
description: 本主题提供有关 Windows Server 2016 中的 HYPER-V 网络虚拟化技术信息
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9efe0231-94c1-4de7-be8e-becc2af84e69
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8db47479d0c6e6014a7df6cecd59b1e87920b76a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887548"
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>Windows Server 2016 中的 HYPER-V 网络虚拟化技术细节

>适用于：Windows Server 2016

服务器虚拟化能让多个服务器实例在同一台物理主机上同步运行，但各个服务器实例都是相互独立的。 每台虚拟机的运作本质上就像是只有这一台服务器在该物理计算机上运行。  
  
网络虚拟化提供类似功能中 （可能具有重叠 IP 地址） 的多个虚拟网络运行在同一, 物理网络基础结构和每个虚拟网络操作就如同它是唯一的虚拟网络上运行共享的网络基础结构。 图 1 显示了此关系。  
  
![服务器虚拟化与网络虚拟化](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)  
  
图 1：服务器虚拟化与网络虚拟化  
  
## <a name="hyper-v-network-virtualization-concepts"></a>Hyper-V 网络虚拟化概念  
客户或租户中的 HYPER-V 网络虚拟化 (HNV) 中，指一组 IP 子网，部署到企业版或数据中心中的"所有者"。 客户可以是一家公司或企业具有多个部门或业务单位专用数据中心需要网络隔离或由服务提供商托管的公共数据中心中的租户。 每个客户可以具有一个或多个[虚拟网络](#VirtualNetworks)在数据中心和每个虚拟网络包含一个或多个[虚拟子网](#VirtualSubnets)。  
  
有两种 HNV 实现将在 Windows Server 2016 中提供：HNVv1 和 HNVv2。  
  
-   **HNVv1**  
  
    HNVv1 是与 Windows Server 2012 R2 和 System Center 2012 R2 Virtual Machine Manager (VMM) 兼容。 配置为 HNVv1 依赖于 WMI 管理和 Windows PowerShell cmdlet （借助于 System Center VMM） 来定义的隔离设置和客户地址 (CA) 的虚拟网络-与物理地址 (PA) 映射和路由。 任何其他功能已添加到 Windows Server 2016 中的 HNVv1，没有新的功能计划。  
    
    • 设置组合和 HNV V1 不兼容的平台。
    
    若使用 HA NVGRE 网关用户需要为要使用 LBFO 团队或没有团队。 或
    
    o 使用网络控制器部署的网关设置成组交换机。

  
-   **HNVv2**  
  
    大量的新功能会参与 HNVv2 这使用 Azure 虚拟筛选平台 (VFP) 转发扩展的 HYPER-V 交换机中实现的。 与 Microsoft Azure Stack，其中包括新的网络控制器中的软件定义网络 (SDN) 堆栈完全集成 HNVv2。  虚拟网络策略定义通过 Microsoft[网络控制器](../../../sdn/technologies/network-controller/Network-Controller.md)使用 RESTful NorthBound (NB) API 和检测到主机代理通过多个 SouthBound 接口 (SBI) 包括 OVSDB。 主机代理程序在实施情况 HYPER-V 交换机的 VFP 扩展中的策略。  
  
    > [!IMPORTANT]  
    > 本主题重点介绍 HNVv2。  
  
### <a name="VirtualNetworks"></a>虚拟网络  
  
-   每个虚拟网络由一个或多个虚拟子网组成。 虚拟网络构成一个独立边界，其中的虚拟网络中的虚拟机仅可以相互通信。 传统上，这种隔离并已强制实施使用分隔的 IP 地址范围和 802.1q Vlan 标记或 VLAN id。 但与 HNV，使用 NVGRE 或 VXLAN 封装，因为它的客户或租户之间的 IP 子网重叠创建的覆盖网络实施隔离。  
  
-   每个虚拟网络在主机上具有唯一路由域 ID (RDID)。 此 RDID 大致映射到一个资源 ID 来标识虚拟网络在网络控制器 REST 资源。 虚拟网络 REST 资源引用的统一资源标识符 (URI) 命名空间中使用附加的资源 id。  
  
### <a name="VirtualSubnets"></a>虚拟子网  
  
-   虚拟子网为同一个虚拟子网中的虚拟机实行第三层 IP 子网语义。 通过使用 NVGRE 租户网络 ID (TNI) 或 VXLAN 网络标识符 (VNI) 字段中强制实施隔离与虚拟子网窗体 （类似于 VLAN） 一个广播的域。  
  
-   每个虚拟子网都属于单个虚拟网络 (RDID)，并分配唯一虚拟子网 ID (VSID) 封装的数据包标头中使用的 TNI 或 VNI 密钥。 VSID 在数据中心中必须是唯一和处于范围内 4096 到 2 ^24-2。  
  
虚拟网络和路由域的主要优点到云是它允许客户将他们自己的网络拓扑 （例如，IP 子网）。 在图 2 的示例中，Contoso 公司有两个不同的网络：研发网络和销售网络。 由于这些网络有不同的路由域 ID，因此彼此之间不能进行交互。 也就是说，Contoso 研发网络与 Contoso 销售网络相互独立，尽管它们都属于 Contoso 公司。Contoso 研发网络包含三个虚拟子网。 注意：一个数据中心内的 RDID 和 VSID 都是唯一的。  
  
![客户网络和虚拟子网](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)  
  
图 2：客户网络和虚拟子网  
  
**第 2 层转发**  
  
在图 2 中 VSID 为 5001 的虚拟机可以将其转发到 VSID 为 5001 通过 HYPER-V 交换机中也存在的虚拟机的数据包。 来自中 VSID 为 5001 的虚拟机的传入数据包发送到 HYPER-V 交换机上特定 VPort。 这些数据包的 HYPER-V 交换机应用入口规则 （例如封闭） 和映射 （例如封装标头）。 数据包将被转发到 HYPER-V 交换机上不同 VPort （如果目标虚拟机附加到同一主机） 或到其他主机 （如果目标虚拟机位于一台主机上） 上的不同的 HYPER-V 交换机。  
  
**层 3 个路由**  
  
同样中 VSID 为 5001, 的虚拟机可以将其数据包路由到中 VSID 为 5002 或 5003 的虚拟机的每个 HYPER-V 主机的 VSwitch 中存在 HNV 分布式路由器。 在将数据包送往 HYPER-V 交换机，HNV 将传入数据包的 VSID 更新为目标虚拟机的 VSID。 这只有在两个 VSID 都属于一个 RDID 的情况下才能实现。  因此，具有 RDID1 的虚拟网络适配器无法而无需遍历网关，将数据包发送到带有 RDID2 的虚拟网络适配器。  
  
> [!NOTE]  
> 在上面的数据包流描述，术语"虚拟机"实际上是指虚拟机上的虚拟网络适配器。 通常一台虚拟机只有一个虚拟网络适配器。 在这种情况下，单词"虚拟机"和"虚拟网络适配器"可以从概念上讲表示同一事物。  
  
每个虚拟子网定义一个第三层 IP 子网以及一个类似于 VLAN 的第二层 (L2) 广播域边界。 当虚拟机广播数据包时，HNV 将使用单播复制 (UR) 来制作一份原始数据包并将替换每个 VM 中与 VSID 相同的地址的目标 IP 和 MAC。  
  
> [!NOTE]  
> 当 Windows Server 2016 附带时，将使用单播复制实现广播和子网多播。 不支持跨子网多播路由和 IGMP。  
  
除作为一个广播域外，VSID 也能提供隔离。 HNV 中的虚拟网络适配器连接到 HYPER-V 交换机端口 ACL 规则应用到的端口 （virtualNetworkInterface REST 资源） 直接或虚拟子网 (VSID) 它的一部分包含。  
  
HYPER-V 交换机端口必须具有应用的 ACL 规则。 此 ACL 可能是允许所有，DENY ALL 或更明确，以便仅允许某些类型的基于 5 元组 （源 IP、 目标 IP、 源端口、 目标端口、 协议） 匹配的流量。  
  
> [!NOTE]  
> HYPER-V 交换机扩展不会使用 HNVv2 中新的软件定义网络 (SDN) 堆栈。 使用 Azure 虚拟筛选平台 (VFP) 交换机扩展不能与任何其他第三方交换机扩展结合使用来实现 HNVv2。  
  
## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>切换和中的 HYPER-V 网络虚拟化的路由  
HNVv2 实现正确第 2 层 (L2) 切换和第 3 层 (L3) 路由语义，若要运行就像物理交换机或路由器会起作用。 当虚拟机连接到 HNV 虚拟网络将尝试建立位于同一虚拟子网 (VSID) 它首先需要了解远程虚拟机的 CA MAC 地址与另一个虚拟机。 如果没有源虚拟机的 ARP 表中的目标虚拟机的 IP 地址的 ARP 条目，则使用此条目中的 MAC 地址。 如果条目不存在，会将源虚拟机发送 ARP 广播与要返回到目标虚拟机的 IP 地址对应的 MAC 地址的请求。 HYPER-V 交换机将截取此请求，并将其发送到主机代理。 主机代理将在其本地数据库进行相应的 MAC 地址的请求的目标虚拟机的 IP 地址查找。  
  
> [!NOTE]  
> 主机代理，充当 OVSDB 服务器使用 VTEP 架构变的体来存储 CA-PA 映射、 MAC 表等。  
  
如果 MAC 地址不可用，主机代理注入 ARP 响应，并将发送此信息反馈到虚拟机。 虚拟机的网络堆栈包含所有必需的 L2 标头的信息后，帧发送到相应的 HYPER-V 端口上 V 开关。 在内部，HYPER-V 交换机测试此框架 N 元组匹配规则分配给 V 端口并适用于基于这些规则的帧的某些转换。 最重要的是一组封装转换应用来构造封装标头使用 NVGRE 或 VXLAN，具体取决于网络控制器级别定义的策略。 根据由主机代理编程的策略，CA-PA 映射用于确定的 HYPER-V 主机的 IP 地址为目标虚拟机所在的位置。 HYPER-V 交换机可确保正确路由规则和 VLAN 标记应用到外部数据包使其到达远程的 PA 地址。  
  
如果要创建具有不同虚拟子网 (VSID) 中的虚拟机的连接连接到 HNV 虚拟网络的虚拟机，需要相应地路由数据包。 HNV 假定星形拓扑使用为下一步跃点来访问所有 IP 前缀 （含义一个默认路由/网关） 在 CA 空间中没有只有一个 IP 地址。 目前，这会强制限制到单个默认路由，不支持非默认路由。  
  
### <a name="routing-between-virtual-subnets"></a>虚拟子网之间的路由  
在物理网络中，IP 子网是第 2 层 (L2) 域的计算机 （虚拟和物理） 可以相互直接通信。 L2 域是一个广播的域其中 ARP 条目 (IP:MAC 地址 map) 学习通过广播所有接口的 ARP 请求和 ARP 响应发回给发出请求的主机。 计算机使用 ARP 响应从中学到的 MAC 信息完全构造包括以太网标头的 L2 帧。 但是，如果在不同的 L3 子网的 IP 地址，ARP 请求不会跨越此 L3 边界。 相反，源子网中的 IP 地址的 L3 路由器接口 （下一步跃点或默认网关） 必须响应这些 ARP 请求具有其自己的 MAC 地址。  
  
在标准 Windows 网络中，管理员可以创建静态路由，并将其分配给网络接口。 此外，"默认网关"通常被配置为默认路由 (0.0.0.0/0) 的数据包发送到何处的接口上的下一步跃点 IP 地址。 如果没有特定的路由存在，将数据包发送到此默认网关。 通常，此网关就是物理网络的路由器。  HNV 使用内置路由器，每个主机的一部分并且可以在每个 VSID 创建虚拟网络的分布式的路由器的接口。  
  
因为 HNV 会假定说的星形拓扑，HNV 分布式的路由器充当一个默认网关的将是相同的 VSID 网络的一部分的虚拟子网之间的所有流量。 地址用作默认网关默认为中的 VSID 的最低 IP 地址，并分配给 HNV 分布式路由器。 此分布式的路由器允许以 VSID 网络中的所有流量非常高效的方式将相应地路由，因为每个主机可以将流量直接路由到相应的主机无需中介。  当 VM 网络相同但虚拟子网不同的两台虚拟机位于同一台物理主机上时，尤其能够看到这种优势。  本节稍后你将会看到，数据包永远不用离开物理主机。  
  
### <a name="routing-between-pa-subnets"></a>PA 子网之间路由  
与分配的每个虚拟子网 (VSID) 的一个 PA IP 地址的 HNVv1，相比 HNVv2 现在使用 Switch-Embedded 组合 (SET) NIC 团队成员每一个 PA IP 地址。 默认部署假定两个 nic，并将每个主机的两个 PA IP 地址分配。 一台主机已从同一个提供程序 (PA) 逻辑上的子网所在的同一 VLAN 已分配的 PA Ip。 在同一虚拟子网中的两个租户 Vm 确实可能位于两个不同的主机连接到两个不同的提供程序逻辑的子网。 HNV 将构造封装基于 CA-PA 映射的数据的外部 IP 标头。 但是，它依赖于主机 TCP/IP 转交给 ARP 堆栈默认 PA 网关，然后生成基于 ARP 响应外部的以太网标头。 通常情况下，此 ARP 响应来自 SVI 接口 L3 路由器的物理交换机上该主机已连接。 HNV 因此依赖于路由封装的数据包提供程序逻辑的子网之间的 L3 路由器 / Vlan。  
  
### <a name="routing-outside-a-virtual-network"></a>虚拟网络外部的路由  
大多数客户部署将需要 HNV 环境与不属于 HNV 环境的资源进行通信。 这就需要网络虚拟化网关来实现两种环境之间的通信。 需要 HNV 网关的基础结构包括私有云以及混合云。 基本上，HNV 网关所需的第 3 层路由 （包括 NAT） 的内部和外部 （物理） 网络之间或不同的站点和/或云 （私有或公共） 使用的 IPSec VPN 或 GRE 隧道之间。  
  
网关可以有不同的实体构成元素。 可以基于 Windows Server 2016 中，合并到机架顶部 (TOR) 交换机充当 VXLAN 网关，访问通过虚拟 IP (VIP) 所播发的负载均衡器，放入其他现有的网络设备，也可以是一个新的独立网络设备.  
  
有关 Windows RAS 网关选项的详细信息，请参阅[RAS 网关](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md)。  
  
## <a name="packet-encapsulation"></a>数据包封装  
在 HNV 中的每个虚拟网络适配器都与两个 IP 地址相关：  
  
-   **客户地址**(CA) 的 IP 地址分配由客户基于其 intranet 基础结构。 该地址允许客户与虚拟机交换网络流量，如同该虚拟机尚未移至私有云或公有云一样。 CA 对虚拟机是可见的，并且可由客户访问。  
  
-   **提供程序地址**(PA) 的 IP 地址分配的托管提供商或数据中心管理员基于其物理网络基础结构。 PA 出现在网络上的数据包中，这些数据包可与托管虚拟机的、运行 Hyper-V 的服务器进行交换。 PA 在物理网络上是可见的，但在虚拟机上不可见。  
  
CA 维护着客户的网络拓扑，该网络拓扑经过虚拟化，可采用 PA 的实施方式，从实际的基础物理网络拓扑中脱离出来。 下图展示了因网络虚拟化而在虚拟机 CA 与网络基础设施 PA 之间所建立的概念关系。  
  
![物理基础结构的网络虚拟化概念图](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)  
  
图 6：物理基础结构的网络虚拟化概念图  
  
在图中，客户虚拟机发送数据包在 CA 空间中，遍布物理网络基础结构通过其自己的虚拟网络或"隧道"。 在上面的示例中，在隧道可以认为的"封套"带有绿色发货标签 （PA 地址） 从左侧源主机发送到目标主机在右侧的 Contoso 和 Fabrikam 数据包。 关键在于主机如何确定"送货地址"(PA) 对应于 Contoso 和 Fabrikam CA、 如何围绕数据包，将"信封"和目标主机如何解包数据包和将传送到 Contoso 和 Fabrikam目标虚拟机正确。  
  
这一简单的类比突显出网络虚拟化的重要方面：  
  
-   将每台虚拟机的 CA 映射到物理主机的 PA。 同一个 PA 可能有多个关联的 CA。  
  
-   虚拟机在 CA 空间，将放入"信封"PA 源和目标对映射中发送的数据包。  
  
-   CA-PA 映射必须允许主机为不同的客户虚拟机区分数据包。  
  
因此，网络虚拟化机制旨在将虚拟机使用的网络地址虚拟化。 网络控制器负责该地址的映射，并且主机代理可以维护使用 MS_VTEP 架构映射数据库。 下一部分将描述地址虚拟化的实际机制。  
  
## <a name="network-virtualization-through-address-virtualization"></a>通过地址虚拟化实现网络虚拟化  
HNV 实现覆盖租户网络使用网络虚拟化通用路由封装 (NVGRE) 或虚拟可扩展局域网 (VXLAN)。  默认值为 VXLAN。  
  
### <a name="virtual-extensible-local-area-network-vxlan"></a>虚拟可扩展局域网 (VXLAN)  
虚拟可扩展局域网 (VXLAN) ([RFC 7348](https://www.rfc-editor.org/info/rfc7348)) 协议已在利用等 Cisco、 Brocade、 Arista、 Dell、 HP 和其他供应商支持的市场中广泛采用。 VXLAN 协议使用 UDP 作为传输方式。 VXLAN IANA 分配的 UDP 目标端口是 4789 和 UDP 源端口应为来自内部的数据包，以用于 ECMP 分布的信息的哈希值。 UDP 标头之后, VXLAN 标头追加到的数据包，其中包括后跟的 3 字节字段为 VXLAN 网络标识符 (VNI)-VSID 的另一个保留 1 字节的字段后, 跟一个保留的 4 字节字段。 VXLAN 标头之后, 会追加 （不带 CA 以太网帧 FCS） 原始 CA L2 帧。  
  
![VXLAN 数据包标头](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)  
  
### <a name="generic-routing-encapsulation-nvgre"></a>通用路由封装 (NVGRE)  
此网络虚拟化机制将基本路由封装 (NVGRE) 用作通道报头的一部分。 在 NVGRE 中，虚拟机的数据包被封装在另一个数据包。 如图 7 所示，新的数据包报头含有合适的源和目标 PA IP 地址，另外还有存储在 GRE 报头密钥字段中的虚拟子网 ID。  
  
![NVGRE 封装](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)  
  
图 7：网络虚拟化 - NVGRE 封装  
  
虚拟子网 ID 允许主机以确定任何指定的数据包客户虚拟机，即使的 PA 和 CA 的数据包可能会重叠。 这可让同一台主机上的所有虚拟机分享一个 PA（如图 7 所示）。  
  
共享 PA 对网络可扩展性产生很大的影响。 网络基础设施必须知悉的 IP 和 MAC 地址数量得以大幅减少。 例如，如果每台终端主机平均有 30 台虚拟机，网络基础设施需要知悉的 IP 和 MAC 地址数量将减少到三十分之一，数据包中嵌入式虚拟子网 ID 还能轻易地将数据包与实际客户联系起来。  
  
对于 Windows Server 2012 R2 共享方案的 PA 是每个主机的每个 VSID 一个 PA。 Windows Server 2016 的方案为每个 NIC 团队成员的一个 PA。  
  
Windows Server 2016 和更高版本，HNV 完全支持 NVGRE 和 VXLAN 现成的;它不需要升级或购买新的网络硬件，如 Nic （网络适配器）、 交换机或路由器。 这是因为这些数据在网络上的是 PA 空间，这是符合当今的网络基础结构中的常规 IP 包。  但是，若要获得最佳性能，请使用支持任务卸载了最新驱动程序支持的 Nic。  
  
## <a name="multi-tenant-deployment-example"></a>多租户部署示例  
下图显示了与 CA-PA 关系由网络策略定义位于云数据中心中的两个客户的示例部署。  
  
![多租户部署示例](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)  
  
图 8：多租户部署示例  
  
思考图 8 中的示例。 在转到托管提供商的共享 IaaS 服务之前：  
  
-   Contoso 公司运行的是一台 SQL Server（名为 **SQL**），IP 地址为 10.1.1.11，以及一台 Web 服务器（名为 **Web**）IP 地址为 10.1.1.12，并用其 SQL Server 处理数据库事务。  
  
-   Fabrikam 公司运行的是一台 SQL Server（也名为 **SQL** ），IP 地址为 10.1.1.11，以及一台 Web 服务器（也名为 **Web** ），IP 地址为 10.1.1.12，并用其 SQL Server 处理数据库事务。  
  
我们将假定托管服务提供商以前创建了通过网络控制器可以对应于其物理网络拓扑的提供程序 (PA) 逻辑网络。 网络控制器从主机相连的逻辑子网的 IP 前缀分配两个 PA IP 地址。 网络控制器还指示相应的 VLAN 标记，要应用的 IP 地址。  
  
使用网络控制器，Contoso 公司和 Fabrikam 公司，则创建其虚拟网络和子网支持的托管服务提供商指定的提供程序 (PA) 逻辑网络。 Contoso 公司和 Fabrikam 公司将其各自的 SQL Server 和 Web 服务器移至同一托管提供商的共享 IaaS 服务，而巧合的是，这些服务器在 Hyper-V 主机 1 中运行 **SQL** 虚拟机，在 Hyper-V 主机 2 中运行 **Web** (IIS7) 虚拟机。 所有虚拟机都保持其原有的内部网 IP 地址（其 CA）。  
  
这两个公司都指定以下的虚拟子网 ID (VSID)，由网络控制器，如下所示。  每个 HYPER-V 主机上的主机代理从网络控制器接收分配的 PA IP 地址，并在非默认网络隔离舱中创建两个 PA 主机 Vnic。 网络接口分配给每个这些主机 Vnic PA IP 地址的分配，如下所示：  
  
-   Contoso 公司虚拟机 VSID 以及 Pa:**VSID**为 5001， **SQL PA**为 192.168.1.10， **Web PA**为 192.168.2.20  
  
-   Fabrikam 公司虚拟机 VSID 以及 Pa:**VSID**为 6001， **SQL PA**为 192.168.1.10， **Web PA**为 192.168.2.20  
  
网络控制器检测到 SDN 主机代理将保留在持久性存储 （在 OVSDB 数据库表） 的策略 （包括 CA-PA 映射） 的所有网络策略。  
  
在 HYPER-V 主机 2 上的 Contoso 公司 Web 虚拟机 (10.1.1.12) 处 10.1.1.11 创建到 SQL Server 的 TCP 连接，发生以下情况：  
  
-   10.1.1.11 的目标 MAC 地址的 VM Arp  
  
-   VFP 扩展 vSwitch 中的截获此数据包，并将其发送到 SDN 主机代理  
  
-   在 MAC 地址为 10.1.1.11 其策略存储区中查找 SDN 主机代理  
  
-   如果找到 MAC，则主机代理注入 ARP 响应发回 VM  
  
-   如果找不到 MAC，不会发送响应和 10.1.1.11 在 VM 中的 ARP 条目标记为无法访问。  
  
-   VM 现在构造正确的 CA 以太网和 IP 标头的 TCP 数据包并将其发送到 vSwitch  
  
-   VFP 转发扩展 vSwitch 中的处理此数据包通过分配给源 vSwitch 端口，该数据包已收到并 VFP 统一的流表中创建一个新的流条目 VFP 层 （如下所述）  
  
-   VFP 引擎上的 IP 和以太网标头基于每个层 （例如虚拟网络层） 执行规则匹配的或流表查找。  
  
-   虚拟网络层中的匹配的规则引用 CA-PA 映射空间，并执行封装。  
  
-   VSID 以及 VNet 层中指定封装类型 （VXLAN 或 NVGRE）。  
  
-   在 VXLAN 封装的情况下使用 5001 VXLAN 标头中的 VSID 构造外部的 UDP 标头。  
    使用分配给 HYPER-V 主机 2 (192.168.2.20) 和 HYPER-V 主机 1 (192.168.1.10) 分别基于 SDN 主机代理的策略存储区的源和目标 PA 地址构造的外部 IP 标头。  
  
-   然后，此数据包流动到 VFP 的 PA 路由层。  
  
-   VFP 的 PA 路由层将引用用于 PA 空间流量和 VLAN ID 的网络隔离舱，并使用主机的 TCP/IP 堆栈将正确转发到 HYPER-V 主机 1 PA 数据包。  
  
-   收到封装的数据，HYPER-V 主机 1 PA 网络隔离舱中接收的数据包，并将其转发到 vSwitch。  
  
-   VFP 处理通过其 VFP 层数据包并 VFP 统一的流表中创建一个新的流条目。  
  
-   VFP 引擎匹配中的虚拟网络层的 ingres 规则和剥离外部封装的数据包的以太网、 IP 和 VXLAN 标头。  
  
-   VFP 引擎然后将数据包转发到目标虚拟机连接到的 vSwitch 端口。  
  
Fabrikam 公司 **Web** 与 **SQL** 虚拟机之间用于通信的类似进程针对 Fabrikam 公司使用 HNV 策略设置。因此，在使用 HNV 时，Fabrikam 公司和 Contoso 公司虚拟机的交互就像在其原有的 Intranet 中进行一样。 尽管两家公司虚拟机使用的是相同的 IP 地址，也不会互相影响。  
  
不同的地址 （Ca 和 Pa）、 HYPER-V 主机的策略设置和 CA 之间的流量入站和出站虚拟机的 PA 地址转换将隔离服务器使用 NVGRE 密钥或 VLXAN VNID 这些的组。 同时，虚拟化映射以及转化将虚拟网络构架从物理网络基础结构中脱离出来。 尽管 Contoso **SQL** 和 **Web** 以及 Fabrikam **SQL** 和**Web** 位于其自己的 CA IP 子网 (10.1.1/24)，两者的物理部署在两台有着不同的 PA 子网（分别为 192.168.1/24 和 192.168.2/24）的主机上进行。 这意味着通过 HNV，可以进行跨子网虚拟机配置和实时迁移。  
  
## <a name="hyper-v-network-virtualization-architecture"></a>Hyper-V 网络虚拟化架构  
在 Windows Server 2016 中，使用 Azure 虚拟筛选平台 (VFP) 中的 HYPER-V 交换机的 NDIS 筛选扩展实现 HNVv2。 VFP 的关键概念在于，使用内部 API 公开给 SDN 主机代理进行编程网络策略匹配操作数据流引擎。 SDN 主机代理本身通过 OVSDB 和 WCF SouthBound 通信通道，从网络控制器接收网络策略。 不只是虚拟网络策略 (例如 CA-PA 映射) 进行编程使用 VFP 但 Acl、 QoS，等的其他策略。  
  
以下是 vSwitch 和 VFP 转发扩展的对象层次结构：  
  
-   vSwitch  
  
    -   外部 NIC 管理  
  
    -   NIC 硬件卸载功能  
  
    -   全局转发规则  
  
    -   端口  
  
        -   转发将十字线固定层出口  
  
        -   空间列出了用于映射和 NAT 池  
  
        -   统一的流表  
  
        -   VFP 层  
  
            -   流表  
  
            -   Group  
  
            -   规则  
  
                -   规则可以引用空格  
  
VFP，在一个层创建的每个策略类型 （例如，虚拟网络），是一组通用的规则/流表。 直到特定规则分配给该图层来实现此类功能，它不具有任何内部函数的功能。 每个层分配优先级和图层按升序排序优先级分配给一个端口。 规则已组织成组主要基于方向和 IP 地址系列。 组还分配优先级和一条规则从一组最多可以匹配给定的流。  
  
与 VFP 扩展 vSwitch 的转发逻辑如下所示：  
  
-   入口处理 （入口从传入端口的数据包的角度）  
  
-   转发  
  
-   出口处理 （出数据包在离开一个端口的角度）  
  
VFP NVGRE 和 VXLAN 封装类型以及外部基于 MAC VLAN 转发支持内部 MAC 转发。  
  
VFP 扩展具有慢速路径和数据包遍历的快速路径。 在流中的第一个数据包必须遍历每个层中的所有规则组和执行规则查找，这是代价高昂的操作。 但是，一旦流注册操作 （基于匹配的规则） 的列表在统一的流表中的所有后续数据包将处理基于统一的流表条目。  
  
主机代理进行编程，HNV 策略。 每个虚拟机网络适配器配置为 IPv4 地址。 这些是虚拟机用来相互交流的 CA，并置于虚拟机的 IP 包中。 HNV 封装基于主机代理的数据库中存储的网络虚拟化策略的 PA 帧中的 CA 帧。  
  
![HNV 体系结构](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)  
  
图 9：HNV 体系结构  
  
## <a name="summary"></a>总结  
基于云的数据中心能带来许多好处，如改善可扩展性和资源使用情况。 为了实现这些潜在的好处，需要能基本解决动态环境中多租户可扩展性问题的技术。 HNV 旨在通过分离物理网络拓扑的虚拟网络拓拟来解决这些问题，并提高数据中心的运营效率。 HNV 构建在现有标准，在当前的数据中心中运行并与现有 VXLAN 基础结构进行操作。 HNV 的客户可以将其数据中心现在整合到私有云或无缝扩展其数据中心的混合云的托管服务器提供程序的环境。  
  
## <a name="BKMK_LINKS"></a>另请参阅  
若要了解有关 HNVv2 的详细信息请参阅以下链接：  
  
|内容类型|参考|  
|----------------|--------------|  
|**社区资源**|-   [私有云体系结构博客](https://blogs.technet.com/b/privatecloud)<br />-提出问题： [cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-   [NVGRE Draft RFC](https://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN - RFC 7348](https://www.rfc-editor.org/info/rfc7348)|  
|**相关的技术**|-对于 Windows Server 2012 R2 中 HYPER-V 网络虚拟化技术详细信息，请参阅[HYPER-V 网络虚拟化技术细节](https://technet.microsoft.com/library/jj134174.aspx)<br />-   [网络控制器](../../../sdn/technologies/network-controller/Network-Controller.md)|  
  


