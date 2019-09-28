---
title: 针对 Windows Server 和 Windows 10 的始终启用 VPN 部署
description: 你可以使用此部署通过在 Windows Server 2016 或更高版本中使用远程访问为远程员工部署 Always On 虚拟专用网络（VPN）连接，并为 Windows 10 客户端计算机 Always On VPN 配置文件。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5eba89cf61354627b63bcdf2420c25e7a44e3d9a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388139"
---
# <a name="always-on-vpn-deployment-for-windows-server-and-windows-10"></a>Always On 适用于 Windows Server 和 Windows 10 的 VPN 部署

>适用于：Windows Server (半年频道), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**以前**远程访问 @ no__t-0<br>
- [**一个**了解 Always On VPN 功能和功能 @ no__t-0

Always On VPN 提供了一个用于远程访问的单内聚解决方案，并支持已加入域、未加入域（工作组）或已加入 Azure AD 的设备，甚至是个人拥有的设备。 使用 Always On VPN 时，连接类型不需要是用户独占或设备独占，可以是二者相结合。 例如，可以先启用设备身份验证，以便进行远程设备管理；然后启用用户身份验证，以便连接到公司内部站点和服务。

## <a name="prerequisites"></a>先决条件

你最有可能部署了可用于部署 Always On VPN 的技术。 除了 DC/DNS 服务器以外，Always On VPN 部署还需要 NPS （RADIUS）服务器、证书颁发机构（CA）服务器和远程访问（路由/VPN）服务器。 设置基础结构后，你必须注册客户端，然后通过多个网络更改安全地将客户端连接到本地。

- Active Directory 域基础结构，包括一个或多个域名系统（DNS）服务器。 内部和外部域名系统（DNS）区域都是必需的，这假设内部区域是外部区域的委派子域（例如，corp.contoso.com 和 contoso.com）。
- 基于 Active Directory 的公钥基础结构（PKI）和 Active Directory 证书服务（AD CS）。
- 用于安装网络策略服务器（NPS）的虚拟或物理服务器（现有或新）。 如果网络中已有 NPS 服务器，则可以修改现有的 NPS 服务器配置，而不是添加新的服务器。
- 作为 RAS 网关 VPN 服务器的远程访问，其中包含支持 IKEv2 VPN 连接和 LAN 路由的一小部分功能。
- 包含两个防火墙的外围网络。  确保你的防火墙允许 VPN 和 RADIUS 通信所需的流量正常运行。 有关详细信息，请参阅[ALWAYS ON VPN 技术概述](../always-on-vpn-technology-overview.md)。
- 外围网络上的物理服务器或虚拟机（VM），其中包含两个物理以太网网络适配器，可将远程访问作为 RAS 网关 VPN 服务器进行安装。 Vm 需要主机的虚拟 LAN （VLAN）。 
- Administrators 中的成员身份或同等身份是必需的最小值。
- 阅读本指南的 "规划" 部分，以确保在执行部署之前已准备好进行此部署。
- 查看适用于每种技术的设计和部署指南。 这些指南可帮助你确定部署方案是否提供组织网络所需的服务和配置。 有关详细信息，请参阅[ALWAYS ON VPN 技术概述](../always-on-vpn-technology-overview.md)。
- 选择用于部署 Always On VPN 配置的管理平台，因为 CSP 不特定于供应商。

>[!IMPORTANT]
>对于此部署，不要求您的基础结构服务器（例如运行 Active Directory 域服务、Active Directory 证书服务和网络策略服务器的计算机）运行的是 Windows Server 2016。 对于基础结构服务器以及运行远程访问的服务器，你可以使用 windows server 的早期版本，如 Windows Server 2012 R2。
>
>不要尝试在 Microsoft Azure 中的虚拟机（VM）上部署远程访问。 不支持在 Microsoft Azure 中使用远程访问（包括远程访问 VPN 和 DirectAccess）。 有关详细信息，请参阅[Microsoft Azure 虚拟机的 Microsoft 服务器软件支持](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。

## <a name="about-this-deployment"></a>关于此部署

提供的说明将指导你使用以下任意方案为运行 Windows 10 的远程客户端计算机部署远程访问，作为点到站点 VPN 连接的单租户 VPN RAS 网关。 还介绍了如何修改某些部署的现有基础结构。 此外，在此部署中，你将找到链接，以帮助你了解有关 VPN 连接过程、要配置的服务器、ProfileXML VPNv2 CSP 节点的详细信息，以及用于部署 Always On VPN 的其他技术。

**Always On VPN 部署方案：**

1. 仅部署 Always On VPN。
2. 使用 Azure AD 为 VPN 连接部署 Always On VPN 和条件访问。

有关所显示方案的详细信息和工作流，请参阅[部署 ALWAYS ON VPN](always-on-vpn-deploy-deployment.md)。

## <a name="what-isnt-provided-in-this-deployment"></a>此部署中不提供的功能

此部署未提供以下内容的说明：

- Active Directory 域服务（AD DS）。
- Active Directory 证书服务（AD CS）和公钥基础结构（PKI）。
- 动态主机配置协议（DHCP）。
- 网络硬件，如以太网布线、防火墙、交换机和集线器。
- 远程用户可以通过 Always On VPN 连接访问的其他网络资源，如应用程序和文件服务器。
- Internet 连接或使用 Azure AD 进行 Internet 连接的条件性访问。 有关详细信息，请参阅[Azure Active Directory 中的条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。

## <a name="next-steps"></a>后续步骤

- [了解有关 Always On VPN 特性和功能的详细信息](../../vpn-map-da.md)

- [详细了解 Always On VPN 增强](../always-on-vpn-enhancements.md)

- [了解一些高级 Always On VPN 功能](always-on-vpn-adv-options.md)

- [了解有关 Always On VPN 技术的详细信息](../always-on-vpn-technology-overview.md)

- [开始规划 Always On 的 VPN 部署](always-on-vpn-deploy-deployment.md)