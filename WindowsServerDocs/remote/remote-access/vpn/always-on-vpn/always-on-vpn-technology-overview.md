---
title: Always On VPN 技术概述
description: '本页 provies 提供指向详细文档链接的 Always On VPN 技术的简要概述。 '
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 31d0d5c12760fc627ce93972f4a70e85f61dd178
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404371"
---
# <a name="always-on-vpn-technology-overview"></a>Always On VPN 技术概述

>适用于：Windows Server (半年频道), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**以前**了解 Always On VPN 增强功能](always-on-vpn-enhancements.md)
- [**一个**了解 Always On VPN 的高级功能](deploy/always-on-vpn-adv-options.md)

对于此部署，你必须安装新的远程访问服务器，该服务器运行 Windows Server 2016，并修改部分现有的部署基础结构。

下图显示了部署 Always On VPN 所需的基础结构。

![Always On VPN 基础结构](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

此图中描述的连接过程包括以下步骤：

1. 使用公共 DNS 服务器，Windows 10 VPN 客户端对 VPN 网关的 IP 地址执行名称解析查询。

2. 使用 DNS 返回的 IP 地址，VPN 客户端将连接请求发送到 VPN 网关。

3. VPN 网关还配置为远程身份验证拨入用户服务（RADIUS）客户端;VPN RADIUS 客户端将连接请求发送到组织/企业 NPS 服务器进行连接请求处理。

4. NPS 服务器处理连接请求，包括执行授权和身份验证，并确定是允许还是拒绝连接请求。

5. NPS 服务器会将访问-接受或拒绝访问响应转发给 VPN 网关。

6. 根据 VPN 服务器从 NPS 服务器接收到的响应，启动或终止连接。

有关上图所示的每个基础结构组件的详细信息，请参阅以下各节。

>[!NOTE]
>如果你已在网络上部署了某些技术，则可以使用本部署指南中的说明来针对此部署目的执行其他技术配置。

## <a name="domain-name-system-dns"></a>域名系统 (DNS)

内部和外部域名系统（DNS）区域都是必需的，这假设内部区域是外部区域的委派子域（例如，corp.contoso.com 和 contoso.com）。

详细了解[域名系统（DNS）](../../../../networking/dns/dns-top.md)或[核心网络指南](../../../../networking/core-network-guide/core-network-guide.md)。

>[!NOTE]
>其他 DNS 设计（如裂脑 DNS）（在内部和外部的单独 DNS 区域中使用相同的域名）或无关的内部和外部域（例如，contoso. local 和 contoso.com）也可能。 有关部署裂脑 DNS 的详细信息，请参阅[将 Dns 策略用于裂脑 Dns 部署](../../../../networking/dns/deploy/split-brain-DNS-deployment.md)。

## <a name="firewalls"></a>防火墙

请确保防火墙允许 VPN 和 RADIUS 通信所需的流量正常运行。

有关详细信息，请参阅为[RADIUS 流量配置防火墙](../../../../networking/technologies/nps/nps-firewalls-configure.md)。

## <a name="remote-access-as-a-ras-gateway-vpn-server"></a>作为 RAS 网关 VPN 服务器的远程访问

在 Windows Server 2016 中，远程访问服务器角色旨在同时作为路由器和远程访问服务器执行，因此，它支持多种功能。 对于本部署指南，只需一小部分这些功能：支持 IKEv2 VPN 连接和 LAN 路由。

IKEv2 是 Internet 工程任务团队请求注释7296中所述的 VPN 隧道协议。 IKEv2 的主要优点是它不必完全了基础网络连接中的中断。 例如，如果连接暂时丢失或用户将客户端计算机从一个网络移到另一个网络，则在重新建立网络连接后，IKEv2 会自动恢复 VPN 连接，而无需用户干预。

通过使用 RAS 网关，你可以部署 VPN 连接，以便为最终用户提供对组织网络和资源的远程访问权限。 当远程计算机连接到 Internet 时，部署 Always On VPN 会在客户端与组织网络之间保持持续连接。 使用 RAS 网关，还可以在不同位置的两个服务器之间创建站点到站点 VPN 连接，例如在主办公室与分支机构之间建立站点到站点 VPN 连接，并使用网络地址转换（NAT），以便网络中的用户可以访问外部资源（如 Internet）。 此外，RAS 网关还支持边界网关协议（BGP），在远程办公室位置也有支持 BGP 的边缘网关时，提供动态路由服务。

你可以通过使用 Windows PowerShell 命令和远程访问 Microsoft 管理控制台（MMC）来管理远程访问服务（RAS）网关。

## <a name="network-policy-server-nps"></a>网络策略服务器 (NPS)

NPS 允许您为连接请求身份验证和授权创建和强制实施组织范围的网络访问策略。 使用 NPS 作为远程身份验证拨入用户服务（RADIUS）服务器时，可以在 NPS 中将网络访问服务器（如 VPN 服务器）配置为 RADIUS 客户端。

也可以配置有关使用 NPS 对连接请求进行授权的网络策略，并且可以配置 RADIUS 记帐，以便 NPS 将记帐信息记录到本地硬盘上或 Microsoft SQL Server 数据库中的日志文件。

有关详细信息，请参阅[网络策略服务器（NPS）](../../../../networking/technologies/nps/nps-top.md)。

## <a name="active-directory-certificate-services"></a>Active Directory 证书服务

证书颁发机构（CA）服务器是运行 Active Directory 证书服务的证书颁发机构。 VPN 配置需要基于 Active Directory 的公钥基础结构（PKI）。

组织可以通过将个人、设备或服务的标识绑定到相应的公钥，使用 AD CS 来增强安全性。 AD CS 还包括一些功能，可用于在各种可伸缩环境中管理证书注册和吊销。 有关详细信息，请参阅[Active Directory 证书服务概述](https://technet.microsoft.com/library/hh831740.aspx)和[公钥基础结构设计指南](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx)。

部署完成期间，你将在 CA 上配置以下证书模板。

- 用户身份验证证书模板

- VPN 服务器身份验证证书模板

- NPS 服务器身份验证证书模板

### <a name="certificate-templates"></a>证书模板

证书模板允许您颁发为所选任务预配置的证书，从而极大地简化了管理证书颁发机构（CA）的任务。 证书模板 MMC 管理单元允许您执行以下任务。

- 查看每个证书模板的属性。

- 复制和修改证书模板。

- 控制哪些用户和计算机可以读取模板和注册证书。

- 执行与证书模板相关的其他管理任务。

证书模板是企业证书颁发机构（CA）的有机组成部分。 它们是环境证书策略的重要元素，它是证书注册、使用和管理的一组规则和格式。

有关详细信息，请参阅[证书模板](https://technet.microsoft.com/library/cc730705.aspx)。

### <a name="digital-server-certificates"></a>数字服务器证书

本部署指南提供了有关使用 Active Directory 证书服务（AD CS）注册并自动向远程访问和 NPS 基础结构服务器注册证书的说明。 AD CS 使你可以构建公钥基础结构（PKI）并为你的组织提供公钥加密、数字证书和数字签名功能。

当你在网络中的计算机之间使用数字服务器证书进行身份验证时，证书将提供：

1. 通过加密的机密性。

2. 数字签名的完整性。

3. 通过将证书密钥与计算机网络上的计算机、用户或设备帐户相关联进行身份验证。

有关详细信息，请[参阅 AD CS 循序渐进指南：两层 PKI 层次结构](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)部署。

## <a name="active-directory-domain-services-ad-ds"></a>Active Directory 域服务 (AD DS)

AD DS 提供了一个分布式数据库，该数据库可以存储和管理有关网络资源的信息，以及启用了目录的应用程序中特定于应用程序的数据。 管理员可以使用 AD DS 将网络元素（如用户、计算机和其他设备）整理到层次内嵌结构。 内嵌层次结构包括 Active Directory 林、林中的域以及每个域中的组织单位 (OU)。 运行 AD DS 的服务器称为域控制器。

AD DS 包含受保护的可扩展身份验证协议（PEAP）用于对用户凭据进行身份验证并评估 VPN 连接请求授权的用户帐户、计算机帐户和帐户属性。 有关部署 AD DS 的信息，请参阅 Windows Server 2016 [Core 网络指南](../../../../networking/core-network-guide/Core-Network-Guide.md)。

完成此部署中的步骤后，你将在域控制器上配置以下各项。

- 在组策略中为计算机和用户启用证书自动注册

- 创建 VPN 用户组

- 创建 VPN 服务器组

- 创建 NPS 服务器组

### <a name="active-directory-users-and-computers"></a>Active Directory 用户和计算机

Active Directory 用户和计算机是包含代表物理实体（例如计算机、人员或安全组）的帐户的 AD DS 的组件。 安全组是管理员可以作为单个单元管理的用户或计算机帐户的集合。 属于特定组的用户帐户和计算机帐户称为组成员。

Active Directory 用户和计算机中的用户帐户具有 NPS 在授权过程中评估的拨号属性-除非用户帐户的 "**网络访问权限**" 属性设置为 "**通过 NPS 网络策略控制访问"** . 这是所有用户帐户的默认设置。 但在某些情况下，此设置可能会有不同的配置，阻止用户使用 VPN 进行连接。 若要防范这种可能性，可以将 NPS 服务器配置为忽略用户帐户的拨入属性。

有关详细信息，请参阅[将 NPS 配置为忽略用户帐户的拨入属性](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties)。

### <a name="group-policy-management"></a>组策略管理

组策略管理实现了基于目录的更改和配置管理用户和计算机设置，包括安全和用户信息。 使用组策略定义用户和计算机组的配置。

通过组策略，你可以指定注册表项、安全性、软件安装、脚本、文件夹重定向、远程安装服务和 Internet Explorer 维护的设置。 你创建的组策略设置包含在组策略对象（GPO）中。 通过将 GPO 与所选 Active Directory 系统容器（站点、域和 Ou）关联，你可以将 GPO 的设置应用于这些 Active Directory 容器中的用户和计算机。 若要跨企业管理组策略对象，可以使用组策略管理编辑器 Microsoft 管理控制台（MMC）。

## <a name="windows-10-vpn-clients"></a>Windows 10 VPN 客户端

除了服务器组件，请确保配置为使用 VPN 的客户端计算机正在运行 Windows 10 周年更新（版本1607）。 Windows 10 VPN 客户端必须已加入域到 Active Directory 域。


Windows 10 VPN 客户端是高度可配置的，并且提供了许多选项。 为了更好地说明此方案使用的特定功能，表1标识了此部署引用的 VPN 功能类别和特定配置。 你将使用本部署稍后部分中讨论的 VPNv2 配置服务提供程序（CSP）来配置这些功能的各个设置。 

表 1. 此部署中讨论的 VPN 功能和配置

| VPN 功能     |     部署方案配置         |
|-----------------|-----------------------------------------------|
| 连接类型 |                 本机 IKEv2                  |
|     路由     |                拆分隧道                |
| 名称解析 |  域名信息列表和 DNS 后缀  |
|   触发器    |    Always On 和受信任的网络检测    |
| 身份验证  | 具有 TPM 保护的 PEAP-GTC 用户证书 |

>[!NOTE]
>PEAP-GTC 和 TPM 分别是 "受保护的可扩展身份验证协议和传输层安全性" 和 "受信任的平台模块"。

### <a name="vpnv2-csp-nodes"></a>VPNv2 CSP 节点

在此部署中，你将使用 ProfileXML VPNv2 CSP 节点创建传递到 Windows 10 客户端计算机的 VPN 配置文件。 配置服务提供程序（Csp）是在 Windows 客户端中公开各种管理功能的接口;从概念上讲，Csp 的工作方式类似于组策略的工作方式。 每个 CSP 都包含表示单个设置的配置节点。 与组策略设置一样，你可以将 CSP 设置与注册表项、文件、权限等关联。 与使用组策略管理编辑器配置组策略对象（Gpo）的方式类似，你可以通过使用移动设备管理（MDM）解决方案（例如 Microsoft Intune）来配置 CSP 节点。 Intune 等 MDM 产品提供了一个用户友好的配置选项，可在操作系统中配置 CSP。

![将移动设备管理配置为 CSP](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

但是，不能直接通过用户界面（UI）（如 Intune 管理控制台）配置某些 CSP 节点。 在这些情况下，必须手动配置开放移动联盟统一资源标识符（OMA-URI）设置。 使用 OMA 设备管理协议（"OMA DM"）配置 OMA-URI，这是大多数新式 Apple、Android 和 Windows 设备支持的通用设备管理规范。 只要它们遵循 OMA 规范，所有 MDM 产品都应以相同的方式与这些操作系统交互。

Windows 10 提供了许多 Csp，但此部署侧重于使用 VPNv2 CSP 来配置 VPN 客户端。 VPNv2 CSP 允许通过唯一 CSP 节点配置 Windows 10 中的每个 VPN 配置文件设置。 VPNv2 CSP 中还包含一个名为*ProfileXML*的节点，它允许您在一个节点中配置所有设置，而不是单独配置。 有关 ProfileXML 的详细信息，请参阅本部署后面的 "ProfileXML 概述" 部分。 有关每个 VPNv2 CSP 节点的详细信息，请参阅[VPNV2 csp](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp)。

## <a name="next-steps"></a>后续步骤

- [了解一些高级 Always On VPN 功能](deploy/always-on-vpn-adv-options.md)

- [开始规划 Always On 的 VPN 部署](deploy/always-on-vpn-deploy-deployment.md)

## <a name="related-topics"></a>相关主题

- [适用于 Microsoft Azure 虚拟机的 Microsoft 服务器软件支持](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)：本文讨论在 Microsoft Azure 虚拟机环境（基础结构即服务）中运行 Microsoft 服务器软件的支持策略。

- [远程访问](../../Remote-Access.md)：本主题概述了 Windows Server 2016 中的远程访问服务器角色。

- [Windows 10 VPN 技术指南](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide)：本指南将指导你完成在企业 VPN 解决方案中针对 Windows 10 客户端做出的决策，以及如何配置你的部署。 本指南引用 VPNv2 配置服务提供程序（CSP），并使用 Microsoft Intune 和适用于 Windows 10 的 VPN 配置文件模板提供移动设备管理（MDM）配置说明。

- [核心网络指南](../../../../networking/core-network-guide/Core-Network-Guide.md)：本指南提供了有关如何在新林中规划和部署完全正常运行的网络和新的 Active Directory 域所需的核心组件的说明。

- [域名系统（DNS）](../../../../networking/dns/dns-top.md)：本主题提供了域名系统（DNS）的概述。 在 Windows Server 2016 中，DNS 是可以使用服务器管理器或 Windows PowerShell 命令安装的服务器角色。 如果要安装新的 Active Directory 林和域，则 DNS 会自动安装 Active Directory 作为林和域的全局目录服务器。

- [Active Directory 证书服务概述](https://technet.microsoft.com/library/hh831740.aspx)：本文档概述了 Windows Server®2012中的 Active Directory 证书服务（AD CS）。 AD CS 是允许你构建公钥基础机构 (PKI) 并为你的组织提供公钥加密、数字证书和数字签名功能的服务器角色。

- [公钥基础结构设计指南](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx)：此 wiki 提供有关设计公钥基础结构（Pki）的指导。 在配置 PKI 和证书颁发机构（CA）层次结构之前，你应该了解你的组织的安全策略和证书实行声明（CPS）。

- [AD CS 循序渐进指南：两层 PKI 层次结构](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)部署：此循序渐进指南介绍了在实验室环境中设置 Active Directory®证书服务（AD CS）的基本配置所需的步骤。 Windows Server 中的 AD CS® 2008 R2 提供可自定义的服务，用于创建和管理在采用公钥技术的软件安全系统中使用的公钥证书。

- [网络策略服务器（NPS）](../../../../networking/technologies/nps/nps-top.md)：本主题概述了 Windows Server 2016 中的网络策略服务器。 通过网络策略服务器 (NPS)，你可以针对连接请求身份验证和授权创建并实施组织级网络访问策略。
