---
title: 软件定义的网络 (SDN)
description: 你可以使用此主题以了解有关在 Windows Server、 System Center 和 Microsoft Azure 提供的软件定义网络 (SDN) 技术。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9a1ea73c-20cd-42c5-95ad-b003b9cc6d64
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33351ccfa1466f667ef9351768c89b373734075d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="software-defined-networking-sdn"></a>软件定义的网络 (SDN)

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此主题以了解有关在 Windows Server Datacenter edition，系统中心 2016 和 Microsoft Azure 提供的软件定义网络 (SDN) 技术。  
  
> [!NOTE]
> 本主题中，除了以下 SDN 内容才可用。
> 
> - [什么是 Windows Server 的 SDN 中的新增功能](sdn-whats-new.md)
> - [在 Windows Server 的数据中心 SDN 简介](sdn-intro.md)
> - [SDN 技术](technologies/Software-Defined-Networking-Technologies.md)  
> - [计划 SDN](plan/Plan-Software-Defined-Networking.md) 
> - [部署 SDN](deploy/Deploy-Software-Defined-Networking.md)  
> - [管理 SDN](manage/manage-sdn.md)
> - [SDN 的安全](security/sdn-security-top.md)
> - [疑难解答 SDN](troubleshoot/Troubleshoot-Software-Defined-Networking.md)
> - [对于 SDN system Center 技术](Sc-Tech-for-Sdn.md)
> - [MicrosoftAzure 和 SDN](Azure_and_Sdn.md)
> - [请联系 Datacenter 和云网络团队](contact-sdn-team.md)
  
## <a name="bkmk_sdn"></a>软件定义的网络概述

软件定义的网络 \(SDN\) 提供的方法可集中配置和管理等路由器、 切换以及网关您的数据中心中的物理和虚拟网络设备。 如 HYPER-V 虚拟交换机用来、 HYPER-V 网络虚拟化和 RAS 网关虚拟网络元素设计为不可或缺元素 SDN 基础结构。

>[!NOTE]
>有关 Hyper-V 主机和运行 SDN 基础结构服务器，如网络控制器和软件负载平衡节点的虚拟机 \(VMs\) 必须安装 Windows Server 2016 Datacenter edition。 对于 Hyper-V 主机包含仅租户工作负载连接到网络 SDN\ 控制虚拟机的功能，你可以运行 Windows Server 2016 标准版。

同时你仍然可以使用你的现有物理开关、 路由器和其他硬件设备，您可以实现虚拟网络和物理网络之间的深度集成，如果在这些设备旨在软件定义的网络的兼容性。  
  
SDN 则，因为网络飞机-管理、 控件和数据飞机-不再绑定网络设备本身，但都抽象使用通过其他实体，如 datacenter 管理软件，如系统中心 2016年。  
  
SDN 允许你动态管理数据中心网络提供的应用程序和工作负载满足自动化、 集中方法。 软件定义的网络提供了以下功能。  
  
-   能够抽象你的应用和从基础物理网络，可以通过虚拟化网络的工作负载。 与一样服务器虚拟化使用 HYPER-V，它们位于一致且工作与你的应用程序和工作负载非中断的方式。 例如，软件定义的网络提供有关你的物理网络元素，如 IP 地址、 开关和负载平衡虚拟抽象。  
  
-   集中定义功能和控制策略控制物理和虚拟网络，包括之间这两种的网络类型的数据流量。  
  
-   即使在你部署新的工作负载或在虚拟或物理网络移动工作负载一致缩放，以进行实施网络策略的能力。  
  
## <a name="bkmk_ws"></a>Windows Server 技术软件定义的网络  
Windows Server 包含以下软件定义的网络技术。  

### <a name="bkmk_nc"></a>网络控制器

新 Windows Server 2016、 网络控制器提供的集中、 可编程点自动化管理、 配置、 监视和虚拟两疑难解答和您的数据中心中的物理网络基础结构。 使用网络控制器，你可以自动网络基础结构，而不是执行手动配置的网络的设备和服务的配置。  
  
网络控制器高度可用和可扩展服务器角色，并提供一个应用程序编程接口 (API)-Southbound API-，可使网络控制器通信网络，和第二个 API-Northbound API-，以便你可以与网络控制器通信。  
  
使用 Windows PowerShell、 具象传输状态 (REST) API，或管理应用程序，你可以使用网络控制器管理以下物理和虚拟网络基础结构。  
  
-   Hyper-V 虚拟机的功能和虚拟交换机  
  
-   物理网络交换机  
  
-   物理网络路由器  
  
-   防火墙软件  
  
-   VPN 网关、 包括远程访问服务 (RAS) Multitenant 网关  
  
-   负载平衡  
  
有关详细信息，请参阅[网络控制器](../sdn/technologies/network-controller/Network-Controller.md)。  
  
### <a name="bkmk_hv"></a>Hyper V 网络虚拟化

HYPER-V 网络虚拟化可帮助你使用虚拟网络抽象你的应用和从物理网络的工作负载。 虚拟网络提供的必要租户隔离时物理共享的网络结构，从而提高资源使用状况上运行。 若要确保你可以执行期待你现有的投资，你可以设置现有网络齿轮虚拟网络。 此外，虚拟网络都与虚拟本地网络 (Vlan) 兼容。  
  
有关详细信息，请参阅[HYPER-V 网络虚拟化](../sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)。  
  
### <a name="bkmk_switch"></a>Hyper V 虚拟交换机用来

HYPER-V 虚拟切换是一个基于软件的第二层以太网网络交换机，你已安装了 HYPER-V 服务器角色后在 HYPER-V 管理器中可用。 切换包括编程方式托管和可扩展连接到虚拟网络和物理网络虚拟机的功能。 此外，Hyper-V 虚拟交换机用来提供策略实施的安全性、隔离和服务级别。  
  
在 HYPER-V 虚拟交换机用来在 Windows Server 2016 中，你可以部署切换 Embedded 组队 （集） 和远程直接内存使用 (RDMA)。 有关详细信息，请参阅部分[远程直接内存使用 (RDMA) 和切换 Embedded 组队 （集）](#bkmk_rdma)本主题中。  
  
关于 HYPER-V 虚拟交换机用来的详细信息，请参阅[HYPER-V 虚拟交换机用来](../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。

### <a name="bkmk_idns"></a>内部 DNS 服务 #40; idn，则 & #41;

托管的虚拟机 \(VMs\) 和应用程序需要 DNS 内自己网络和 Internet 上的外部资源进行通信。 与 idn，则，你可以与 DNS 名称分辨率服务提供租户对其独立、 当地名称空间和 Internet 资源。

有关详细信息，请参阅[内部 DNS 服务 #40; idn，则 & #41;对于 SDN](technologies/Idns-for-Sdn.md)。
  
### <a name="bkmk_nfv"></a>网络函数虚拟化

在当今的软件定义的数据中心，由硬件设备（如负载平衡防火墙、路由器，开关，以及等）的网络功能已越来越正在虚拟化为虚拟装置。 此"网络函数虚拟化"是的自然而然服务器虚拟化和网络虚拟化。 虚拟装置快速等新兴，并创建一个新市场中。 它们不断生成兴趣获得动力在这两个虚拟化平台和云服务。  
  
可使用以下网络函数虚拟化技术。  
  
-   **软件负载平衡 (SLB) 和网络地址翻译 (NAT)**。 北美南和东西分层显示 4 负载平衡和 NAT 通过支持直接服务器返回与其退货网络交通可以跳过负载平衡集线器增强吞吐量。 有关详细信息，请参阅[软件负载平衡 (SLB) 的 SDN](technologies/network-function-virtualization/software-load-balancing-for-sdn.md)。  
  
-   **Datacenter 防火墙**。 此分布式的防火墙提供具体访问 acl)，使你可以将防火墙策略，在 VM 接口级别或子网级别的应用。  
  
    有关详细信息，请参阅[Datacenter 防火墙概述](../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。  
  
-   **RAS 网关**。 你可以使用网关桥虚拟网络和非虚拟网络; 之间通信特别是，你可以将部署站点的 VPN 网关、 转移网关、 和泛型路由封装 (GRE) 网关。 此外，M + N 网关冗余中均受支持。 有关详细信息，请参阅[SDN RAS 网关](technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)。
  
有关详细信息，请参阅[网络函数虚拟化](technologies/network-function-virtualization/Network-Function-Virtualization.md)。  
  
### <a name="bkmk_rdma"></a>远程直接内存使用 (RDMA) 和切换嵌入分组 （集）  
在 Windows Server 2016，可以启用 RDMA 上绑定 HYPER-V 虚拟交换机或不切换 Embedded 组队 （集） 的网络适配器。 这允许你使用较少的网络适配器，当你想要使用 RDMA 和设置同时。  
  
设置是你可以在 HYPER-V 和软件定义网络 (SDN) 堆栈包括在 Windows Server 2016 的环境中使用的替代 NIC 组合解决方案。 设置将集成的某些 NIC 组合功能到 HYPER-V 虚拟交换机用来。  
  
设置允许你在一个和八物理以太网网络适配器一个或多个软件基于虚拟网络适配器插入之间进行分组。 这些虚拟网络适配器提供更快的性能以及故障功能的驱动器发生故障时的网络适配器。  
组成员网络适配器必须所有将在同一物理 HYPER-V 主机要放置在团队中安装。  
  
此外，你可以使用 Windows PowerShell 命令以启用数据中心桥 (DCB)、 使用 RDMA 虚拟 NIC (vNIC)，创建一个虚拟交换机用 HYPER-V 和使用集和 RDMA vNICs 创建一个虚拟交换机用 HYPER-V。  
  
有关详细信息，请参阅[远程直接内存使用 (RDMA) 和切换 Embedded 组队 （集）](https://technet.microsoft.com/library/mt403349.aspx)。  
  
### <a name="bkmk_rras"></a>SDN RAS 网关  
RAS 网关是一个软件基于、 租户，边框网关协议 (BGP) 支持路由器中有适用于云服务提供商 (Csp) 和主机使用 HYPER-V 网络虚拟化多个租户虚拟网络的企业的 Windows Server 2016。  
  
RAS 网关提供网关池、 M + N 冗余、 多种类型的站点之间的 VPN 连接和 BGP 路线反射以向您提供灵活设计选择您网关的基础结构。  
  
有关详细信息，请参阅[RAS 网关 SDN](technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
  
### <a name="bkmk_slb"></a>软件负载平衡 (SLB)  
云服务提供商 (Csp) 和部署软件定义网络 (SDN) 在 Windows Server 2016 的企业可以使用软件负载平衡 (SLB) 激增，均匀地分布租户和虚拟网络资源的租户客户网络通信。 Windows Server SLB 使主持相同的工作负载，多个服务器高可用性和可扩展性提供。  
  
有关详细信息，请参阅[软件负载平衡 & #40;SLB & #41;对于 SDN](../sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)。

### <a name="windows-server-containers"></a>Windows Server 容器

Windows Server 容器是一个简洁操作系统虚拟化方法，用于从其他服务在同一容器主机上运行的分隔应用程序或服务。 若要启用此功能，每个容器具有操作系统、 进程，文件系统、 注册表以及 IP 地址的视图。 与 Windows Server 2016 中，现在可以将 Windows Server 容器连接到虚拟网络。 有关详细信息，请参阅[Windows Server 容器](technologies/containers/Container-networking-overview.md)。

### <a name="contact-the-datacenter-and-cloud-networking-product-team"></a>联系 Datacenter 和云网络产品团队

如果你们有兴趣在讨论通过 Microsoft 或其他 SDN 客户 SDN 技术，有几种方法制定联系人。

有关详细信息，请参阅[联系 Datacenter 和云网络团队](contact-sdn-team.md)。
