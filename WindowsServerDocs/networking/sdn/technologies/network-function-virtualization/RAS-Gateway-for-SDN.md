---
title: SDN RAS 网关
description: 你可以使用本主题以了解 RAS 关，这是基于软件，multitenant、Windows Server 2016 中的边框网关协议 (BGP) 支持路由器。
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
ms.openlocfilehash: 052911dcd52df82ef4e259de0c64078c54f00195
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-for-sdn"></a>SDN RAS 网关

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解 RAS 关，这是一个软件基于、租户，边框网关协议 (BGP) 支持路由器设计云服务提供商 (Csp) 和主机使用 Hyper-V 网络虚拟化多个租户虚拟网络的企业的 Windows Server 2016。  
  
> [!NOTE]  
> 本主题中，除了以下 RAS 网关的主题都可用。  
>   
> -   [什么是 RAS 网关中的新增功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [RAS 网关部署建筑](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [RAS 网关高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [边框网关协议 & #40;BGP & #41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [BGP Windows PowerShell 命令参考](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
在 Windows Server 2016，RAS 网关传送网络之间的物理网络和 VM 网络资源，无论资源位于何处的交通。 你可以使用 RAS 网关之间的物理和虚拟网络相同的物理位置，或在 Internet 上的许多不同的物理位置的路线网络流量。  
  
多租赁是云基础结构，以支持虚拟机的工作负载的多个租户，还将它们彼此，隔离时运行的相同基础结构的所有工作负载的能力。 个别租户多个工作负载可以互连和可管理远程，但这些系统不执行与其他租户的工作负载互连也可以其他租户远程管理它们。  
  
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>安装 SDN RAS 网关先决条件  
你无法使用的 Windows 界面时你希望用于 SDN 租户型部署 RAS 网关安装远程访问。 相反，你必须使用 Windows PowerShell。  
  
但你可以通过使用 Windows PowerShell 安装 RAS 网关之前，你必须使用 Windows PowerShell 添加**远程访问**Windows 功能。 若要执行此操作，请在 Windows PowerShell 提示符下运行以下命令。  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
此命令添加**远程访问**功能和功能的 Windows PowerShell 命令。  
  
你在添加之后**远程访问**到服务器中,，你可以安装远程访问作为 RAS 网关对租户模式并边框网关协议 (BGP)。  
  
有关详细信息，请参阅 Windows PowerShell 参考主题[安装远程访问](https://technet.microsoft.com/library/hh918408.aspx)。  
  
## <a name="ras-gateway-features"></a>RAS 网关功能  
以下是在 Windows Server 2016 RAS 网关功能。 你可以将部署 RAS 网关高可用性池中一次使用所有这些功能。  
  
-   **站点的 VPN**。 此 RAS 网关功能将允许你使用站点的 VPN 连接来连接 Internet 的不同的物理位置的两个网络。 对于主机他们数据中心中的许多租户的 Csp，RAS 网关提供租户网关解决方案，以允许你租户访问，以及通过从远程的站点的站点的 VPN 连接管理他们的资源并允许您的数据中心中的虚拟资源其物理网络之间网络流量。  
  
-   **点到站点 VPN**。 此 RAS 网关功能允许远程连接到你的组织的网络，组织员工或管理员。  对于租户部署，租户网络管理员可以使用点到站点的 VPN 连接来访问虚拟网络资源，在 CSP 数据中心。  
  
-   **隧道 GRE**。 通用路由封装 (GRE) 基于隧道启用租户虚拟网络和外部网络之间的连接。 因为 GRE 协议是轻型和支持 GRE 是适用于大多数网络的设备适合隧道不需要的数据加密变得。 本主题中所述，GRE 支持站点 (S2S) 隧道解决租户虚拟网络和租户外部网络使用多租户网关之间的转移的问题。  
  
-   **动态路由边框网关协议 (BGP)**。 BGP 减少在路由器上的路线手动配置需要，因为它是动态路由协议，并自动学习通过使用站点的 VPN 连接来连接的站点之间的路线。 如果你的组织有多个网站连接使用 BGP 启用路由器，如 RAS 网关，BGP 允许路由器自动计算和使用有效彼此在发生网络中断或失败的路线。 有关详细信息，请参阅[RFC 4271](https://tools.ietf.org/html/rfc4271)。  
  

  


