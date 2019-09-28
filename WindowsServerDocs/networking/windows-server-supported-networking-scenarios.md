---
title: Windows Server 支持的网络方案
description: 本主题提供有关 Windows Server 2016 和更高版本中支持的新网络方案的信息
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f338ddf0a7d3a4fe41277ddbf49b0c3db34ae11b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395695"
---
# <a name="windows-server-supported-networking-scenarios"></a>Windows Server 支持的网络方案

>适用于：Windows Server \(Semi-年收入为 no__t-1，Windows Server 2016

本主题提供有关支持的和不支持的方案的信息，在此版本的 Windows Server 2016 中可以或不能执行此方案。  
>[!IMPORTANT]
>对于所有生产方案，请使用原始设备制造商提供的最新签名的硬件驱动程序，\(OEM @ no__t 或独立硬件供应商 \(IHV @ no__t。
  
## <a name="bkmk_supp"></a>支持的网络方案

此部分包含有关 Windows Server 2016 支持的网络方案的信息，并包括以下方案类别。  
  
-   [软件定义的网络（SDN）方案](#bkmk_sdn)  
  
-   [网络平台方案](#bkmk_netp)  
  
-   [DNS 服务器方案](#bkmk_dns)  
  
-   [具有 DHCP 和 DNS 的 IPAM 方案](#bkmk_ipam)  
  
-   [NIC 组合方案](#bkmk_nicteam)

- [切换嵌入组合 \(SET @ no__t-2 方案](#bkmk_set)
  
### <a name="bkmk_sdn"></a>软件定义的网络（SDN）方案
 
你可以使用以下文档，使用 Windows Server 2016 部署 SDN 方案。  
  
  
-   [使用脚本部署软件定义的网络基础结构](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
有关详细信息，请参阅[软件定义&#40;的&#41;网络 SDN](sdn/software-defined-networking.md)。  
  
#### <a name="bkmk_netc"></a>网络控制器方案

网络控制器方案使你能够：  
  
-   部署和管理网络控制器的多节点实例。 有关详细信息，请参阅[使用 Windows PowerShell 部署网络控制器](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)。  
  
-   使用 REST Northbound API 通过网络控制器以编程方式定义网络策略。  
  
-   使用网络控制器，通过 Hyper-v 网络虚拟化来创建和管理虚拟网络-使用 NVGRE 或 VXLAN 封装。  
  
有关详细信息，请参阅[网络控制器](sdn/technologies/network-controller/Network-Controller.md)。  
  
#### <a name="bkmk_netf"></a>网络功能虚拟化（NFV）方案  
NFV 方案使你能够：  
  
-   部署并使用软件负载平衡器来分发 northbound 和 southbound 流量。  
  
-   部署并使用软件负载平衡器，为使用 Hyper-v 网络虚拟化创建的虚拟网络分发 eastbound 和 westbound 流量。  
  
-   为使用 Hyper-v 网络虚拟化创建的虚拟网络部署和使用 NAT 软件负载均衡器。  
  
-   部署和使用第3层转发网关  
  
-   部署并使用站点到站点 IPsec （IKEv2）隧道的虚拟专用网络（VPN）网关  
  
-   部署并使用通用路由封装（GRE）网关。  
  
-   使用边界网关协议（BGP）在站点之间部署和配置动态路由和传输路由。  
  
-   为第3层和站点到站点网关配置 M + N 冗余，并为 BGP 路由配置。  
  
-   使用网络控制器指定虚拟网络和网络接口上的 Acl。  
  
有关详细信息，请参阅[网络功能虚拟化](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)。  
  
### <a name="bkmk_netp"></a>网络平台方案

对于本部分中的方案，Windows Server 网络团队支持使用任何 Windows Server 2016 认证驱动程序。 请与网络接口卡 \(NIC @ no__t 制造商联系，以确保具有最新的驱动程序更新。
  
利用网络平台方案，你可以：  
  
-   使用汇聚 NIC 结合使用单个网络适配器的 RDMA 和以太网通信。  
  
-   使用 Packet Direct 创建低延迟的数据路径，在 Hyper-v 虚拟交换机和单个网络适配器中启用。  
  
-   配置设置以在多达两个网络适配器之间分散 SMB 直通和 RDMA 流量流。  
  
有关详细信息，请[参阅远程直接内存&#40;访问&#41; RDMA 和交换机嵌入式&#40;组合&#41;集](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。  
  
#### <a name="bkmk_switch"></a>Hyper-v 虚拟交换机方案

Hyper-v 虚拟交换机方案使你能够：  
  
-   使用远程直接内存访问（RDMA） vNIC 创建 Hyper-v 虚拟交换机  
  
-   使用交换机嵌入式组合（SET）和 RDMA Vnic 创建 Hyper-v 虚拟交换机  
  
-   在 Hyper-v 虚拟交换机中创建集团队  
  
-   使用 Windows PowerShell 命令管理集团队  
  
有关详细信息，请[参阅远程直接内存&#40;访问&#41; RDMA 和交换机嵌入式&#40;组合&#41;集](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>DNS 服务器方案

DNS 服务器方案使你能够：  
  
-   使用 DNS 策略指定基于地理位置的流量管理  
  
-   使用 DNS 策略配置 split 大脑 DNS  
  
-   使用 DNS 策略对 DNS 查询应用筛选器  
  
-   使用 DNS 策略配置应用程序负载平衡  
  
-   根据一天中的时间指定智能 DNS 响应  
  
-   配置 DNS 区域传送策略  
  
-   在 Active Directory 域服务（AD DS）集成的区域上配置 DNS 服务器策略  
  
-   配置响应速率限制  
  
-   指定基于 DNS 的命名实体身份验证（窗格会）  
  
-   在 DNS 中配置对未知记录的支持  
  
有关详细信息，请参阅[Windows server 2016 中的 Dns 客户端中的新增功能](dns/What-s-New-in-DNS-Client.md)和[windows SERVER 2016 中 dns 服务器的](dns/What-s-New-in-DNS-Server.md)新增功能。  
  
### <a name="bkmk_ipam"></a>具有 DHCP 和 DNS 的 IPAM 方案

IPAM 方案使你能够：  
  
-   跨多个联合 Active Directory 林中发现和管理 DNS 和 DHCP 服务器以及 IP 寻址  
  
-   使用 IPAM 集中管理 DNS 属性，包括区域和资源记录。  
  
-   定义粒度基于角色的访问控制策略，并委派 IPAM 用户或用户组来管理指定的 DNS 属性集。  
  
-   使用适用于 IPAM 的 Windows PowerShell 命令自动执行 DHCP 和 DNS 的访问控制配置。  
  
    有关详细信息，请参阅[管理 IPAM](technologies/ipam/Manage-IPAM.md)。  
  
### <a name="bkmk_nicteam"></a>NIC 组合方案

NIC 组合方案使你能够：  
  
-   在支持的配置中创建 NIC 组  
  
-   删除 NIC 组  
  
-   使用受支持的配置将网络适配器添加到 NIC 组  
  
-   删除 NIC 组中的网络适配器  
  
> [!NOTE]  
> 在 Windows Server 2016 中，可以在 Hyper-v 中使用 NIC 组合，但在某些情况下，在创建 NIC 组时，可能不会在基础网络适配器上自动启用虚拟机队列（VMQ）。 如果出现这种情况，你可以使用以下 Windows PowerShell 命令，以确保在 NIC 组成员适配器上启用 VMQ： `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

有关详细信息，请参阅[NIC 组合](technologies/nic-teaming/NIC-Teaming.md)。 

### <a name="bkmk_set"></a>切换嵌入组合 \(SET @ no__t-2 方案

SET 是一种备用 NIC 组合解决方案，可用于在 Windows Server 2016 中包含 Hyper-v 的环境和软件定义的网络（SDN）堆栈。 将一些 NIC 组合功能集成到 Hyper-v 虚拟交换机。 

有关详细信息，请参阅[远程直接内存访问（RDMA）和交换机嵌入式组合（SET）](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>不支持的网络方案  
Windows Server 2016 不支持以下网络方案。  
  
-   基于 VLAN 的租户虚拟网络。  
  
-   是或覆盖区中不支持 IPv6。  
  


