---
title: SDN 技术
description: 在此部分中的主题提供概述以及有关 Windows Server 2016 中包含的软件定义网络技术的技术的信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f842ac0d1a09106c1898374cf8dd1c7823d7dae
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="sdn-technologies"></a>SDN 技术

>适用于：Windows Server（半年通道），Windows Server 2016

在此部分中的主题提供概述以及有关 Windows Server 2016 中包含的软件定义网络技术的技术的信息。  
  
> [!NOTE]  
> 有关其他网络软件定义的文档，你可以使用下面的库部分。  
>   
> - [计划 SDN](../plan/Plan-Software-Defined-Networking.md)
> - [部署 SDN](../deploy/Deploy-Software-Defined-Networking.md)
> - [管理 SDN](../manage/manage-sdn.md)
> - [SDN 的安全](../security/sdn-security-top.md)
> - [疑难解答 SDN](../troubleshoot/Troubleshoot-Software-Defined-Networking.md)

有许多技术合作以创建 Microsoft 软件定义网络 (SDN) 解决方案，其中包括以下程序：  
  
-   **[边框网关协议 & #40;BGP & #41;](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)**  
  
    在上一个 Windows Server 2016 远程访问服务 (RAS) 网关配置，边框网关协议 (BGP) 提供了功能管理传送网络租户的 VM 网络和他们远程访问的站点之间的交通。 BGP 减少在路由器上的路线手动配置需要，因为它是动态路由协议，并自动学习通过使用站点的 VPN 连接来连接的站点之间的路线。  
  
-   **[Datacenter 防火墙概述](../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)**  
  
    Datacenter 防火墙是一种新随附于 Windows Server 2016 服务。 它是网络层，5 元协议、 源代码和目的地端口号 （源代码和目的地的 IP 地址），状态、 租户防火墙。 作为一项服务服务提供商提供和部署时租户管理员可以安装和配置防火墙策略，以帮助保护其虚拟网络从有害来自 Internet 的通信和 intranet 网络。  
  
  
-   **[Hyper-V 网络虚拟化](../../sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)**  
  
    HYPER-V 网络虚拟化 (HNV) 实现了虚拟化客户在顶部共享物理网络基础结构的网络。  
  
- **[内部 DNS 服务 #40; idn，则 & #41;对于 SDN](../../sdn/technologies/Idns-for-Sdn.md)**

    托管的虚拟机 \(VMs\) 和应用程序需要 DNS 内自己网络和 Internet 上的外部资源进行通信。 与 idn，则，你可以与 DNS 名称分辨率服务提供租户对其独立、 当地名称空间和 Internet 资源。

-   **[网络控制器](../../sdn/technologies/network-controller/Network-Controller.md)**  
  
    网络控制器提供集中、 可编程自动化以管理、 配置、 监视和解决你的数据中心中的虚拟和物理网络基础结构的点。  
  
-   **[网络函数虚拟化](../../sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)**  
  
    越来越为虚拟装置虚拟化由硬件设备 （如负载平衡防火墙、 路由器，开关，以及等） 的网络功能。  
  
    网络交换机、 网关、 Nat、 负载平衡，防火墙，Microsoft 已虚拟化。  

-   **[SDN RAS 网关](../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)**
  
    RAS 网关是一个软件基于、 租户，边框网关协议 (BGP) 支持路由器中有适用于云服务提供商 (Csp) 和主机使用 HYPER-V 网络虚拟化多个租户虚拟网络的企业的 Windows Server 2016。  
      
- **[远程直接内存访问 & #40;RDMA & #41;切换 Embedded 组队 & #40; 以及设置和 #41;](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)**  
  
    你可以使用已合并好的 NIC 结合使用的网络适配器 RDMA 和以太网交通。 已合并好的 NIC 允许你的网络适配器可用于管理、 远程直接内存访问 RDMA 启用存储，以及租户交通。 这减少与您的数据中心中的每个服务器资金支出，因为需要更少的网络适配器管理不同类型的每个服务器的交通。  
  
    设置是已集成 HYPER-V 虚拟交换机用来在 NIC 组合解决方案。 设置允许达八物理 NIC 分组到一个集团队，从而提高可用性并提供故障转移。 在 Windows Server 2016，可以创建限于 RDMA 服务器消息块 (SMB) 以及使用的设置团队。
  

-   **[软件负载平衡 & #40;SLB & #41;对于 SDN](../../sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)**  

    云服务提供商 (Csp) 和部署软件定义网络 (SDN) 在 Windows Server 2016 的企业可以使用软件负载平衡 (SLB) 激增，均匀地分布租户和虚拟网络资源的租户客户网络通信。 Windows Server SLB 使主持相同的工作负载，多个服务器高可用性和可扩展性提供。
  
-   **[System Center](../../sdn/Sc-Tech-for-Sdn.md) **可用于系统中心 2016年虚拟机管理器 (VMM) 和运营经理部署和管理 SDN 基础结构，包括网络控制器、 软件负载平衡，以及网关。 你还可以使用 VMM 集中定义控制虚拟网络策略并链接到你的应用程序或工作负载的策略。
  
- **[Windows 容器](../technologies/Containers/Container-networking-overview.md)**
    
    Windows Server 容器是一个简洁操作系统虚拟化方法，用于从其他服务在同一容器主机上运行的分隔应用程序或服务。 若要启用此功能，每个容器具有操作系统、 进程，文件系统、 注册表以及 IP 地址的视图。 与 Windows Server 2016 中，现在可以将 Windows Server 容器连接到虚拟网络。 Windows 容器类似于网络关于希望在虚拟机的功能。 每个容器已连接到哪些传入和传出的通信的虚拟交换机虚拟网络适配器。 强制容器同一台主机上的分隔，为每个 Windows Server 和 HYPER-V 容器容器该网络适配器安装到其中创建网络盒。 Windows Server 容器使用主机 vNIC 吸附到虚拟交换机用来。 HYPER-V 容器使用综合 VM NIC （不会受到实用程序 VM） 连接到虚拟交换机用来。

