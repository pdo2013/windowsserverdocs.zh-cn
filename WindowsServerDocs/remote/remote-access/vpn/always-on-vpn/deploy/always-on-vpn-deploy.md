---
title: 针对 Windows Server 和 Windows 10 的始终启用 VPN 部署
description: 你可以使用此部署通过在 Windows Server 2016 或更高版本的远程访问和始终启用 VPN 配置文件的 windows 10 客户端计算机部署为远程员工始终在虚拟专用网络 (VPN) 连接。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cb60bdc6d6f3ff074f04827aa95c9e8e8abf35b
ms.sourcegitcommit: c435f91ef6f3ff5ffd2661291264b939d5ce4e2a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/21/2018
ms.locfileid: "8981127"
---
# 适用于 Windows Server 和 Windows 10 的始终启用 VPN 部署

>适用于： Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**以前：** 远程访问](../../../Remote-Access.md)<br>
& #187;[**下一步：** 了解相关的始终启用 VPN 特性和功能的信息](../../vpn-map-da.md)


始终启用 VPN 提供用于远程访问和支持已加入域的单个、 一致的解决方案、 非域加入 （工作组） 或 Azure AD – 设备，甚至是个人拥有的设备。  始终启用 VPN，使用该连接类型没有专门为用户或设备，但可以是两者的组合。 例如，你可以启用远程设备管理的设备身份验证，然后启用连接到公司内部站点和服务的用户身份验证。



## 先决条件

你最有可能有技术，你可以使用部署始终启用 VPN 部署。 以外 DC/DNS 服务器，始终启用 VPN 部署所需 NPS (RADIUS) 服务器、 证书颁发机构 (CA) 服务器和远程访问 (路由/VPN) 服务器。 基础结构设置后，必须注册的客户端，然后将客户端连接到你的本地安全地通过多个网络更改。

- Active Directory 域基础结构，包括一个或多个域名系统 (DNS) 服务器。 内部和外部域名系统 (DNS) 区域是必需的这假定内部区域是外部的区域 （例如，corp.contoso.com 和 contoso.com） 委派的子域。
- Active Directory 基于公钥基础结构 (PKI) 和 Active Directory 证书服务 (AD CS)。
- 服务器、 虚拟或物理、 现有或新，若要安装网络策略服务器 (NPS)。 如果你的网络上已经 NPS 服务器，你可以修改现有 NPS 服务器配置，而不是添加新的服务器。
- 为与功能支持 IKEv2 VPN 连接和 LAN 路由一小部分 RAS 网关 VPN 服务器的远程访问。
- 包含两个防火墙的外围网络。  确保你的防火墙允许通信所需的 VPN 和 RADIUS 通信正常运行。 有关详细信息，请参阅[始终在 VPN 技术概述](../always-on-vpn-technology-overview.md)。
- 物理服务器或虚拟机 (VM) 上有两个物理以太网网络适配器为 RAS 网关 VPN 服务器安装远程访问外围网络。 Vm 主机需要虚拟局域网 (VLAN)。 
- 管理员，或等效的成员身份时所需的最低。
- 阅读本指南以确保在执行部署前将准备此部署的规划部分。
- 查看每个使用的技术的设计和部署指南。 这些指南可以帮助你确定的部署方案提供的服务和你的组织的网络所需的配置。 有关详细信息，请参阅[始终在 VPN 技术概述](../always-on-vpn-technology-overview.md)。
- 部署始终启用 VPN 配置，因为云解决方案提供商不是特定于供应商的所选的管理平台。


>[!IMPORTANT]
>对于此部署，它不是你的基础结构服务器，例如运行 Active Directory 域服务、 Active Directory 证书服务和网络策略服务器的计算机正在运行 Windows Server 2016 的一项要求。 你可以使用较早版本的 Windows Server，如 Windows Server 2012 R2，基础结构服务器和运行远程访问服务器。
>
>请不要尝试在虚拟机上部署远程访问 \(VM\) Microsoft Azure 中的。 在 Microsoft Azure 中使用远程访问不支持，包括远程访问 VPN 和 DirectAccess。 有关详细信息，请参阅[Microsoft 服务器软件支持 Microsoft Azure 虚拟机](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。


## <a name="bkmk_about"></a>有关此部署

提供的说明将指导你完成部署远程访问作为一个租户 VPN RAS 网关的点数格式 \ to\ 站点的 VPN 连接，才能使用任何下述，在运行 Windows 10 的远程客户端计算机的方案。 你还可以找到有关修改现有部署基础结构的一部分的说明。 此外整个此部署中，找到链接来帮助你了解有关 VPN 连接过程、 要配置的服务器、 ProfileXML VPNv2 CSP 节点和其他技术来部署始终启用 VPN。

**始终启用 VPN 部署方案：**

1. 部署始终启用 VPN 仅。
2. 部署使用 Azure AD 的 VPN 连接条件访问始终启用 VPN。


有关详细信息和呈现的方案的工作流，请参阅[部署始终启用 VPN](always-on-vpn-deploy-deployment.md)。


## <a name="bkmk_not"></a>此部署中不提供内容

此部署不会提供的说明：

- Active Directory 域服务 \(AD DS\)。
- Active Directory 证书服务 \(AD CS\) 和公钥基础结构 \(PKI\)。
- 动态主机配置协议 \(DHCP\)。 
- 网络硬件，例如以太网电缆、 防火墙、 开关和集线器。
- 其他网络资源，如应用程序和文件服务器，远程用户可访问通过始终启用 VPN 连接。
- Internet 连接或 Internet 连接使用 Azure AD 的条件访问。 有关详细信息，请参阅[访问 Azure Active Directory 中的条件](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。




## 后续步骤

- [了解有关始终启用 VPN 特性和功能的详细信息](../../vpn-map-da.md)

- [了解有关始终启用 VPN 增强功能](../always-on-vpn-enhancements.md)

- [了解的一些高级始终启用 VPN 功能](always-on-vpn-adv-options.md)

- [了解有关始终启用 VPN 技术的详细信息](../always-on-vpn-technology-overview.md)

- [开始菜单规划始终启用 VPN 部署](always-on-vpn-deploy-deployment.md)


---
