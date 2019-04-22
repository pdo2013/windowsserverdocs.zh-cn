---
title: Windows Server 支持的网络方案
description: 本主题提供有关新支持的网络方案中 Windows Server 2016 及更高版本的信息
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 85f73f1f7caf833d23d3d693c0d754f52c4aa27d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812228"
---
# <a name="windows-server-supported-networking-scenarios"></a>Windows Server 支持的网络方案

>适用于：Windows Server\(半年频道\)，Windows Server 2016

本主题提供有关支持和不受支持的方案，您可以或不能执行与此版本的 Windows Server 2016 的信息。  
>[!IMPORTANT]
>对于所有生产方案，使用最新签名的硬件驱动程序从原始设备制造商\(OEM\)或独立硬件供应商\(IHV\)。
  
## <a name="bkmk_supp"></a>支持的网络方案

本部分包括有关受支持的网络方案的 Windows Server 2016 的信息，并且包括以下方案类别。  
  
-   [软件定义网络 (SDN) 方案](#bkmk_sdn)  
  
-   [网络平台方案](#bkmk_netp)  
  
-   [DNS 服务器方案](#bkmk_dns)  
  
-   [IPAM DHCP 和 DNS 方案](#bkmk_ipam)  
  
-   [NIC 组合方案](#bkmk_nicteam)

- [交换机嵌入式组合\(设置\)方案](#bkmk_set)
  
### <a name="bkmk_sdn"></a>软件定义网络 (SDN) 方案
 
以下文档可用于部署 Windows Server 2016 的 SDN 方案。  
  
  
-   [部署软件定义的网络基础结构使用脚本](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
有关详细信息，请参阅[软件定义的网络&#40;SDN&#41;](sdn/software-defined-networking.md)。  
  
#### <a name="bkmk_netc"></a>网络控制器方案

网络控制器方案使您能够：  
  
-   部署和管理网络控制器的多个节点实例。 有关详细信息，请参阅[使用 Windows PowerShell 部署网络控制器](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)。  
  
-   使用网络控制器使用 REST Northbound API 以编程方式定义网络策略。  
  
-   使用网络控制器来创建和管理 HYPER-V 网络虚拟化-使用 NVGRE 或 VXLAN 封装的虚拟网络。  
  
有关详细信息，请参阅[网络控制器](sdn/technologies/network-controller/Network-Controller.md)。  
  
#### <a name="bkmk_netf"></a>网络功能虚拟化 (NFV) 方案  
NFV 方案使您能够：  
  
-   部署和使用软件负载均衡器分发 northbound 和 southbound 流量。  
  
-   部署和使用软件负载均衡器分配 eastbound 和 westbound 与 HYPER-V 网络虚拟化创建的虚拟网络的流量。  
  
-   部署和 NAT 软件负载均衡器用于创建与 HYPER-V 网络虚拟化的虚拟网络。  
  
-   部署和使用第 3 层转发网关  
  
-   部署和使用站点到站点 IPsec (IKEv2) 隧道虚拟专用网络 (VPN) 网关  
  
-   部署和使用通用路由封装 (GRE) 网关。  
  
-   部署和配置动态路由和使用边界网关协议 (BGP) 的站点之间的传输路由。  
  
-   配置 M + N 冗余的第 3 层和站点到站点网关和 BGP 路由。  
  
-   使用网络控制器来指定在虚拟网络和网络接口上的 Acl。  
  
有关详细信息，请参阅[网络功能虚拟化](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)。  
  
### <a name="bkmk_netp"></a>网络平台方案

在本部分中 Windows Server 网络方案团队支持任何 Windows Server 2016 认证的驱动程序的使用。 请与您的网络接口卡\(NIC\)制造商联系，以确保具有最新的驱动程序更新。
  
网络平台方案使您能够：  
  
-   使用聚合的 NIC 组合使用单个网络适配器的 RDMA 和以太网流量。  
  
-   使用 Packet Direct 中的 HYPER-V 虚拟交换机，并且单个网络适配器启用创建的低延迟数据路径。  
  
-   配置集来传播 SMB 直通和 RDMA 最多两个网络适配器之间的通信流。  
  
有关详细信息，请参阅[远程直接内存访问&#40;RDMA&#41;和交换机嵌入式组合&#40;设置&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。  
  
#### <a name="bkmk_switch"></a>HYPER-V 虚拟交换机的方案

HYPER-V 虚拟交换机方案使您能够：  
  
-   使用远程直接内存访问 (RDMA) vNIC 创建 HYPER-V 虚拟交换机  
  
-   使用交换机嵌入式组合 (SET) 和 RDMA Vnic 创建 HYPER-V 虚拟交换机  
  
-   在 HYPER-V 虚拟交换机中创建组团队  
  
-   使用 Windows PowerShell 命令管理将团队设置  
  
有关详细信息，请参阅[远程直接内存访问&#40;RDMA&#41;和交换机嵌入式组合&#40;设置&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>DNS 服务器方案

DNS 服务器方案使您能够：  
  
-   指定地理位置基于使用 DNS 策略的流量管理  
  
-   配置拆分式 DNS 使用 DNS 策略  
  
-   使用 DNS 策略的 DNS 查询上应用筛选器  
  
-   配置应用程序负载均衡使用 DNS 策略  
  
-   指定智能 DNS 响应基于一天的时间  
  
-   配置 DNS 区域传输策略  
  
-   配置策略，在 Active Directory 域服务 (AD DS) 集成的 DNS 服务器的区域  
  
-   配置响应速率限制  
  
-   指定命名实体 （窗格会） 基于 DNS 的身份的验证  
  
-   在 DNS 中配置对未知记录的支持  
  
有关详细信息，请参阅主题[What's New in Windows Server 2016 中 DNS 客户端](dns/What-s-New-in-DNS-Client.md)并[What's New in Windows Server 2016 中 DNS 服务器](dns/What-s-New-in-DNS-Server.md)。  
  
### <a name="bkmk_ipam"></a>IPAM DHCP 和 DNS 方案

IPAM 方案使您能够：  
  
-   发现和管理 DNS 和 DHCP 服务器和 IP 寻址跨多个联合的 Active Directory 林  
  
-   使用 IPAM 的 DNS 属性，包括区域和资源记录的集中式管理。  
  
-   定义精细的基于角色的访问控制策略和委派 IPAM 用户或用户组来管理指定的 DNS 属性的集。  
  
-   使用 IPAM 的 Windows PowerShell 命令来自动执行访问控制配置为 DHCP 和 DNS。  
  
    有关详细信息，请参阅[管理 IPAM](technologies/ipam/Manage-IPAM.md)。  
  
### <a name="bkmk_nicteam"></a>NIC 组合方案

NIC 组合方案使您能够：  
  
-   在受支持的配置中创建的 NIC 组  
  
-   删除 NIC 组  
  
-   将网络适配器添加到受支持的配置中的 NIC 组  
  
-   从 NIC 组中删除网络适配器  
  
> [!NOTE]  
> 在 Windows Server 2016 中，您可以使用 NIC 组合中的 HYPER-V，但在某些情况下虚拟机队列 (VMQ) 可能不会自动启用的基础网络适配器上时创建的 NIC 组。 如果发生这种情况，可以使用以下 Windows PowerShell 命令以确保 NIC 团队成员适配器上已启用 VMQ: `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

有关详细信息，请参阅[NIC 组合](technologies/nic-teaming/NIC-Teaming.md)。 

### <a name="bkmk_set"></a>交换机嵌入式组合\(设置\)方案

集是一个可以在 Windows Server 2016 中包括的 HYPER-V 和软件定义网络 (SDN) 堆栈的环境中使用的替代 NIC 组合解决方案。 组将某些 NIC 组合功能集成到 HYPER-V 虚拟交换机。 

有关详细信息，请参阅[远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>不受支持的网络方案  
在 Windows Server 2016 中不支持以下网络方案。  
  
-   基于 VLAN 的租户虚拟网络。  
  
-   包含或覆盖中不支持 IPv6。  
  


