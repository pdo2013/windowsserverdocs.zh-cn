---
title: 套餐软件定义的网络的基础结构
description: 本主题提供有关如何计划软件定义网络 (SDN) 基础结构部署的信息。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ea7e53c8-11ec-410b-b287-897c7aaafb13
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bb3b9313996637fa5ee7367c538fe04d7cbefea9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="plan-a-software-defined-network-infrastructure"></a>套餐软件定义的网络的基础结构

>适用于：Windows Server（半年通道），Windows Server 2016

查看以下信息，以帮助规划软件定义网络 (SDN) 基础结构部署。 查看此信息后，请参阅[部署软件定义的网络基础结构](../deploy/Deploy-a-Software-Defined-Network-Infrastructure.md)有关部署的信息。

>[!NOTE]
>本主题中，除了以下 SDN 计划内容才可用。  
>
> - [安装和部署网络控制器的准备工作要求](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  

有关信息关于 Hyper-V 网络虚拟化 (HNV)，你可以使用虚拟化 Microsoft SDN 部署中的网络，请参阅[Hyper-V 网络虚拟化](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)。  

## <a name="prerequisites"></a>先决条件
本主题介绍了大量硬件和软件先决条件，包括：

-   **物理网络**  
    你需要访问你的物理网络设备使用 RDMA 技术，如果配置 Vlan、路由、BGP、数据中心桥 (ETS) 和数据中心桥 (PFC) 如果使用的 RoCE 基于 RDMA 技术。 本主题介绍开关手动配置以及对等 BGP 上层 3 开关 / 路由器或路由和远程访问之上虚拟机。   

-   **物理计算主机**  
这些主机运行 Hyper-V，并且需要到主机 SDN 基础结构和租户虚拟机。  为了获得最佳性能，述后面在这些主机才能特定网络硬件**网络硬件**部分。  
      
  
## <a name="physical-network-configuration"></a>物理网络配置

每个物理计算主机需要一个或多个与物理开关端口连接的网络适配器通过网络连接。 （可选）通过二层备份的多个网络逻辑段隔离是网络[VLAN](https://en.wikipedia.org/wiki/Virtual_LAN)。 IP 子网前缀 VLAN Id 下方显示的示例包括和必须自定义为你根据从你的网络管理员指南环境。 如果有任何你逻辑网络标签或访问型 VLAN ID 0 这些网络配置时使用逻辑子网 System Center 虚拟机 Manager 或 PowerShell 脚本配置文件中。

>[!IMPORTANT]
>Windows Server 2016 软件定义网络支持 IPv4 地址包含和覆盖层。 IPv6 不受支持。
  
### <a name="management-and-hnv-provider-logical-networks"></a>管理和 HNV 提供商的逻辑网络

所有物理都计算主机需有权访问逻辑管理网络和 HNV 提供商逻辑网络。 如果逻辑网络使用 Vlan，必须连接到中继切换端口，其中有权访问这些 Vlan 物理计算主机。 同样，在计算主机上的物理网络适配器不必须激活任何 VLAN 筛选。 如果你正在使用 Switch-Embedded 组队（集），并且在计算主机具有多个 NIC 团队成员（即网络适配器），您必须连接到同一个第二层广播域该特定主机 NIC 团队成员所有。  
  
计划目的的 IP 地址，每个物理计算主机必须从管理逻辑网络分配至少一个 IP 地址。 网络控制器会自动将两个 IP 地址分配从 HNV 提供商逻辑网络。 如果物理计算主机运行的其他基础结构虚拟机（例如，网络控制器、SLB/输入混合或关）该主机必须为每台基础结构虚拟机托管分配从管理逻辑网络其他 IP 地址。   
  
此外，每个月输入混合 SLB 的基础结构虚拟机必须 HNV 提供商逻辑网络从保留的 IP 地址。 

>[!IMPORTANT]
>必须从之外 HNV 提供商逻辑网络配置 IP 地址池分配这些 SLB/输入混合 IP 地址。 但无法执行此操作可能导致重复的网络上的 IP 地址。 

网络控制器要求管理网络作为其余 IP 地址从保留的地址。 你必须手动创建主机 A 记录 DNS 中的其余部分的 IP 地址。  
  
DHCP 服务器可以自动分配管理网络的 IP 地址，也可以手动分配的静态 IP 地址。 SDN 堆栈自动个别 Hyper-V 主机从指定通过和由网络控制器 IP 池分配 HNV 提供商网络的 IP 地址。   
  
结构管理员静态分配供 SLB/通过 PowerShell 脚本输入混合或 VMM HNV 提供商 IP 地址。 网络控制器 HNV 提供商 IP 地址物理计算主机只会后面分配给网络控制器主机代理接收网络策略特定租户虚拟机。  
  
#### <a name="sample-network-topology"></a>示例网络拓扑

自定义子网前缀 VLAN Id，根据你的网络管理员指南网关 IP 地址。  
  
网络名称|子网|面具|在主干 VLAN ID|网关|预订<br />（示例）  
----------------|----------|--------|--------------------|-----------|-----------------------------  
|**管理**|10.184.108.0|24|7|10.184.108.1|10.184.108.1-路由器<br /><br />10.184.108.4-网络控制器<br /><br />10.184.108.10-计算主机 1<br /><br />10.184.108.11-计算主机 2<br /><br />10.184.108.X-计算主机 X  
|**HNV 提供程序**|10.10.56.0|23|11|10.10.56.1|10.10.56.1-路由器<br /><br />10.10.56.2-SLB/MUX1  
  
### <a name="logical-networks-for-gateways-and-the-software-load-balancer"></a>用于网关和软件负载平衡逻辑网络
  
其他逻辑网络需要创建和预配网关和 SLB 使用量。 同样，你需要处理你的网络管理员联系以获取正确 IP 前缀、VLAN Id 和 IP 地址网关这些网络。

#### <a name="transit-logical-network"></a>公交逻辑网络
  
RAS 网关和 SLB/输入混合使用公交逻辑网络交换 BGP 等信息和北美/南（外部内部）租户交通。 通常将此子网的大小小于其他。 需要连接到具有中继和访问这些 Vlan 此子网仅在运行 RAS 网关或 SLB/输入混合虚拟机的物理计算主机上计算主机的网络适配器连接到的开关端口。 每个 SLB/输入混合或 RAS 网关虚拟机静态分配一个 IP 地址与公交逻辑网络。

#### <a name="public-vip-logical-network"></a>公共 VIP 逻辑网络  
  
公共 VIP 逻辑网络需要具有 IP 子网前缀可路由之外云环境 (通常 Internet 可路由)。  这些将前端外部客户端用于访问包括站点的网关前端 VIP 虚拟网络资源的 IP 地址。

#### <a name="private-vip-logical-network"></a>专用 VIP 逻辑网络
  
专用 VIP 逻辑网络不需要与用于 VIPs 仅从内部云客户端，如 SLB 管理器或专用服务访问会路由之外云中。
  
#### <a name="gre-vip-logical-network"></a>GRE VIP 逻辑网络

GRE VIP 网络中已存在单独的定义赋予网关虚拟机上为 S2S GRE 连接类型你 SDN 结构运行的 VIPs 子网。 此网络不需要在你的物理交换机或路由器中预配置并没有 VLAN 分配。   

### <a name="sample-network-topology"></a>示例网络拓扑

自定义子网前缀 VLAN Id，根据你的网络管理员指南网关 IP 地址。  
  
网络名称|子网|面具|在主干 VLAN ID|网关|预订<br />（示例）  
----------------|----------|--------|--------------------|-----------|-----------------------------  
|**公交**|10.10.10.0|24|10|10.10.10.1|10.10.10.1-路由器  
|**公共 VIP**|41.40.40.0|27|纳帕|41.40.40.1|41.40.40.1-路由器<br /> 41.40.40.2-SLB/输入混合 VIP<br />41.40.40.3-IPSec S2S VPN VIP  
|**专用 VIP**|20.20.20.0|27|纳帕|20.20.20.1|20.20.20.1-默认网关（路由器）  
|**GRE VIP**|31.30.30.0|24|纳帕|31.30.30.1|31.30.30.1-默认网关|  
  
### <a name="logical-networks-required-for-rdma-based-storage"></a>逻辑 RDMA 基于存储所需的网络  
  
如果你是使用 RDMA 基于存储，然后你将需要在你计算和存储主机定义 VLAN 和子网为每个物理适配器。 通常，你将有两个物理适配器，每个节点此配置。  
  
> [!IMPORTANT]  
> 大多数物理开关需要将其发送以优质的服务设置下订单来正确应用中标记的 VLAN 上 RDMA 通信。  不要放置 RDMA 交通到标记的 VLAN 或物理访问模式端口。  
  
网络名称  |子网  |面具  |在主干 VLAN ID  |网关  |预订<br />（示例）    
---------|---------|---------|---------|---------|---------  
**容量 1**     |    10.60.36.0     | 25        |   8      |  10.60.36.1       |  10.60.36.1-路由器<br />10.60.36.x-计算主机 x<br />10.60.36.y-计算主机 y<br />10.60.36.v-计算群集<br />10.60.36.w-存储群集  
|**Storage2**|10.60.36.128|25|9|10.60.36.129|10.60.36.129-路由器<br />10.60.36.x-计算主机 x<br />10.60.36.y-计算主机 y<br />10.60.36.v-计算群集<br />10.60.36.w-存储群集  
  
有关配置切换的详细信息，请参阅**配置示例**部分。  
  
## <a name="routing-infrastructure"></a>路由基础结构  
  
如果你正在部署您使用脚本的 SDN 基础结构，管理、HNV 提供商、公交和 VIP 子网必须是可路由到彼此物理网络上。     
  
路由信息 \ (例如下一个 hop\) 的 VIP 子网公布通过 SLB/输入混合和 RAS 网关带入物理网络使用内部 BGP 对等。 VIP 逻辑网络并不一定 VLAN 分配，不是在第二层切换（例如顶部的机架开关）预配置。  
  
你需要在你 SDN 基础结构用于接收公布通过 SLB/MUXes 和 RAS 网关的 VIP 逻辑网络路线路由器上创建 BGP 等。 对 BGP 等只需（从 SLB/输入混合或 RAS 通往外部 BGP 等）中出现的一种方法。  上方的第一层的路由你可以使用静态路线或另一种动态路由协议 OSPF，如但是，如上文所述，VIP 逻辑网络 IP 子网前缀执行需要路由从物理网络对外部 BGP 等。   
  
通常，在托管的交换机或网络基础结构的一部分路由器配置 BGP 对等。 BGP 等还可能 Windows Server 上已安装在路由仅模式下访问远程服务器 (RAS) 角色配置。 必须配置网络基础架构此 BGP 路由器同级自己 ASN 和允许从分配给 SDN 组件 ASN 对等 \（SLB/输入混合和 RAS Gateways\）。 你必须从你的物理路由器或该路由器的控件中的网络管理员获取以下信息：

- 路由器 ASN  
- 路由器的 IP 地址  
- 以供 SDN 组件（可以专用 ASN 范围从任何 AS 编号）ASN

>[!NOTE]
>四个字节 Asn SLB/输入混合不支持。 你必须将两个字节 Asn 分配到 SLB/输入混合和路由器 wo 的连接。 可以 4 字节 Asn 使用您的环境中的其他位置。  
  
你或你的网络管理员必须配置 BGP 路由器等接受 ASN 和 IP 地址或子网地址，使用你的 RAS 网关和 SLB/MUXes 公交逻辑网络的连接。
  
有关详细信息，请参阅[边框网关协议 & #40;BGP & #41;](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).
  
## <a name="default-gateways"></a>默认网关

配置为连接到多个网络，如物理主机和网关虚拟机的计算机必须只能在一个默认网关配置。 通常分区会配置默认网关适配器用于到达一直向 Internet 上。

对于虚拟机，使用以下规则决定要用作默认网关哪个网络：

1. 公交网络用作默认网关，虚拟机连接到网络公交时，或是否多穴公交与任何其他共享网络。  
2. 管理网络用作默认网关，如果虚拟机仅连接到管理网络。  
3.  作为默认网关必须永远不会使用 HNV 提供商的网络。 唯一的虚拟机连接到该网络将 SLB/MUXes 和 RAS 网关。  
4.  虚拟机将永远不会直接连接到的容量 1、Storage2、公用 VIP 或专用 VIP 网络。  
  
对于 Hyper-V 主机和存储节点，将管理网络用作默认网关。  存储网络永远不会必须默认网关分配。
  
## <a name="network-hardware"></a>网络硬件

你可以使用以下各部分计划网络硬件部署。

### <a name="network-interface-cards-nics"></a>网络接口卡 (Nic)

为了获得最佳性能，在你使用 Hyper-V 主机和存储主机中的网络接口卡需要特定功能。  
 
远程直接内存访问(RDMA) 是内核绕过便可以将大量数据传输不涉及主机 CPU 的技术。 网络适配器上的 DMA 引擎执行传送，因为 CPU 不使用的内存移动。  这将释放 CPU 执行其他操作。  

切换 Embedded 组队（集）是一种替代 NIC 组合解决方案，你可以在 Hyper-V 和软件定义网络 (SDN) 堆栈包括在 Windows Server 2016 的环境中使用。 组集成到 Hyper-V 虚拟交换机用来 NIC 组合的某些功能。

有关详细信息，请参阅[远程直接内存访问 & #40;RDMA & #41;切换 Embedded 组队 & #40; 以及设置和 #41;](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  

以涵盖致 VXLAN 或 NVGRE 封装标题租户虚拟网络通信的开销，（切换和主机）的第二层结构网络 MTU 必须设置为大于或等于 1674 字节 \（包括第二层以太网 headers\）。 支持新的 Nic *EncapOverhead*高级的适配器关键字将设置通过网络控制器主机代理自动 MTU。 不支持新的 Nic *EncapOverhead*关键字需要在每台使用的物理主机上手动设置 MTU 大小*JumboPacket* \(or equivalent\) 关键字。

### <a name="switches"></a>切换
  
当选择物理切换和您的环境的路由器确保它支持以下套功能。  

- 交换机端口 MTU 设置 \(required\)  
- MTU 设置为 > = 1674 字节 \（包括 Header\ L2 以太网）  
- L3 协议 \(required\)  
- ECMP  
- BGP \(IETF RFC 4271\)\-based ECMP

实现系统必须支持必须明细表中以下 IETF 标准。

- RFC 2545:"BGP 4 多协议扩展 IPv6 域间路由"  
- RFC 4760:"多协议扩展 BGP 4"  
- RFC 4893:"的四个字节 AS BGP 支持编号空间"  
- RFC 4456:"BGP 路线倒影：全屏替代 Mesh 内部 BGP (IBGP)"  
- 对于 BGP 机制 RFC 4724:"此壮美重启"  

以下标记协议是必需的。

- VLAN-隔离的各种类型的通信
- 802.1 q 主干

以下各项提供链接控制。

- 服务质量 \ (如果使用 RoCE\ 仅必需 PFC)
- 增强交通所选内容 \(802.1Qaz\)
- 基于流控制优先级 \（802.1 p/Q 和 802.1Qbb\）

提供以下各项的可用性和冗余。

- 切换可用性（必需）
- 高度可用路由器才能执行网关功能。 你可以使用多机箱 switch\ 路由器或 VRRP 类似的技术来执行此操作。
        
以下各项提供管理功能。

**监视**

- SNMP v1 或 SNMP v2（如果使用网络控制器物理开关监视必需）  
- SNMP Mib \（如果您使用的网络控制器 for 物理开关 monitoring\ 必需）  
- MIB-II (RFC 1213)、LLDP，接口 MIB \(RFC 2863\)，如果-MIB IP MIB、IP-前进 MIB、Q-大桥 MIB、大桥 MIB、LLDB MIB、实体 MIB、IEEE8023-延迟 MIB  
  
下图显示的示例四个节点设置。 出于清晰度，第一个图显示只需网络控制器、第二个显示网络控制器和软件负载平衡，和第三个图显示了网络控制器、软件负载平衡和网关。  
  
存储网络和 vNICs 不可 shonwn 这些图中。 如果您计划使用 SMB 基于存储，以下是需。    
  
在（假定正确的逻辑网络存在正确的网络连接）的任何物理计算主机，可以重新分发的基础结构和租户虚拟机。  
  
### <a name="network-controller-deployment"></a>网络控制器部署

部署网络控制器之前，你必须查看安装软件要求以及配置安全和动态 DNS 注册。 有关详细信息，请参阅[安装和部署网络控制器的准备工作要求](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)。

三个网络控制器节点配置虚拟机上都具有高可以设置。 也显示为两个租户对租户 2 虚拟网络分为两个虚拟子网模拟 web 层和数据库层。  

![SDN NC 计划](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>网络控制器和软件加载平衡部署

高可用性，有两个或多个输入混合 SLB/节点。
   
![SDN NC 计划](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)
  
### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>网络控制器、软件负载平衡，并 RAS 网关部署

有三个网关虚拟机;两个处于活动状态，并且其中一个冗余。

![SDN NC 计划](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  
  
  
  
基于 TP5 部署自动化、Active Directory 必须可用和可以访问来自这些个子网。 有关 Active Directory 的详细信息，请参阅[Active Directory 域服务概述](https://technet.microsoft.com/en-us/library/mt703721.aspx)。  
  
>[!IMPORTANT] 
>如果使用 VMM 部署，确保你的基础结构虚拟机 (VMM 服务器，广告 DNS SQL Server、等) 不位于任何图所示的四个主机。  
  
## <a name="switch-configuration-examples"></a>切换配置示例  
  
若要配置你的物理交换机或路由器的帮助，由于各种切换型号和供应商的样本配置文件的一组都在[Microsoft SDN Github 存储库](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples)。 提供了详细的自述和测试的命令行界面）的命令专用开关。         
  
  
## <a name="compute"></a>计算  
所有 Hyper-V 主机必须都具有安装了 Windows Server 2016、启用 Hyper-V，并且外部 Hyper-V 虚拟交换机用来创建了至少一个物理适配器连接到管理逻辑网络。 主机必须接通通过分配给管理主机 vNIC 管理 IP 地址。  
  
可以使用任何使用 Hyper-V，共享或当地兼容的存储类型。   
  
> [!TIP]  
> 如果你使用的你所有的虚拟交换机相同的名称，但它不是强制，将方便。 如果你计划部署脚本与，请参阅与关联的注释`vSwitchName`变量 config.psd1 文件中。  
  
**主机计算要求**  
下表显示的示例部署中所使用的四个物理主机的最低硬件和软件要求。  
  
主机|硬件要求|软件要求|  
--------|-------------------------|-------------------------  
|物理 hyper-v 主机|4 核心 2.66 GHz CPU<br /><br />32 GB RAM<br /><br />磁盘空间的 300 GB<br /><br />1 Gb/秒（或更快地）物理网络适配器|操作系统：Windows Server 2016<br /><br />Hyper-V 安装角色|  
  
  
**SDN 基础结构虚拟机角色要求**  
  
角色|vCPU 要求|内存要求|磁盘要求|  
--------|---------------------|-----------------------|---------------------  
|网络控制器（三个节点）|4 vCPUs|4 GB 分钟 (推荐 8 GB)|OS 驱动器 75 GB  
|SLB/输入混合（三个节点）|8 vCPUs|推荐为 8 GB|OS 驱动器 75 GB  
|RAS 网关<br /><br />（三个节点网关、两个活动、一个被动单个池）|8 vCPUs|推荐为 8 GB|OS 驱动器 75 GB  
|对于每月输入 SLB 混合对等 RAS 网关 BGP 路由器<br /><br />（或者用作或切换 BGP 路由器）|2 vCPUs|2 GB|OS 驱动器 75 GB|  
  
  
如果你使用用于部署的 VMM，将需要 VMM 和其他非 SDN 基础结构附加的基础结构虚拟机资源。 有关其他信息，请参阅[系统中心 Technical preview 最低硬件建议。](https://technet.microsoft.com/library/dn997303.aspx)  
  
## <a name="extending-your-infrastructure"></a>扩展基础结构  
您的基础结构的调整大小和资源要求是取决于你打算接待租户工作负载虚拟机。 CPU、内存和磁盘需求的基础结构虚拟机 (例如：网络控制器、SLB、网关、等) 上的表列出了。 你可以添加多个能够根据需要扩展这些基础结构虚拟机。 但是，在 Hyper-V 主机上运行任何租户虚拟机具有自己的 CPU、内存和磁盘要求，你必须考虑。   
  
当租户工作负载虚拟机开始消耗物理 Hyper-V 主机上的资源太多时，你可以通过添加其他物理主机扩展您的基础结构。 可以与虚拟机管理器或使用（具体取决于你最初部署方式的基础结构）PowerShell 脚本完成创建新的服务器资源通过网络控制器。 如果你需要添加 HNV 提供商的网络的其他 IP 地址，则可以创建新逻辑子网（使用相应 IP 池），可使用在主机。  
  
  
## <a name="see-also"></a>请参阅  
[安装和部署网络控制器的准备工作要求](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
[软件定义的网络 & #40;SDN & #41;](../Software-Defined-Networking--SDN-.md)  
  


