---
title: Windows Server 2016 hyper V 网络虚拟化技术详细信息
description: 本主题提供的有关 Windows Server 2016 中 HYPER-V 网络虚拟化技术的信息
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
ms.openlocfilehash: af2b2a0b151601124bb473c465e7d5a97f2b9150
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>Windows Server 2016 hyper V 网络虚拟化技术详细信息

>适用于：Windows Server 2016

服务器虚拟化使单个物理主机; 在同时运行多个 server 实例然而彼此隔离 server 实例。 每个虚拟机本质上是表示像是唯一物理计算机上运行的服务器。  
  
网络虚拟化提供类似的功能，以在相同 （可能与重叠 IP 地址） 的多个虚拟网络中运行物理网络基础结构和每个虚拟网络表示像是仅虚拟网络上共享的网络基础结构运行。 图 1 显示此关系。  
  
![与网络虚拟化服务器虚拟化](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)  
  
与网络虚拟化图 1： 服务器虚拟化  
  
## <a name="hyper-v-network-virtualization-concepts"></a>Hyper V 网络虚拟化概念  
在 HYPER-V 网络虚拟化 (HNV) 的客户或租户被定义为 IP 子网部署企业或数据中心中的一组"所有者"。 客户可以是 corporation 或具有多个部门或企业单位专用数据中心中的要求网络隔离或在公共数据中心服务提供商托管其租户企业。 每个客户可以有一个或多个[虚拟网络](#VirtualNetworks)数据中心中，在每个虚拟网络包含一项或多[虚拟子网](#VirtualSubnets)。  
  
有两个 HNV 实现这将适用于 Windows Server 2016: HNVv1 和 HNVv2。  
  
-   **HNVv1**  
  
    HNVv1 与 Windows Server 2012 R2 和系统中心 2012 R2 虚拟机管理器 (VMM) 兼容。 配置 HNVv1 依赖 WMI 管理和 （通过系统中心 VMM 主持） 的 Windows PowerShell cmdlet 隔离设置和客户地址 （加拿大）-虚拟网络的定义物理地址 （路径） 映射和路由到。 已添加到在 Windows Server 2016 HNVv1 任何其他功能，并计划任何新的功能。  
    
    • 设置团队合作，HNV V1 不兼容的平台。
    
    O 使用 HA NVGRE 网关用户需要为使用 LBFO 团队或没有团队。 或
    
    与一组 o 使用网络控制器部署网关搭配使用切换。

  
-   **HNVv2**  
  
    在 HNVv2 以实现使用 Azure 虚拟筛选平台 (VFP) 转发 HYPER-V 切换中的扩展包含大量新功能。 其中包括软件定义网络 (SDN) 堆栈中的新网络控制器的 Microsoft Azure 堆栈与完全集成 HNVv2。  通过 Microsoft 定义虚拟网络策略[网络控制器](../../../sdn/technologies/network-controller/Network-Controller.md)使用 rest 风格 NorthBound （注意） API 和检测到主机代理通过多个 SouthBound 接口 (SBI) 包括 OVSDB。 主机专员程序的实施了 HYPER-V 切换 VFP 扩展的策略。  
  
    > [!IMPORTANT]  
    > 本主题 HNVv2 重点介绍。  
  
### <a name="VirtualNetworks"></a>虚拟网络  
  
-   每个虚拟网络包含一个或多个虚拟个子网。 虚拟网络形成隔离边界了虚拟网络中的虚拟机仅可以与彼此通信。 过去，这种隔离了使用强制 Vlan 隔离的 IP 地址范围和 802.1 q 标记或 VLAN id。 但 HNV，与隔离可使用强制 NVGRE 或 VXLAN 封装重叠 IP 子网客户或租户之间的可能性创建覆盖网络。  
  
-   在主机上，每个虚拟网络具有唯一路由域 ID (RDID)。 此 RDID 大约为资源 ID 找出虚拟网络网络控制器中的其余部分资源地图。 虚拟网络其余资源引用了统一资源标识符 (URI) 命名空间使用附加资源 id。  
  
### <a name="VirtualSubnets"></a>虚拟子网  
  
-   虚拟子网实现位于同一虚拟子网虚拟机层 3 IP 子网语义。 虚拟子网形成广播的域 （类似于 VLAN） 并隔离强制使用 NVGRE 租户网络 ID (TNI) 或 VXLAN 网络标识符 (VNI) 字段。  
  
-   每个虚拟子网属于单个虚拟网络 (RDID)，并且不分配唯一虚拟子网 ID (VSID) 封装的数据包标题中使用 TNI 或 VNI 密钥。 VSID 必须数据中心中的唯一并且处于范围内 4096 至 2 ^24 2。  
  
虚拟网络并路由域的关键优势到云是它使客户能够将自己网络拓扑 （例如，IP 子网）。 图 2 显示的示例，其中 Contoso Corp 有两个独立的网络，R 和网络 D 和销售网络。 由于这些网络拥有不同路由域 Id，他们无法彼此之间进行交互。 Contoso R 和 D 网络即，独立于 Contoso 销售网络即使两所拥有的 Contoso Corp.Contoso R 和 D 网络包含三个虚拟个子网。 请注意的 RDID 和 VSID 数据中心中的唯一。  
  
![客户网络和虚拟子网](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)  
  
图 2： 客户网络并虚拟子网  
  
**第二层转移**  
  
图 2 VSID 5001 中的虚拟机可以让其数据包转发到还正在 VSID 5001 通过 HYPER-V 切换虚拟机。 从 VSID 5001 在虚拟机传入的数据包发送到特定 VPort 上 HYPER-V 切换。 通过这些数据包 HYPER-V 切换应用入口规则 （例如封闭） 和映射 （例如封装标题）。 将包转发不同 VPort 上 HYPER-V 切换到 （如果目标虚拟机已连接到同一台主机） 还是不同 HYPER-V 交换机 （如果目标虚拟机位于另一台主机上） 不同主机上。  
  
**第 3 路由**  
  
同样，VSID 5001 中的虚拟机可以让其数据包 HNV 分布式路由器出现在每个 HYPER-V 主机 VSwitch 路由到中 VSID 5002 或 VSID 5003 虚拟机。 在数据包传递到 HYPER-V 切换、 HNV 目标虚拟机的 VSID 到更新的传入的数据包 VSID。 如果这两个 VSIDs 相同 RDID 中，只会发生这种情况。  因此，RDID1 的虚拟网络适配器无法发送数据包 RDID2 的虚拟网络适配器不遍历网关。  
  
> [!NOTE]  
> 在上面的数据包流量描述，术语"虚拟机"实际上是指在虚拟机上的虚拟网络适配器。 一般情况下是虚拟机仅有一个虚拟网络适配器。 在此情况下，字词"虚拟机"和"虚拟网络适配器"可以概念意味着相同的操作。  
  
每个虚拟子网定义层 3 IP 子网和第二层 2 广播的域边界类似于 VLAN。 当虚拟机广播数据包时，HNV 使用单址广播复制 (UR) 使原始数据包一份和目的地 IP 和 MAC 替换为每个 VM 相同 VSID 中有哪些的地址。  
  
> [!NOTE]  
> 当 Windows Server 2016 发布时，将使用单址广播复制实现广播和子网多。 交叉网多路广播路由和 IGMP 不受支持。  
  
除了正在广播的域，VSID 提供隔离。 中 HNV 虚拟网络适配器已连接到 HYPER-V 切换端口 ACL 规则应用直接与端口 （virtualNetworkInterface 其余资源） 或到虚拟子网 (VSID) 它的一部分包含。  
  
HYPER-V 切换端口必须 ACL 规则应用。 此 ACL 可能是允许所有，所有拒绝，或者是更多具体仅允许某些类型的交通基于 5 元来源 IP、 目的地 IP、 源端口、 目的地端口 （协议） 匹配。  
  
> [!NOTE]  
> HYPER-V 切换扩展不起作用 HNVv2 使用新的软件定义网络 (SDN) 堆栈中。 使用 Azure 虚拟筛选平台 (VFP) 切换扩展它不能用于任何其他第三方切换扩展结合实现 HNVv2。  
  
## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>切换和 HYPER-V 网络虚拟化中路由  
HNVv2 实现正确的第二层 2 切换和 3 层 (L3) 路由语义一样物理开关工作或路由器不起作用。 当虚拟机连接到 HNV 虚拟网络将尝试重新建立位于同一虚拟子网 (VSID) 它将首先需要了解远程虚拟机的 CA MAC 地址与另一个虚拟机的连接。 如果 ARP 源虚拟机 ARP 表中的目标虚拟机的 IP 地址条目，使用中此项的 MAC 地址。 如果不存在条目，源虚拟机将发送 ARP 广播与相对于目的地虚拟机的 IP 地址的 MAC 地址返回的申请。 HYPER-V 开关将拦截此请求，并将其发送到主机专员。 主机代理看起来本地数据库中的相应的 MAC 地址请求的目标虚拟机的 IP 地址。  
  
> [!NOTE]  
> 主机人员，作为 OVSDB 服务器使用 VTEP 方案的变体存储加利福尼亚州映射、 MAC 表中，等等。  
  
如果有 MAC 地址，则主机代理插入 ARP 响应，并将发送到该虚拟机此功能重新。 虚拟机网络堆栈具有所有必需的 L2 标头信息后，该框架将发送给 V 开关中对应的 HYPER-V 端口中。 在内部，HYPER-V 切换测试 N 元匹配规则分配给 V 端口针对此框架和适用于框架根据这些规则的某些转换。 最重要的是一组封装转换应用以构建封装标头使用 NVGRE 或 VXLAN，具体取决于定义网络控制器上的策略。 根据编程方式设置主机代理机构的策略，CA 州映射用于确定 HYPER-V 主机的 IP 地址目标虚拟机所处的位置。 HYPER-V 切换确保正确的路由规则，并且使其达到远程州地址 VLAN 标记到外部数据包应用。  
  
如果连接到 HNV 虚拟网络虚拟机想要创建具有不同的虚拟子网 (VSID) 中的虚拟机的连接，将需要会路由相应地数据包。 HNV 假定星形拓扑在 CA 空间的跃用作联系所有 IP 前缀 （含义一个默认路线/关） 中只有一个 IP 地址。 当前，这将强制单个默认路线限制和非默认路线均不支持。  
  
### <a name="routing-between-virtual-subnets"></a>之间虚拟子网路由  
在物理的网络，IP 子网是在计算机 （虚拟和物理） 可以直接互相第二层 2 域。 L2 域是广播的域了条目 （IP:MAC 地址和地图） 的所有接口广播的 ARP 请求通过学习和 ARP 响应会发送回请求主机。 在计算机使用 MAC 信息从 ARP 响应学习了完全构建包括以太网标题 L2 帧。 但是，如果在不同的 L3 子网 IP 地址，ARP 请求不跨越此 L3 边界。 相反，在源子网 IP 地址 L3 路由器接口 （旁边跳跃或默认网关） 必须响应这些 ARP 有它自己的 MAC 地址。  
  
标准 Windows 网络，以管理员可以创建静态的路线并分配给网络接口这些。 此外，"默认网关"通常配置上的默认路线 (0.0.0.0/0) 数据包发送的位置接口的下一步跳跃 IP 地址。 如果不存在任何特定的路线为此默认网关发送数据包。 这通常是用于你的物理网络路由器。  HNV 使用内置路由器，为每台主机的一部分，并且具有中以创建一个分布式的路由器的虚拟网络的每个 VSID 界面。  
  
由于 HNV 假定星形拓扑，HNV 分布式的路由器将作为一个默认网关之间的同一 VSID 网络虚拟子网执行的所有通信。 该地址用作中 VSID 最低 IP 地址默认网关默认值，并且赋予 HNV 分布式路由器。 在 VSID 网络中的所有通信非常有效方法因为每台主机可以直接将通信路由到相应的主机而无需中介相应地路由允许此分布式的路由器。  在同一 VM 网络但不同的虚拟子网两个虚拟机相同的物理主机时，这是尤其如此。  当你将看到更高版本在此部分中，数据包从来都离开物理主机。  
  
### <a name="routing-between-pa-subnets"></a>路由州子网之间  
与 HNVv1 分配的每个虚拟子网 (VSID) 个州 IP 地址，HNVv2 现在使用个州 IP 地址，每个 NIC Switch-Embedded 组队 （集） 团队的成员。 默认部署假定两个 NIC 团队和分配每台主机的两个州 IP 地址。 一台主机具有对 IPs 从上的同一 VLAN 相同的提供商 （州） 逻辑网分配。 连接到两个不同提供程序逻辑子网这两种不同主机上可能确实位于同一虚拟子网两个租户虚拟机的功能。 HNV 将构建外部 IP 标题为封装数据包基于加利福尼亚州映射。 但是，，它将依赖于 ARP 默认州网关主机 TCP/IP 堆栈，然后生成根据 ARP 响应外部以太网标题。 通常情况下，此 ARP 响应来自 SVI 界面上的物理交换机或 L3 路由器主机连接的位置。 HNV 因此依赖路由封装提供商逻辑子网之间的数据包 L3 路由器 / Vlan。  
  
### <a name="routing-outside-a-virtual-network"></a>路由之外虚拟网络  
大部分客户部署需要通信 HNV 环境中并非 HNV 环境的一部分的资源。 必须网络虚拟化网关允许在两个环境之间进行通信。 需要 HNV 网关的基础结构包含专用云和混合云。 基本上，HNV 网关所需的一层 3 路由或其他站点和/或使用 IPSec VPN 或 GRE 隧道 （专用或公共） 云霞内部和外部 （物理） 网络 （包括 NAT） 之间。  
  
网关可能会不同物理外形规格。 他们可以基于 Windows Server 2016，并入机架 Top （或） 切换充当 VXLAN 网关、 访问通过虚拟 IP (VIP) 公布负载平衡，通过进入其他现有的网络设备，也可以是一种新的独立网络设备。  
  
有关 Windows RAS 网关选项的详细信息，请参阅[RAS 网关](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md)。  
  
## <a name="packet-encapsulation"></a>数据包封装  
在 HNV 每个虚拟网络适配器是与两个 IP 地址的方法：  
  
-   **客户地址**（加拿大） 的 IP 地址的客户，根据他们的 intranet 基础结构分配。 此地址允许客户就像这没有了移动到公共或专用的云交换网络与虚拟机的交通。 CA 正在到虚拟机可见，并且可客户。  
  
-   **提供商地址**承载提供程序分配的 （州） 的 IP 地址或 datacenter 管理员根据他们物理网络的基础结构。 对将出现在网络上的数据包交换运行 HYPER-V 虚拟机承载的服务器。 对是可见物理网络，但不是到该虚拟机。  
  
Ca 维护客户网络拓扑，虚拟化和实现 PAs 的分隔从实际基础物理网络拓扑和地址。 以下图表显示了虚拟机 Ca 和网络网络虚拟化定基础结构 PAs 之间的概念关系。  
  
![网络虚拟化轻物理基础结构的概念图示](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)  
  
图 6： 的网络虚拟化轻物理基础结构的概念图示  
  
在图，客户虚拟机发送数据数据包 CA 领域遍历通过其自己的虚拟网络，或者"隧道"物理网络基础结构。 在上方的示例中，隧道可以看作的周围 Contoso 和 Fabrikam 数据包使用绿色发货标签 （州地址） 从左侧源主机传送到目标主机右侧的"信封"。 该键主机如何确定"送货地址"（州） 对应于 Contoso 和 Fabrikam CA、 如何"信封"将围绕数据包，以及如何目标主机可以解数据包包和正确向 Contoso 和 Fabrikam 目的地的虚拟机提供。  
  
这种简单的对比突出显示网络虚拟化的要点：  
  
-   每个虚拟机 CA 映射到物理主机 PA. 可以与相同 PA.关联的多个 Ca  
  
-   虚拟机发送至"信封"具有州源代码和目的地对基于在地图上放置 CA 空格数据包。  
  
-   加利福尼亚州映射必须允许区分另一个客户虚拟机的数据包主机。  
  
因此，机制网络虚拟化是虚拟化使用虚拟机的网络地址。 负责地址的映射的网络控制器和主机代理维护使用 MS_VTEP 方案的地图数据库。 下一步部分介绍了地址虚拟化实际机制。  
  
## <a name="network-virtualization-through-address-virtualization"></a>通过地址虚拟网络虚拟化  
HNV 实现覆盖租户网络使用的网络虚拟化泛型路由封装 (NVGRE) 或虚拟可扩展本地网络 (VXLAN)。  VXLAN 是默认行为。  
  
### <a name="virtual-extensible-local-area-network-vxlan"></a>虚拟可扩展本地网络 (VXLAN)  
虚拟可扩展本地网络 (VXLAN) ([RFC 7348](http://www.rfc-editor.org/info/rfc7348)) 协议已在市场的位置，如 Cisco、 Brocade、 Arista、 Dell、 HP 和其他人供应商提供的支持广泛采用。 VXLAN 协议使用作为传输 UDP。 VXLAN IANA 分配 UDP 目的地端口 4789 并且 UDP 源端口应哈希代码从内部包用于 ECMP 分配的信息。 UDP 标头之后, VXLAN 标题附有数据包，其中包括跟 3 字节字段为 VXLAN 网络标识符 (VNI)-VSID-另一个保留 1 字节字段后跟一个保留的 4 字节字段。 VXLAN 标头之后, 将被添加原始 CA L2 框架 （不带 CA 以太网帧 FCS 中）。  
  
![VXLAN 数据包标题](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)  
  
### <a name="generic-routing-encapsulation-nvgre"></a>一般路由封装 (NVGRE)  
该网络虚拟化机制作为隧道标题的一部分使用通用传送封装 (NVGRE)。 在 NVGRE 内另一台数据包, 封装虚拟机数据包。 此新数据包标题具有适当的源代码和除了虚拟子网 ID，如下所示图 7 中，GRE 标头，键字段中存储的目标州 IP 地址。  
  
![NVGRE 封装](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)  
  
图 7： 网络虚拟化-NVGRE 封装  
  
虚拟子网 ID 允许识别任何给定数据包客户虚拟机主机，即使路径和 CA 的数据包上可能会重叠。 这允许所有虚拟机共享单个的路径，同一台主机上为 7 图所示。  
  
共享州上网络可扩展性有很大的影响。 可以已经提供了可观减少数 IP 和 MAC 需要获知通过网络基础结构的地址。 例如，如果每结束主机 30 虚拟机，IP 大量平均并且需要获知由网络的基础结构的 MAC 地址减少 30.embedded 数据包中的虚拟子网 Id 倍还启用轻松关联的数据包到实际的客户。  
  
对 Windows Server 2012 R2 的共享方案为每台主机 VSID 每个州。 对于 Windows Server 2016 方案是一对每个 NIC 团队的成员。  
  
Windows Server 2016 和更高版本，HNV 完全支持 NVGRE 和 VXLAN 开箱;它不需要升级或购买新的网络硬件，例如 Nic （的网络适配器）、 开关或路由器。 这是因为路上这些数据包常规 IP 数据包为使用当今的网络的基础结构兼容的路径空间中。  但是，若要获取最佳性能使用受支持 Nic 支持任务卸载最新驱动程序。  
  
## <a name="multi-tenant-deployment-example"></a>多租户部署示例  
下图显示两个位于云数据中心网络策略的定义加利福尼亚州关系的客户的部署示例。  
  
![多租户部署示例](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)  
  
图 8 部分： 多租户部署示例  
  
图 8 中的示例中，请考虑。 事先到移至宿主的提供商的共享 IaaS 服务：  
  
-   Contoso Corp 运行 SQL Server (名为**SQL**) 在 IP 地址 10.1.1.11 和 web 服务器 (名为**Web**) 其 SQL Server 用于数据库交易的 IP 地址 10.1.1.12。  
  
-   Fabrikam Corp 运行 SQL Server，还名为**SQL**和指定 IP 地址 10.1.1.11 和 web 服务器，也称为**Web**和还 10.1.1.12 IP 地址，使用其 SQL Server 数据库交易记录。  
  
我们将假定宿主的服务提供商之前创建了提供商 （州） 逻辑网络通过网络控制器要与他们物理网络拓扑相对应。 网络控制器分配从主机相连的逻辑子网 IP 前缀两个州 IP 地址。 网络控制器也表示相应的 VLAN 标记，以将应用的 IP 地址。  
  
然后使用网络控制器、 Contoso Corp 和 Fabrikam Corp 创建其虚拟网络和子网其指定宿主的服务提供商的提供商 （路径） 逻辑网络提供支持。 Contoso Corp 和 Fabrikam Corp 移动他们各自的 SQL Server 和至相同宿主提供商的 web 服务器共享马上地点、 它们运行的 IaaS 服务**SQL**虚拟机上 HYPER-V 主机 1 和**Web** HYPER-V 主机 2 平板电脑 (IIS7) 虚拟机。 所有虚拟机都维护其原始的 intranet IP 地址 (其 Ca)。  
  
这两个公司都下面的虚拟子网 ID (VSID) 分配通过网络控制器，如下所示。  在每个 HYPER-V 主机上主机代理从网络控制器接收分配的路径 IP 地址，并在非默认网络盒创建两个州主机 vNICs。 每次如下所示的路径 IP 地址的分配位置这些主机 vNICs 分配网络接口：  
  
-   Contoso Corp 虚拟机 VSID 和 PAs: **VSID**是 5001， **SQL 路径**是 192.168.1.10， **Web 路径**是 192.168.2.20  
  
-   Fabrikam Corp 虚拟机 VSID 和 PAs: **VSID**是 6001， **SQL 路径**是 192.168.1.10， **Web 路径**是 192.168.2.20  
  
网络控制器检测到 SDN 主机代理将要维护的策略 （在 OVSDB 数据库表格） 持久存储中 （包括映射加利福尼亚州） 的所有网络策略。  
  
当 Contoso Corp Web 虚拟机 (10.1.1.12) HYPER-V 主机 2 上创建的 TCP 连接到 SQL Server 10.1.1.11 在时，会发生下列情况：  
  
-   VM Arp 10.1.1.11 目标 MAC 地址  
  
-   在该 vSwitch VFP 扩展拦截此数据包，并将其发送到 SDN 主机代理  
  
-   在 10.1.1.11 的 MAC 地址其策略应用商店中进行查找 SDN 主机代理  
  
-   如果找到 MAC，则主机代理插入返回到 VM ARP 响应  
  
-   如果找不到 MAC，不会发送响应，并为 10.1.1.11 VM 中的 ARP 项目标记无法访问。  
  
-   VM 现在构造 TCP 数据包具有正确的 CA 以太网和 IP 标题，并将其发送到该 vSwitch  
  
-   转发在该 vSwitch 的扩展 VFP 进程分配给源 vSwitch 端口数据包接听和 VFP 统一的流表中创建一个新的流量条目 VFP 层 （如下所述） 通过此包  
  
-   VFP 引擎执行规则匹配或流表查找 IP 和以太网标题基于每一层 （例如虚拟网络层）。  
  
-   匹配的规则虚拟网络层引用 CA 州映射空间，并执行封装。  
  
-   封装类型 （VXLAN 或 NVGRE） 以及 VSID VNet 层中指定。  
  
-   在 VXLAN 封装的情况下使用 5001 VXLAN 标题中的 VSID 构造外部 UDP 标头。  
    外部 IP 头构建的源代码和目的地的路径地址分配给 HYPER-V 主机 2 (192.168.2.20) 和 HYPER-V 主机 1 (192.168.1.10) 分别基于 SDN 主机代理策略应用商店。  
  
-   此包然后到 VFP 路径路由一层发展。  
  
-   VFP 路径路由一层将参考用于州空间路况和 VLAN ID 网络盒并使用的主机 TCP/IP 堆栈正确将州数据包转发给 HYPER-V 主机 1。  
  
-   在收到封装数据包，HYPER-V 主机 1 在对网络盒收到包和转发给该 vSwitch。  
  
-   VFP 处理通过它 VFP 层数据包，并在 VFP 统一的流表中创建一个新的流量条目。  
  
-   VFP 引擎匹配虚拟网络层 ingres 规则，并且去除外部封装的数据包以太网、 IP 和 VXLAN 标题。  
  
-   然后，VFP 引擎转发到目标 VM 连接到该 vSwitch 端口数据包。  
  
之间 Fabrikam Corp 交通类似的过程**Web**和**SQL**虚拟机的 Fabrikam Corp.使用 HNV 策略设置因此，HNV，Fabrikam Corp 和 Contoso Corp 虚拟机交互如同他们在其原始 intranet。 他们可以从未彼此之间进行交互，即使他们使用同一 IP 地址。  
  
（Ca 和 PAs） 的分隔地址、 HYPER-V 主机的策略设置和 CA 和传入和传出虚拟机交通州之间的地址转换找出这些套使用 NVGRE 键或 VLXAN VNID 的服务器。 此外，虚拟化映射和转变将分离从物理网络基础结构的虚拟网络体系结构。 尽管 Contoso **SQL**和**Web**和 Fabrikam **SQL**和**Web**驻留在其自身 CA IP 子网 (10.1.1/24)、 他们物理部署分别发生在不同的路径子网、 192.168.1/24 和 192.168.2/24，两台主机上。 这意味着是交叉网虚拟机预配和实时迁移变得可能具有 HNV。  
  
## <a name="hyper-v-network-virtualization-architecture"></a>HYPER-V 网络虚拟化建筑  
在 Windows Server 2016 中，使用 Azure 虚拟筛选平台 (VFP) 是 HYPER-V 切换内的 NDIS 筛选扩展实现 HNVv2。 关键 VFP 概念，具有对 SDN 主机代理编程网络策略的公开内部 API 匹配项操作流引擎。 SDN 主机代理本身通过 OVSDB 和 WCF SouthBound 的信道，从网络控制器接收网络策略。 不只是虚拟网络策略 (例如地图加利福尼亚州) 编程方式设置使用 VFP 但是其他的策略，如 Acl、 QoS，等等。  
  
VSwitch 和转发扩展 VFP 对象层次是以下方法：  
  
-   vSwitch  
  
    -   外部 NIC 管理  
  
    -   NIC 硬件减轻  
  
    -   全球转发规则  
  
    -   端口  
  
        -   出口转发层用于头发固定  
  
        -   空间列出映射和 NAT 池  
  
        -   统一的流表  
  
        -   VFP 层  
  
            -   流表  
  
            -   组  
  
            -   规则  
  
                -   规则引用空间  
  
VFP，层创建的每个策略类型 （例如，虚拟网络），并是规则/流表格的通用集合。 没有任何内部功能直到特定规则归入该层，以实现此类功能。 每一层分配优先级，并由升序优先级层分配到某个端口。 规则分为主要基于方向和 IP 地址家庭组。 此外会组分配优先级搭配，最多，组中的一个规则可以给定的流量。  
  
使用扩展 VFP 该 vSwitch 的转移逻辑如下所示：  
  
-   进入处理 （从数据包进入端口角度入口）  
  
-   转移  
  
-   出口处理 （从离开端口数据包角度出口）  
  
VFP 为 NVGRE 和 VXLAN 封装类型外部 MAC VLAN 基于转移支持密友 MAC 转移。  
  
VFP 扩展具有 slow 路径和数据包遍历 fast 路径。 流中的第一个数据包必须遍历每一层中的所有规则组并执行规则查找即便宜的操作。 但是，统一的流表的操作 （基于匹配规则） 列表中注册流程后后续数据包将处理基于统一的流表条目。  
  
HNV 策略是编程方式设置主机代理机构。 IPv4 地址配置虚拟机的每个网络适配器。 以下是虚拟机将用于通信相互，Ca 和中 IP 包虚拟机从携带他们。 HNV 封装 CA 帧州基于存储在主机代理数据库中的网络虚拟化策略。  
  
![HNV 建筑](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)  
  
图 9: HNV 建筑  
  
## <a name="summary"></a>摘要  
基于云的数据中心可以提供了许多的权益，如改进可扩展性和更好的资源使用率。 要实现这些潜在的好处，需要一种技术，从根本上说解决了在动态环境中的多租户可扩展性的问题。 HNV 设计解决这些问题，还通过分离物理网络拓扑的虚拟网络拓扑改进 datacenter 运营效率。 现有标准上生成，HNV 在当今的数据中心中运行，而与你的现有 VXLAN 基础结构进行操作。 使用 HNV 的客户可以现在将他们的数据中心合并到专用云或无缝扩展到宿主 server 提供商环境混合云与他们的数据中心。  
  
## <a name="BKMK_LINKS"></a>请参阅  
若要了解有关 HNVv2 的详细信息，请查看下面的链接：  
  
|键入的内容|引用|  
|----------------|--------------|  
|**社区资源**|-   [专用云体系结构博客](http://blogs.technet.com/b/privatecloud)<br />-提问：[cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-   [NVGRE 草稿 RFC](http://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN-RFC 7348](http://www.rfc-editor.org/info/rfc7348)|  
|**相关的技术**|-在 Windows Server 2012 R2 的 HYPER-V 网络虚拟化技术详细信息，请参阅[HYPER-V 网络虚拟化技术的详细信息](https://technet.microsoft.com/library/jj134174.aspx)<br />-   [网络控制器](../../../sdn/technologies/network-controller/Network-Controller.md)|  
  


