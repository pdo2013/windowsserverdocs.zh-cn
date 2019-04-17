---
title: Windows Server 受支持的网络方案
description: 本主题提供有关新受支持网络连接情况下在 Windows Server 2016 及更高版本的信息
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 70198f97c4ec39de4b78de28ab196dc3e86a684c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="windows-server-supported-networking-scenarios"></a>Windows Server 受支持的网络方案

>适用于：Windows Server \(Semi-Annual Channel\)，Windows Server 2016

本主题提供受支持，且不受支持的方案，您可以或无法执行此版本的 Windows Server 2016 的信息。  
>[!IMPORTANT]
>所有生产情况下，都使用你的原始设备制造商的最新签名的硬件驱动程序 \(OEM\) 或独立的硬件供应商 \(IHV\)。
  
## <a name="bkmk_supp"></a>受支持的网络方案

此部分中包含有关受支持的网络方案针对 Windows Server 2016，和信息包含以下情况类别。  
  
-   [软件定义网络 (SDN) 方案](#bkmk_sdn)  
  
-   [网络平台方案](#bkmk_netp)  
  
-   [DNS 服务器方案](#bkmk_dns)  
  
-   [DHCP 和 DNS IPAM 方案](#bkmk_ipam)  
  
-   [NIC 组合方案](#bkmk_nicteam)

- [切换 Embedded 组队 \(SET\) 方案](#bkmk_set)
  
### <a name="bkmk_sdn"></a>软件定义网络 (SDN) 方案
 
可以使用下面的文档部署 SDN 与 Windows Server 2016 的方案。  
  
  
-   [部署软件定义的网络基础结构使用脚本](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
有关详细信息，请参阅[软件定义网络 & #40;SDN & #41;](sdn/software-defined-networking.md).  
  
#### <a name="bkmk_netc"></a>网络控制器方案

网络控制器方案，您可以：  
  
-   部署和管理网络控制器的多个节点实例。 有关详细信息，请参阅[部署网络控制器使用 Windows PowerShell](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)。  
  
-   使用网络控制器编程方式使用 Northbound REST API 定义网络策略。  
  
-   使用网络控制器创建和管理 Hyper-V 网络虚拟化-使用 NVGRE 或 VXLAN 封装虚拟网络。  
  
有关详细信息，请参阅[网络控制器](sdn/technologies/network-controller/Network-Controller.md)。  
  
#### <a name="bkmk_netf"></a>网络函数虚拟化 (NFV) 方案  
NFV 方案，您可以：  
  
-   部署和一个软件负载平衡用于分配 northbound 和 southbound 通信。  
  
-   部署和一个软件负载平衡用于分配了 Hyper-V 网络虚拟化创建虚拟网络 eastbound 和 westbound 交通。  
  
-   部署和 NAT 软件负载平衡用于创建了 Hyper-V 网络虚拟化虚拟网络。  
  
-   部署和使用层 3 转移网关  
  
-   部署和使用进行站点的隧道 (IKEv2) 虚拟专用网络 (VPN) 网关  
  
-   部署，并使用通用路由封装 (GRE) 网关。  
  
-   部署和配置动态路由和公交路由使用边框网关协议 (BGP) 的站点之间。  
  
-   为 3 层和站点的网关，以及为 BGP 路由配置 M + N 冗余。  
  
-   使用网络控制器上虚拟网络和网络接口指定 Acl。  
  
有关详细信息，请参阅[网络函数虚拟化](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)。  
  
### <a name="bkmk_netp"></a>网络平台方案

对于此部分中的方案 Windows Server 网络团队支持任何 Windows Server 2016 认证的驱动程序的使用。 请咨询你的网络接口卡 \(NIC\) 制造商，以确保你具有最新的驱动程序更新。
  
网络平台方案，您可以：  
  
-   使用已合并好的 NIC 组合 RDMA 和以太网使用单个网络适配器的交通。  
  
-   通过使用数据包直接，在 Hyper-V 虚拟交换机用来，并在单个网络适配器中启用创建低延迟数据路径。  
  
-   配置设置，以分散 SMB 直接和 RDMA 通信流之间达两个网络适配器。  
  
有关详细信息，请参阅[远程直接内存访问 & #40;RDMA & #41;切换 Embedded 组队 & #40; 以及设置和 #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
#### <a name="bkmk_switch"></a>Hyper-V虚拟交换机用来方案

Hyper-V 虚拟交换机用来方案，您可以：  
  
-   使用远程直接内存访问 (RDMA) vNIC 创建一个虚拟交换机用 Hyper-V  
  
-   使用切换 Embedded 组队 （集） 和 RDMA vNICs 创建一个虚拟交换机用 Hyper-V  
  
-   在 Hyper-V 虚拟交换机用来中创建组团队  
  
-   使用 Windows PowerShell 命令管理集团队  
  
有关详细信息，请参阅[远程直接内存访问 & #40;RDMA & #41;切换 Embedded 组队 & #40; 以及设置和 #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>DNS 服务器方案

DNS 服务器方案，您可以：  
  
-   指定地理位置基于交通管理使用 DNS 策略  
  
-   配置使用 DNS 策略裂 DNS  
  
-   有关使用 DNS 策略 DNS 查询应用筛选器  
  
-   配置使用 DNS 策略应用程序负载平衡  
  
-   指定智能 DNS 响应将基于当天的时间  
  
-   配置 DNS 区域传输策略  
  
-   集成的 Active Directory 域服务 (广告 DS) 区域配置 DNS 服务器策略  
  
-   配置响应率限制  
  
-   指定 DNS 基于命名的实体 (DANE) 的身份验证  
  
-   在 DNS 配置未知记录的支持  
  
有关详细信息，请参阅主题[什么是 Windows Server 2016 中的 DNS 客户端中的新增功能](dns/What-s-New-in-DNS-Client.md)和[DNS 服务器，在 Windows Server 2016 的新增](dns/What-s-New-in-DNS-Server.md)。  
  
### <a name="bkmk_ipam"></a>DHCP 和 DNS IPAM 方案

IPAM 方案，您可以：  
  
-   发现和管理 DHCP 和 DNS 服务器，并跨多个联盟 Active Directory 林 IP 地址  
  
-   用于 DNS 属性，包括区域和资源记录的集中管理 IPAM。  
  
-   定义具体基于角色访问控制策略并委派 IPAM 用户或用户组来管理您指定的 DNS 属性套。  
  
-   使用 Windows PowerShell 用于命令 IPAM 自动化 DHCP 和 DNS 访问控制配置。  
  
    有关详细信息，请参阅[管理 IPAM](technologies/ipam/Manage-IPAM.md)。  
  
### <a name="bkmk_nicteam"></a>NIC 组合方案

NIC 组合方案，您可以：  
  
-   在受支持的配置中创建 NIC 团队  
  
-   删除 NIC 团队  
  
-   添加到受支持的配置 NIC 团队的网络适配器  
  
-   删除 NIC 团队中的网络适配器  
  
> [!NOTE]  
> 在 Windows Server 2016 中，你可以使用 NIC 组合中 Hyper-V，但是在某些情况下虚拟机队列 (VMQ) 可能不会自动上基础的网络适配器时启用创建 NIC 团队。 如果发生这种情况，可以使用下面的 Windows PowerShell 命令以确保 VMQ 已启用了 NIC 团队成员适配器： `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

有关详细信息，请参阅[NIC 组合](technologies/nic-teaming/NIC-Teaming.md)。 

### <a name="bkmk_set"></a>切换 Embedded 组队 \(SET\) 方案

设置是你可以在 Hyper-V 和软件定义网络 (SDN) 堆栈包括在 Windows Server 2016 的环境中使用的替代 NIC 组合解决方案。 组集成到 Hyper-V 虚拟交换机用来 NIC 组合的某些功能。 

有关详细信息，请参阅[远程直接内存访问 (RDMA) 和切换 Embedded 组队 （集）](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>不受支持的网络方案  
Windows Server 2016 中不支持以下网络方案。  
  
-   基于 VLAN 租户虚拟网络。  
  
-   IPv6 包含或覆盖中不支持。  
  


