---
title: Windows Server 2016 中的 GRE 隧道
description: 你可以使用本主题来了解 Windows Server 2016 中 RAS 网关的通用路由封装（GRE）隧道功能的更新。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: df2023bf-ba64-481e-b222-6f709edaa5c1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: be57bc0ce1b509c49f269618765c79f380fd3b12
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404679"
---
# <a name="gre-tunneling-in-windows-server-2016"></a>Windows Server 2016 中的 GRE 隧道

>适用于：Windows Server（半年频道）、Windows Server 2016

Windows Server 2016 为 RAS 网关提供 \(GRE @ no__t 隧道功能的通用路由封装的更新。  
  
GRE 是一种轻型隧道协议，可以在 Internet 协议网间上的虚拟点对点链路内封装各种网络层协议。 Microsoft GRE 实现可以封装 IPv4 和 IPv6。  
  
在许多情况下，GRE 隧道非常有用，因为：  
  
-   它们是轻型和 RFC 2890 兼容的，使其可与各种供应商设备互操作  
  
-   可以将边界网关协议 \(BGP @ no__t 用于动态路由  
  
-   可以配置 GRE 多租户 RAS 网关，以便与软件定义的网络一起使用 \(SDN @ no__t-1
  
-   你可以使用 System Center Virtual Machine Manager 来管理 GRE @ no__t-0based RAS 网关
  
-   在配置为 GRE RAS 网关的6核虚拟机上，最多可实现 2.0 Gbps 吞吐量
  
-   单个网关支持多种连接模式  
  
基于 GRE 的隧道可在租户虚拟网络与外部网络之间实现连接。 由于 GRE 协议是轻型协议，因此大多数网络设备上都提供对 GRE 的支持，因此它成为不需要数据加密的隧道的理想选择。 

站点到站点（S2S）隧道中的 GRE 支持可解决租户虚拟网络与使用多租户网关的租户外部网络之间的转发问题，如本主题后面所述。  
  
GRE 隧道功能旨在满足以下要求：  
  
-   宿主提供程序必须能够创建用于转发的虚拟网络，而不会修改物理交换机配置。  
  
-   宿主提供程序必须能够将子网添加到其面向外部的网络，而不必在其基础结构中修改物理交换机的配置。  
使用 GRE 隧道功能，可以通过 Microsoft 技术在服务产品中实现软件定义的网络，从而为托管服务提供商提供多种主要方案。  
  
以下是一些示例方案：  
  
-   [从租户虚拟网络访问到租户物理网络](#BKMK_Access)  
  
-   [高速连接](#BKMK_Speed)  
  
-   [与基于 VLAN 的隔离集成](#BKMK_Integration)  
  
-   [访问共享资源](#BKMK_Shared)  
  
-   [向租户的第三方设备服务](#BKMK_thirdparty)  
  
## <a name="key-scenarios"></a>关键方案

下面是 GRE 隧道功能寻址的关键方案。  
  
### <a name="BKMK_Access"></a>从租户虚拟网络访问到租户物理网络

此方案提供了一种可缩放的方式，可提供从租户虚拟网络到托管服务提供商的本地租户物理网络的访问。 在多租户网关上建立 GRE 隧道终结点，另一个 GRE 隧道终结点是在物理网络上的第三方设备上建立的。 在虚拟网络中的虚拟机与物理网络上的第三方设备之间路由第3层流量。  
  
![GRE 隧道连接主机物理网络和租户虚拟网络](../../media/gre-tunneling-in-windows-server/GRE_.png)  
  
### <a name="BKMK_Speed"></a>高速连接

此方案可提供一种可缩放的方式，以提供从租户本地网络到托管服务提供商网络中的虚拟网络的高速连接。 租户通过多协议标签交换（MPLS）连接到服务提供商网络，在该网络中，将在托管服务提供商的边缘路由器与租户的虚拟网络之间建立 GRE 隧道。  
  
![连接租户企业 MPLS 网络和租户虚拟网络的 GRE 隧道](../../media/gre-tunneling-in-windows-server/GRE-.png)  
  
### <a name="BKMK_Integration"></a>与基于 VLAN 的隔离集成

此方案使你可以将基于 VLAN 的隔离与 Hyper-v 网络虚拟化集成。 托管提供商网络上的物理网络包含使用基于 VLAN 的隔离的负载均衡器。 多租户网关在物理网络上的负载均衡器和虚拟网络上的多租户网关上建立 GRE 隧道。  
  
可以在源和目标之间建立多个隧道，并使用 GRE 密钥来区分隧道。  
  
![连接租户虚拟网络的多个 GRE 隧道](../../media/gre-tunneling-in-windows-server/GRE-VLANIsolation.png)  
  
### <a name="BKMK_Shared"></a>访问共享资源

此方案允许访问位于宿主提供商网络中的物理网络上的共享资源。  
  
你可能会有一个共享服务，该服务位于托管提供程序网络中要与多个租户虚拟网络共享的物理网络上的某个服务器上。  
  
具有非重叠子网的租户网络通过 GRE 隧道访问公用网络。 单个租户网关在 GRE 隧道之间路由，从而将数据包路由到相应的租户网络。  
  
在这种情况下，可以使用第三方硬件设备替换单个租户网关。  
  
![使用多个隧道连接多个虚拟网络的单租户网关](../../media/gre-tunneling-in-windows-server/GRE-SharedResource.png)  
  
### <a name="BKMK_thirdparty"></a>向租户的第三方设备服务

此方案可用于将第三方设备（如硬件负载平衡器）集成到租户虚拟网络流量流中。 例如，源自企业站点的流量通过 S2S 隧道传递到多租户网关。 流量通过 GRE 隧道路由到负载均衡器。 负载均衡器将流量路由到企业虚拟网络上的多个虚拟机。 对于在虚拟网络中具有可能重叠的 IP 地址的其他租户，也会发生相同的情况。 网络流量在负载均衡器上使用 Vlan 隔离，适用于支持 Vlan 的所有第3层设备。  
  
![将虚拟网络连接到第三方设备的多个 GRE 隧道](../../media/gre-tunneling-in-windows-server/GREThirdParty.png)  
  
## <a name="configuration-and-deployment"></a>配置和部署

GRE 隧道在 S2S 接口内作为附加协议公开。 它的实现方式类似于以下网络博客中所述的 IPSec S2S 隧道：[具有 Windows Server 2012 R2 的多租户站点到站点（S2S） VPN 网关](https://blogs.technet.com/b/networking/archive/2013/09/29/multi-tenant-site-to-site-s2s-vpn-gateway-with-windows-server-2012-r2.aspx)  
  
请参阅以下主题，了解部署网关的示例，包括 GRE 隧道网关：  
  
[使用脚本部署软件定义的网络基础结构](../../../networking/sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
  
## <a name="more-information"></a>详细信息

有关部署 S2S 网关的详细信息，请参阅以下主题：  
  
-   [RAS 网关](RAS-Gateway.md)  
  
-   [边界网关协议&#40;BGP&#41;](../bgp/Border-Gateway-Protocol-BGP.md)  
  
-   [New！Windows Server 2012 R2 RAS 多租户网关部署指南 @ no__t-0  
  
-   [通过 RAS 多租户网关部署边界网关协议（BGP）](https://blogs.technet.com/b/wsnetdoc/archive/2014/04/03/deploy-border-gateway-protocol-bgp-with-the-RAS-multitenant-gateway.aspx)  
  


