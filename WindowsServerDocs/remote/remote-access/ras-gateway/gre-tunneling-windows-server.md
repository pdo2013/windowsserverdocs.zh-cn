---
title: Windows Server 2016 中的 GRE 隧道
description: 可以使用本主题以了解更新到通用路由封装 (GRE) 隧道功能的 Windows Server 2016 中的 RAS 网关。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: df2023bf-ba64-481e-b222-6f709edaa5c1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0ec077ad5e97edd3db7d1dc4e662bb191f7885b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887858"
---
# <a name="gre-tunneling-in-windows-server-2016"></a>Windows Server 2016 中的 GRE 隧道

>适用于：Windows 服务器 （半年频道），Windows Server 2016

Windows Server 2016 提供更新到基本路由封装\(GRE\) RAS 网关的隧道功能。  
  
GRE 是一种轻型隧道协议，可以在 Internet 协议网间上的虚拟点对点链路内封装各种网络层协议。 Microsoft GRE 实现可以封装 IPv4 和 IPv6。  
  
GRE 隧道可在许多情况下由于：  
  
-   它们是轻量和 RFC 2890 兼容，使其与供应商的各种设备可互操作  
  
-   可以使用边界网关协议\(BGP\)用于动态路由  
  
-   可以使用软件定义的网络配置为使用 GRE 多租户 RAS 网关\(SDN\)
  
-   可以使用 System Center Virtual Machine Manager 来管理 GRE\-基于 RAS 网关
  
-   您可以获得配置 GRE RAS 网关为 6 核虚拟机上的最大 2.0 Gbps 吞吐量
  
-   单个网关支持多个连接模式  
  
基于 GRE 的隧道可在租户虚拟网络与外部网络之间实现连接。 由于 GRE 协议是轻型和支持 GRE 是可在大多数网络设备上变得不需要的数据加密的隧道的理想之选。 

站点到站点 (S2S) 隧道中的 GRE 支持可解决租户虚拟网络与使用多租户网关的租户外部网络之间转发的问题，如本主题后面所述。  
  
GRE 隧道功能旨在满足以下要求：  
  
-   宿主提供程序必须能够创建的要转发的虚拟网络，而无需修改物理交换机配置。  
  
-   宿主提供程序必须能够将子网添加到其面向外部的网络，而无需修改其基础结构中的物理交换机的配置。  
GRE 隧道功能启用，或增强了几个主要方案的托管服务提供程序使用 Microsoft 技术来实现其服务产品/服务中的软件定义的网络。  
  
以下是一些示例方案：  
  
-   [来自租户虚拟网络，租户的物理网络的访问](#BKMK_Access)  
  
-   [高速连接](#BKMK_Speed)  
  
-   [使用基于 VLAN 的隔离的集成](#BKMK_Integration)  
  
-   [访问共享资源](#BKMK_Shared)  
  
-   [向租户的第三方设备的服务](#BKMK_thirdparty)  
  
## <a name="key-scenarios"></a>关键方案

以下是 GRE 隧道功能地址的关键方案。  
  
### <a name="BKMK_Access"></a>来自租户虚拟网络，租户的物理网络的访问

此方案，可缩放的方式来提供从租户虚拟网络，租户位于托管服务提供商的物理网络的访问权限。 对多租户网关建立的 GRE 隧道终结点，其他 GRE 隧道终结点建立的第三方设备上的物理网络。 第 3 层流量的虚拟网络中的虚拟机和物理网络上的第三方设备之间进行路由。  
  
![连接主机物理网络和租户虚拟网络的 GRE 隧道](../../media/gre-tunneling-in-windows-server/GRE_.png)  
  
### <a name="BKMK_Speed"></a>高速连接

此方案，可缩放的方式来提供从租户的本地网络到虚拟网络位于托管服务提供商网络中的高速连接。 租户连接到服务提供商网络通过多协议标签交换 (MPLS)，其中托管服务提供商边缘路由器和多租户网关到租户的虚拟网络之间建立的 GRE 隧道。  
  
![租户企业 MPLS 网络和租户虚拟网络连接的 GRE 隧道](../../media/gre-tunneling-in-windows-server/GRE-.png)  
  
### <a name="BKMK_Integration"></a>使用基于 VLAN 的隔离的集成

这种情况下，可将基于 VLAN 的隔离与 HYPER-V 网络虚拟化集成。 托管提供商网络上的物理网络包含负载均衡器使用基于 VLAN 的隔离。 多租户网关建立物理网络上的负载均衡器与虚拟网络上的多租户网关之间的 GRE 隧道。  
  
可以在源和目标之间建立多个隧道和 GRE 密钥用于区分隧道。  
  
![多个 GRE 隧道连接租户虚拟网络](../../media/gre-tunneling-in-windows-server/GRE-VLANIsolation.png)  
  
### <a name="BKMK_Shared"></a>访问共享资源

这种情况下，可访问位于托管提供商网络上的物理网络上的共享的资源。  
  
您可能想要与多个租户虚拟网络共享宿主提供程序网络中的物理网络上的服务器上的共享的服务。  
  
具有非重叠子网的租户网络通过 GRE 隧道访问常用的网络。 单租户网关将路由之间 GRE 隧道，因此数据包路由到相应的租户网络。  
  
在此方案中，可以由第三方硬件设备替换单个租户网关。  
  
![单租户网关使用多个隧道连接多个虚拟网络](../../media/gre-tunneling-in-windows-server/GRE-SharedResource.png)  
  
### <a name="BKMK_thirdparty"></a>向租户的第三方设备的服务

这种情况下可将第三方设备 （如硬件负载均衡器） 集成到租户虚拟网络流量流。 例如，从企业站点发起的流量通过 S2S 隧道传递到多租户网关。 流量通过 GRE 隧道路由到负载均衡器。 负载均衡器将流量路由到企业的虚拟网络上的多个虚拟机。 相同的操作会针对具有可能重叠的 IP 地址在虚拟网络中的另一个租户。 网络流量隔离在使用 Vlan，则负载均衡器和适用于所有支持 Vlan 的第 3 层设备。  
  
![多个 GRE 隧道连接到第三方设备的虚拟网络](../../media/gre-tunneling-in-windows-server/GREThirdParty.png)  
  
## <a name="configuration-and-deployment"></a>配置和部署

GRE 隧道公开为 S2S 界面中的其他协议。 它是作为以下网络博客中所述的 IPSec S2S 隧道以类似的方式实现：[多租户站点到站点 (S2S) VPN 网关与 Windows Server 2012 R2](https://blogs.technet.com/b/networking/archive/2013/09/29/multi-tenant-site-to-site-s2s-vpn-gateway-with-windows-server-2012-r2.aspx)  
  
请参阅以下主题中有关部署网关，包括 GRE 隧道网关的示例：  
  
[部署软件定义的网络基础结构使用脚本](../../../networking/sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
  
## <a name="more-information"></a>详细信息

有关部署 S2S 网关的详细信息，请参阅以下主题：  
  
-   [RAS 网关](RAS-Gateway.md)  
  
-   [边界网关协议&#40;BGP&#41;](../bgp/Border-Gateway-Protocol-BGP.md)  
  
-   [新增功能！Windows Server 2012 R2 RAS 多租户网关部署指南](https://blogs.technet.com/b/wsnetdoc/archive/2014/03/26/new-windows-server-2012-r2-RAS-multitenant-gateway-deployment-guide.aspx)  
  
-   [部署 RAS 多租户网关的边界网关协议 (BGP)](https://blogs.technet.com/b/wsnetdoc/archive/2014/04/03/deploy-border-gateway-protocol-bgp-with-the-RAS-multitenant-gateway.aspx)  
  


