---
title: 用于 SDN 的 RAS 网关
description: 你可以使用本主题来了解 RAS 网关，该网关是 Windows Server 2016 中基于软件、多租户、边界网关协议（BGP）的路由器。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a32357a5-ab1a-4a4c-848a-7a4ed65b1921
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 20fc19dc31ee612de0a736bfe989f930a9afa202
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405864"
---
# <a name="ras-gateway-for-sdn"></a>用于 SDN 的 RAS 网关

>适用于：Windows Server （半年频道），Windows Server 2016 # # SDN 的 RAS 网关  


RAS 网关是为云服务提供商（Csp）和使用 Hyper-v 网络虚拟化托管多个租户虚拟网络的企业而设计的基于软件、多租户、边界网关协议（BGP）的路由器。 RAS 网关用于在物理网络与 VM 网络资源之间路由网络流量，而不考虑位置。 您可以将网络流量路由到同一物理位置或多个不同的位置。   

多租户是云基础结构的功能，用于支持多个租户的虚拟机工作负荷，但将它们彼此隔离，而所有工作负荷在同一基础结构上运行。 单个租户的多个工作负载可以互连和接受远程管理，但这些系统不与其他租户的工作负载互连，其他租户也不能远程管理它们。

  
> [!NOTE]  
> 除了本主题之外，还提供了以下 RAS 网关主题。  
>   
> -   [RAS 网关中的新增功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [RAS 网关部署体系结构](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [RAS 网关高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [边界网关协议&#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [BGP Windows PowerShell 命令参考](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
    
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>安装用于 SDN 的 RAS 网关的先决条件  
如果要在多租户模式下部署 RAS 网关以便与 SDN 一起使用，则不能使用 Windows 界面来安装远程访问。 相反，你必须使用 Windows PowerShell。  
  
但在可以使用 Windows PowerShell 安装 RAS 网关之前，必须使用 Windows PowerShell 来添加**RemoteAccess** Windows 功能。 为此，请在 Windows PowerShell 提示符下运行以下命令。  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
此命令为功能添加**RemoteAccess**功能和 Windows PowerShell 命令。  
  
将**RemoteAccess**添加到服务器后，可以使用多租户模式将远程访问安装为 RAS 网关，并边界网关协议（BGP）。  
  
有关详细信息，请参阅 Windows PowerShell 参考主题[安装-远程访问](https://technet.microsoft.com/library/hh918408.aspx)。  
  
## <a name="ras-gateway-features"></a>RAS 网关功能  
下面是 Windows Server 2016 中的 RAS 网关功能。 可以在使用所有这些功能的高可用性池中同时部署 RAS 网关。  
  
-   **站点到站点 VPN**。 此 RAS 网关功能允许你使用站点到站点 VPN 连接在 Internet 上的不同物理位置连接两个网络。 对于在数据中心托管许多租户的 Csp，RAS 网关提供多租户网关解决方案，使租户能够通过站点到站点 VPN 连接访问和管理其资源，并允许网络流量在数据中心及其物理网络中的虚拟资源。  
  
-   **点到站点 VPN**。 此 RAS 网关功能允许组织员工或管理员从远程位置连接到组织的网络。  对于多租户部署，租户网络管理员可以使用点到站点 VPN 连接来访问 CSP 数据中心的虚拟网络资源。  
  
-   **GRE 隧道**。 基于通用路由封装（GRE）的隧道可在租户虚拟网络与外部网络之间实现连接。 因为 GRE 协议是轻型的，并且大多数网络设备上提供 GRE 支持，所以它成为用于无需数据加密的隧道的理想选择。 站点到站点（S2S）隧道中的 GRE 支持可解决租户虚拟网络与使用多租户网关的租户外部网络之间的转发问题，如本主题后面所述。  
  
-   **动态路由边界网关协议（BGP）** 。 BGP 可减少对在路由器上进行手动路由配置的需要，因为它是一种动态路由协议，可自动了解使用站点到站点 VPN 连接进行连接的站点之间的路由。 如果你的组织有多个使用启用了 BGP 的路由器（如 RAS 网关）连接的站点，则在发生网络中断或故障时，BGP 允许路由器自动计算并使用有效的路由。 有关详细信息，请参阅[RFC 4271](https://tools.ietf.org/html/rfc4271)。  
  

  


