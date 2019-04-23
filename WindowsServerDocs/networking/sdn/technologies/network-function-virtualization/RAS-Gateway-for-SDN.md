---
title: 用于 SDN 的 RAS 网关
description: 可以使用本主题以了解 RAS 网关，它是基于软件的多租户、 Windows Server 2016 中的边界网关协议 (BGP) 支持路由器。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a32357a5-ab1a-4a4c-848a-7a4ed65b1921
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4f1ad0b3f0b5921a53faa8a45baae9f0b8711873
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856268"
---
# <a name="ras-gateway-for-sdn"></a>用于 SDN 的 RAS 网关

>适用于：Windows Server （半年频道） 用于 SDN 的 Windows Server 2016 的 # # RAS 网关  


RAS 网关是基于软件的多租户，支持边界网关协议 (BGP) 路由器设计的云服务提供商 (Csp) 和企业该主机使用的 HYPER-V 网络虚拟化的多个租户虚拟网络。 RAS 网关之间路由网络流量的物理网络和 VM 网络资源，而不考虑位置。 你可以路由相同的物理位置或多个不同位置的网络流量。   

多租户是云基础结构以支持多个租户的虚拟机工作负荷，尚未将其隔离，而同一基础结构上的所有工作负荷运行的能力。 单个租户的多个工作负载可以互连和接受远程管理，但这些系统不与其他租户的工作负载互连，其他租户也不能远程管理它们。

  
> [!NOTE]  
> 除了本主题，RAS 网关可用的主题如下。  
>   
> -   [什么是 RAS 网关中的新增功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [RAS 网关部署体系结构](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [RAS 网关高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [边界网关协议&#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [BGP Windows PowerShell 命令参考](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
    
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>安装适用于 SDN RAS 网关的先决条件  
不能使用 Windows 界面安装远程访问，当你想要部署在多租户模式下用于 SDN 的 RAS 网关。 相反，必须使用 Windows PowerShell。  
  
但可以通过使用 Windows PowerShell 安装 RAS 网关之前，必须使用 Windows PowerShell 添加**RemoteAccess** Windows 功能。 若要执行此操作，在 Windows PowerShell 提示符下运行以下命令。  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
此命令将添加**RemoteAccess**功能和功能的 Windows PowerShell 命令。  
  
添加后**RemoteAccess**到您的服务器中，你可以安装远程访问作为具有多组织模式下的 RAS 网关和边界网关协议 (BGP)。  
  
有关详细信息，请参阅 Windows PowerShell 参考主题[Install-remoteaccess](https://technet.microsoft.com/library/hh918408.aspx)。  
  
## <a name="ras-gateway-features"></a>RAS 网关功能  
以下是 Windows Server 2016 中的 RAS 网关功能。 你可以部署在一次使用所有这些功能的高可用性池的 RAS 网关。  
  
-   **站点到站点 VPN**。 此 RAS 网关功能，可通过使用站点到站点 VPN 连接通过 Internet 连接两个网络位于不同物理位置。 对于托管在自己的数据中心中的多个租户的 Csp，RAS 网关提供多租户网关解决方案，允许租户将访问和管理其资源通过站点到站点 VPN 连接从远程站点，这将允许之间的网络流量在你的数据中心和其物理网络中的虚拟资源。  
  
-   **点到站点 VPN**。 此 RAS 网关功能，组织员工或管理员就可以从远程位置连接到你组织的网络。  对于多租户部署，租户网络管理员可以使用点到站点 VPN 连接来访问在 CSP 数据中心的虚拟网络资源。  
  
-   **GRE 隧道**。 通用路由封装 (GRE) 基于隧道租户虚拟网络与外部网络之间实现连接。 因为 GRE 协议是轻型的，并且大多数网络设备上提供 GRE 支持，所以它成为用于无需数据加密的隧道的理想选择。 站点到站点 (S2S) 隧道中的 GRE 支持可解决租户虚拟网络与使用多租户网关的租户外部网络之间转发的问题，如本主题后面所述。  
  
-   **动态路由与边界网关协议 (BGP)**。 BGP 可减少对在路由器上进行手动路由配置的需要，因为它是一种动态路由协议，可自动了解使用站点到站点 VPN 连接进行连接的站点之间的路由。 如果你的组织有多个使用如 RAS 网关启用 BGP 的路由器连接的站点，BGP 允许自动计算并使用彼此发生网络中断或故障时的有效路由的路由器。 有关详细信息，请参阅[RFC 4271](https://tools.ietf.org/html/rfc4271)。  
  

  


