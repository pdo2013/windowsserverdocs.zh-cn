---
title: 远程访问
description: 本主题概述了 Windows Server 2016 中的远程访问服务器角色。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/18/2018
ms.openlocfilehash: faf12ad22678fa58ea933613759e3e8414966bca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818358"
---
# <a name="remote-access"></a>远程访问

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows 10

远程访问指南您提供 Windows Server 2016 中的远程访问服务器角色的概述，包括以下主题：

- [Always On VPN 部署指南](vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)
- [边界网关协议&#40;BGP&#41;](bgp/Border-Gateway-Protocol-BGP.md)
- [RAS 网关](ras-gateway/RAS-Gateway.md) 
- [远程访问服务器角色文档](ras/Remote-Access-Server-Role-Documentation.md)
- [用于 SDN 的 RAS 网关](../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)
- [虚拟专用网络 (VPN)](vpn/vpn-top.md)
 
有关其他网络技术的详细信息，请参阅[Windows Server 2016 中的网络](https://docs.microsoft.com/windows-server/networking/networking)。

远程访问服务器角色是这些相关的网络访问技术的逻辑分组：[远程访问服务 (RAS)](#bkmk_da)，[路由](#bkmk_rras)，和[Web 应用程序代理](#bkmk_proxy)。 这些技术是远程访问服务器角色的 *角色服务* 。 当安装远程访问服务器角色具有**添加角色和功能向导**或 Windows PowerShell 中，可以安装一个或多个这些三个角色服务。

>[!IMPORTANT]
>请不要尝试在虚拟机上部署远程访问\(VM\)在 Microsoft Azure 中。 不支持在 Microsoft Azure 中使用远程访问。 您不能用于远程访问在 Azure VM 中部署 VPN、 DirectAccess、 或 Windows Server 2016 或 Windows Server 的早期版本中的任何其他远程访问功能。 有关详细信息，请参阅[对 Microsoft Azure 虚拟机的 Microsoft 服务器软件支持](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。

## <a name="bkmk_da"></a>远程访问服务\(RAS\) -RAS 网关

当你安装**DirectAccess 和 VPN (RAS)** 角色服务，您要部署远程访问服务网关\( **RAS 网关**\)。 你可以部署 RAS 网关的单个租户 RAS 网关的虚拟专用网络\(VPN\)服务器、 多租户 RAS 网关 VPN 服务器，并为 DirectAccess 服务器。

- **RAS 网关的单个租户**。 通过使用 RAS 网关，你可以部署 VPN 连接，以便向最终用户提供对组织的网络和资源的远程访问。 如果你的客户端运行 Windows 10，你可以部署 Always On VPN，可维护客户端和你的组织网络之间的持续性连接，只要远程计算机连接到 Internet。 使用 RAS 网关，你还可以创建不同的位置，在两个服务器之间的站点到站点 VPN 连接如之间主办公室和分支机构，并使用网络地址转换\(NAT\) ，以便内部用户网络可以访问外部资源，如 Internet。 此外，RAS 网关支持边界网关协议 (BGP)，提供动态路由服务时远程机构位置还具有边缘网关支持 BGP。

- **RAS 网关-多租户**。 可以作为多租户、 基于软件的 edge 网关和路由器部署 RAS 网关，当使用超\-V 网络虚拟化，或者您需使用虚拟局域网部署的 VM 网络\(Vlan\)。 使用 RAS 网关，云服务提供商\(Csp\)和企业可以启用数据中心和云网络虚拟和物理网络，包括 Internet 之间路由流量。 与 RAS 网关，租户可以使用点使站点的 VPN 连接从任何位置访问其数据中心内的 VM 网络资源。 您还可以租户与其远程站点与 CSP 数据中心之间的站点到站点 VPN 连接。 此外，您可以使用 BGP 配置 RAS 网关，用于动态路由，并且你可以启用网络地址转换\(NAT\) VM 网络上的 Vm 提供 Internet 访问。

>[!IMPORTANT]
> RAS 网关与多租户功能也是 Windows Server 2012 R2 中提供的。

- **Always On VPN**。 Always On VPN 使远程用户能够安全地访问共享的资源、 网站、 intranet 和内部网络上的应用程序，无需连接到 VPN。 

有关详细信息，请参阅[RAS 网关](ras-gateway/RAS-Gateway.md)并[边界网关协议 (BGP)](bgp/Border-Gateway-Protocol-BGP.md)。

## <a name="bkmk_rras"></a>路由

在本地网络上的子网之间路由网络流量，可以使用远程访问。 路由的网络地址转换 (NAT) 路由器运行 BGP、 路由信息协议 (RIP) 和使用 Internet 组管理协议 (IGMP) 支持多播的路由器的 LAN 路由器提供支持。 作为使用全功能的路由器，你可以部署 RAS 服务器计算机上或作为正在运行 HYPER-V 的计算机上的虚拟机 (VM)。

若要为 LAN 路由器安装远程访问，请使用添加角色和功能向导在服务器管理器，然后选择**远程访问**服务器角色和**路由**角色服务; 或键入以下内容在 Windows PowerShell 提示符下，命令，然后按 ENTER。

```  
Install-RemoteAccess -VpnType RoutingOnly
```  

## <a name="bkmk_proxy"></a>Web 应用程序代理

Web 应用程序代理是 Windows Server 2016 中的远程访问角色服务。 Web 应用程序代理为企业网络中的 Web 应用程序提供反向代理功能，使任一设备上的用户能够从企业网络外部访问这些 Web 应用程序。 Web 应用程序代理预身份验证访问 web 应用程序使用 Active Directory 联合身份验证服务 (AD FS)，同时还充当 AD FS 代理。

若要安装 Web 应用程序代理远程访问，请使用添加角色和功能向导在服务器管理器，并选择**远程访问**服务器角色和**Web 应用程序代理**角色服务; 或在 Windows PowerShell 提示符下键入以下命令，然后按 ENTER。  

```  
Install-RemoteAccess -VpnType SstpProxy  
```  

有关详细信息，请参阅[Web 应用程序代理](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server)。


---