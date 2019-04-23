---
title: 针对 Windows Server 和 Windows 10 的始终启用 VPN 部署
description: 可以使用此部署通过使用针对 Windows 10 客户端计算机在 Windows Server 2016 或更高版本的远程访问和 Always On VPN 配置文件部署的远程员工始终在虚拟专用网络 (VPN) 连接。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cb60bdc6d6f3ff074f04827aa95c9e8e8abf35b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859618"
---
# <a name="always-on-vpn-deployment-for-windows-server-and-windows-10"></a>适用于 Windows Server 和 Windows 10 的 always On VPN 部署

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows 10

&#171;  [**上一：** 远程访问](../../../Remote-Access.md)<br>
&#187;[**下一步：** 了解有关 Always On VPN 特性和功能](../../vpn-map-da.md)


Always On VPN 提供了单个且一致的解决方案，用于远程访问和支持已加入域的、 非域加入 （工作组） 或加入 Azure AD-设备，甚至个人拥有的设备。  使用 Always On VPN，连接类型不需要以独占方式为用户或设备，但可以是这两者的组合。 例如，您可以针对远程设备管理启用设备身份，然后启用用户身份验证连接到内部公司站点和服务。



## <a name="prerequisites"></a>先决条件

您很可能有的技术部署可用于部署 Always On VPN。 在 DC/DNS 服务器，以外 Always On VPN 部署所需的 NPS (RADIUS) 服务器、 证书颁发机构 (CA) 服务器和远程访问 (路由/VPN) 服务器。 一旦设置基础结构，必须注册客户端，然后将客户端连接到你的本地安全地通过多个网络更改。

- Active Directory 域基础结构，包括一个或多个域名系统 (DNS) 服务器。 内部和外部域名系统 (DNS) 区域是必需的这假定的内部区域是外部的区域 （例如，corp.contoso.com 和 contoso.com） 的委托的子域。
- Active Directory 基于公钥基础结构 (PKI) 和 Active Directory 证书服务 (AD CS)。
- 虚拟或物理、 现有或新，若要安装网络策略服务器 (NPS) 服务器。 如果你的网络上已有的 NPS 服务器，您可以修改现有的 NPS 服务器配置，而不是添加新的服务器。
- 为支持 IKEv2 VPN 连接和 LAN 路由的功能的一小部分的 RAS 网关 VPN 服务器的远程访问。
- 包括两个防火墙的外围网络。  请确保防火墙允许流量所需的 VPN 和 RADIUS 通信，才能正常工作。 有关详细信息，请参阅[始终在 VPN 技术概述](../always-on-vpn-technology-overview.md)。
- 物理服务器或与两个物理以太网网络适配器的 RAS 网关 VPN 服务器作为安装远程访问外围网络上的虚拟机 (VM)。 Vm 主机需要虚拟 LAN (VLAN)。 
- 管理员或同等成员资格是最低要求。
- 阅读本指南，以确保在执行部署之前准备为此部署的规划部分。
- 查看为每个所用的技术设计和部署指南。 这些指南可以帮助您确定部署方案提供的服务和组织的网络所需的配置。 有关详细信息，请参阅[始终在 VPN 技术概述](../always-on-vpn-technology-overview.md)。
- 所选部署 Always On VPN 配置，因为 CSP 不是特定于供应商的管理平台。


>[!IMPORTANT]
>对于此部署中，它不是您的基础结构服务器，例如运行 Active Directory 域服务、 Active Directory 证书服务和网络策略服务器的计算机正在运行 Windows Server 2016 的要求。 可以使用早期版本的 Windows Server、 Windows Server 2012 R2，例如，为基础结构服务器和运行远程访问服务器。
>
>请不要尝试在虚拟机上部署远程访问\(VM\)在 Microsoft Azure 中。 在 Microsoft Azure 中使用远程访问不被支持，包括远程访问 VPN 和 DirectAccess。 有关详细信息，请参阅[对 Microsoft Azure 虚拟机的 Microsoft 服务器软件支持](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。


## <a name="bkmk_about"></a>有关此部署

提供的说明将引导你完成将远程访问部署为单个租户 VPN RAS 网关点\-到\-站点 VPN 连接，使用任何下述，对于运行 Windows 的远程客户端计算机的方案10。 您还会发现修改某些现有的基础结构部署的说明。 此外在此部署中，找到链接，以帮助你详细了解 VPN 连接过程中，要配置的服务器、 ProfileXML VPNv2 CSP 节点和其他技术来部署 Always On VPN。

**Always On VPN 部署方案：**

1. 始终在 VPN 上仅部署。
2. 将 Always On VPN 部署使用的 VPN 连接使用 Azure AD 条件性访问。


有关详细信息和提供的方案的工作流，请参阅[部署 Always On VPN](always-on-vpn-deploy-deployment.md)。


## <a name="bkmk_not"></a>在此部署中不提供内容

此部署不提供的说明：

- Active Directory 域服务\(AD DS\)。
- Active Directory 证书服务\(AD CS\)和公钥基础结构\(PKI\)。
- 动态主机配置协议\(DHCP\)。 
- 网络硬件，如以太网电缆、 防火墙、 交换机和集线器。
- 其他网络资源，例如，应用程序和文件服务器，远程用户可以访问通过 Always On VPN 连接。
- Internet 连接或 Internet 连接使用 Azure AD 的条件性访问。 有关详细信息，请参阅[Azure Active Directory 中的条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。




## <a name="next-steps"></a>后续步骤

- [了解有关 Always On VPN 特性和功能的详细信息](../../vpn-map-da.md)

- [了解有关 Always On VPN 增强功能的详细信息](../always-on-vpn-enhancements.md)

- [了解一些高级的 Always On VPN 功能](always-on-vpn-adv-options.md)

- [了解有关 Always On VPN 技术的详细信息](../always-on-vpn-technology-overview.md)

- [开始规划 Always On VPN 部署](always-on-vpn-deploy-deployment.md)


---
