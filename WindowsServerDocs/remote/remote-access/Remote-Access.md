---
title: 远程访问
description: 本主题概述了 Windows Server 2016 中的远程访问服务器角色。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/18/2018
ms.openlocfilehash: d2fa9c82c4cab05b2a60916fee3f09c1ea48a472
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388909"
---
# <a name="remote-access"></a>远程访问

>适用于：Windows Server (半年频道), Windows Server 2016, Windows Server 2012 R2, Windows 10

远程访问指南提供了有关 Windows Server 2016 中的远程访问服务器角色的概述，并涵盖了以下主题：

- [Always On VPN 部署指南](vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)
- [边界网关协议&#40;BGP&#41;](bgp/Border-Gateway-Protocol-BGP.md)
- [RAS 网关](ras-gateway/RAS-Gateway.md) 
- [远程访问服务器角色文档](ras/Remote-Access-Server-Role-Documentation.md)
- [用于 SDN 的 RAS 网关](../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)
- [虚拟专用网 (VPN)](vpn/vpn-top.md)
 
有关其他网络技术的详细信息，请参阅[Windows Server 2016 中的网络](https://docs.microsoft.com/windows-server/networking/networking)。

远程访问服务器角色是以下相关网络访问技术的逻辑分组：[远程访问服务（RAS）](#bkmk_da)、[路由](#bkmk_rras)和[Web 应用程序代理](#bkmk_proxy)。 这些技术是远程访问服务器角色的 *角色服务* 。 使用 "**添加角色和功能向导**" 或 Windows PowerShell 安装远程访问服务器角色时，可以安装这三个角色服务中的一个或多个。

>[!IMPORTANT]
>请勿尝试在 Microsoft Azure 的虚拟机上部署远程访问 \(VM @ no__t-1。 不支持在 Microsoft Azure 中使用远程访问。 不能在 Azure VM 中使用远程访问在 Windows Server 2016 或更早版本的 Windows Server 中部署 VPN、DirectAccess 或任何其他远程访问功能。 有关详细信息，请参阅[Microsoft Azure 虚拟机的 Microsoft 服务器软件支持](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。

## <a name="bkmk_da"></a>远程访问服务 \(RAS @ no__t-2-RAS 网关

安装**DirectAccess 和 VPN （RAS）** 角色服务时，将 \(**RAS 网关**部署远程访问服务网关 \)。 你可以使用单租户 RAS 网关虚拟专用网络 \(VPN @ no__t 服务器、多租户 RAS 网关 VPN 服务器以及 DirectAccess 服务器部署 RAS 网关。

- **RAS 网关-单个租户**。 通过使用 RAS 网关，你可以部署 VPN 连接，以便为最终用户提供对组织网络和资源的远程访问权限。 如果客户端运行的是 Windows 10，则可以部署 Always On VPN，无论何时远程计算机连接到 Internet，都可以在客户端与组织网络之间保持持久性连接。 使用 RAS 网关，还可以在两个服务器之间创建站点到站点 VPN 连接，例如在主办公室与分支机构之间，并使用网络地址转换 \(NAT @ no__t-1，使网络中的用户可以访问外部资源（如 Internet）。 此外，RAS 网关还支持边界网关协议（BGP），在远程办公室位置也有支持 BGP 的边缘网关时，提供动态路由服务。

- **RAS 网关-多租户**。 使用超级 @ no__t-0V 网络虚拟化时，可以将 RAS 网关部署为多租户、基于软件的边缘网关和路由器，或者使用虚拟局域网部署的 VM 网络 \(VLANs @ no__t。 使用 RAS 网关，云服务提供商 \(CSPs @ no__t，企业可以在虚拟网络与物理网络（包括 Internet）之间进行数据中心和云网络流量路由。 使用 RAS 网关，租户可以使用点到站点 VPN 连接从任何位置访问数据中心内的 VM 网络资源。 你还可以向租户提供其远程站点与 CSP 数据中心之间的站点到站点 VPN 连接。 此外，还可以为 RAS 网关配置动态路由的 BGP 网关，还可以启用网络地址转换 \(NAT @ no__t，为 VM 网络上的 Vm 提供 Internet 访问。

>[!IMPORTANT]
> Windows Server 2012 R2 还提供了具有多租户功能的 RAS 网关。

- **ALWAYS ON VPN**。 Always On VPN 使远程用户能够安全地访问内部网络上的共享资源、intranet 网站和应用程序，而无需连接到 VPN。 

有关详细信息，请参阅[RAS 网关](ras-gateway/RAS-Gateway.md)和[边界网关协议（BGP）](bgp/Border-Gateway-Protocol-BGP.md)。

## <a name="bkmk_rras"></a>传递

可以使用远程访问来路由局域网上子网之间的网络流量。 路由支持网络地址转换（NAT）路由器、运行 BGP 的 LAN 路由器、路由信息协议（RIP）和使用 Internet 组管理协议（IGMP）的支持多播的路由器。 作为功能齐全的路由器，你可以在运行 Hyper-v 的计算机上的服务器计算机或虚拟机（VM）上部署 RAS。

若要将远程访问安装为 LAN 路由器，请使用服务器管理器中的 "添加角色和功能向导"，然后选择 "**远程访问**服务器" 角色和 "**路由**角色服务"。或者，在 Windows PowerShell 提示符下键入以下命令，然后按 ENTER。

```  
Install-RemoteAccess -VpnType RoutingOnly
```  

## <a name="bkmk_proxy"></a>Web 应用程序代理

Web 应用程序代理是 Windows Server 2016 中的远程访问角色服务。 Web 应用程序代理为企业网络中的 Web 应用程序提供反向代理功能，使任一设备上的用户能够从企业网络外部访问这些 Web 应用程序。 Web 应用程序代理使用 Active Directory 联合身份验证服务（AD FS）预先验证对 web 应用程序的访问权限，并将其充当 AD FS 代理。

若要将远程访问安装为 Web 应用程序代理，请使用服务器管理器中的 "添加角色和功能向导"，然后选择 "**远程访问**服务器" 角色和 " **Web 应用程序代理**" 角色服务;或者，在 Windows PowerShell 提示符下键入以下命令，然后按 ENTER。  

```  
Install-RemoteAccess -VpnType SstpProxy  
```  

有关详细信息，请参阅[Web 应用程序代理](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server)。


---