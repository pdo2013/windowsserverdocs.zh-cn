---
title: Always On VPN 技术概述
description: '此页提供了简要概述以及指向详细文档的 Always On VPN 技术。 '
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: fd3f7c6ca8555e270aabf04bbee6800ed284080c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821258"
---
# <a name="always-on-vpn-technology-overview"></a>Always On VPN 技术概述
>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows 10

&#171;  [**上一：** 了解有关 Always On VPN 增强功能](always-on-vpn-enhancements.md)<br>
&#187;  [**下一步：** 了解有关 Always On VPN 的高级功能](deploy/always-on-vpn-adv-options.md)

为此部署中，您必须安装新的远程访问服务器正在运行 Windows Server 2016 中，以及修改某些现有的基础结构的部署。

下图显示了 Always On VPN 部署所需的基础结构。

![Always On VPN 基础结构](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

在此图中所示的连接过程包括以下步骤：

1. Windows 10 VPN 客户端使用公用 DNS 服务器，VPN 网关的 IP 地址执行名称解析查询。

2. VPN 客户端使用 dns 返回的 IP 地址，发送到 VPN 网关的连接请求。

3. VPN 网关也被配置为远程身份验证拨入用户服务\(RADIUS\)客户端; VPN RADIUS 客户端将连接请求发送到/企业组织 NPS 服务器进行连接请求处理。

4. NPS 服务器处理连接请求，包括执行授权和身份验证，并确定是允许还是拒绝连接请求。

5. NPS 服务器将转发到 VPN 网关的访问-接受或拒绝访问的响应。

6. 启动或终止连接根据 VPN 服务器从 NPS 服务器收到的响应。

上图中所示的每个基础结构组件的详细信息，请参阅以下各节。

>[!NOTE]
>如果你已有一些在网络上部署这些技术，可用于说明本部署指南中为此部署执行的技术的其他配置。

## <a name="domain-name-system-dns"></a>域名系统 (DNS)

内部和外部域名系统 (DNS) 区域是必需的这假定的内部区域是外部的区域 （例如，corp.contoso.com 和 contoso.com） 的委托的子域。

详细了解如何[域名系统 (DNS)](../../../../networking/dns/dns-top.md)或[核心网络指南](../../../../networking/core-network-guide/core-network-guide.md)。




>[!NOTE] 
>其他 DNS 设计，例如 （使用相同的域名在内部和外部在单独的 DNS 区域中） 拆分式 DNS 或不相关的内部和外部域 （例如，contoso.local 和 contoso.com） 也是可行的。 部署拆分式 DNS 的详细信息，请参阅[拆分/式 DNS 部署的使用 DNS 策略](../../../../networking/dns/deploy/split-brain-DNS-deployment.md)。

## <a name="firewalls"></a>防火墙

请确保防火墙允许流量所需的 VPN 和 RADIUS 通信，才能正常工作。

有关详细信息，请参阅[配置对 RADIUS 流量的防火墙](../../../../networking/technologies/nps/nps-firewalls-configure.md)。

## <a name="remote-access-as-a-ras-gateway-vpn-server"></a>为 RAS 网关 VPN 服务器的远程访问

在 Windows Server 2016 中，远程访问服务器角色旨在执行以及路由器和远程访问服务器;因此，它支持广泛的功能。 有关此部署指南，要求只有一小部分的这些功能： 对 IKEv2 VPN 连接和 LAN 路由的支持。

IKEv2 VPN 隧道协议注释 7296 针对 Internet 工程任务组请求中所述。 IKEv2 的主要优点是它不必完全在基础网络连接中断。 例如，如果连接暂时中断或用户客户端计算机从一个网络移到另一个，IKEv2 时自动还原 VPN 连接重新建立网络连接 — 所有这些都无需用户干预。

通过使用 RAS 网关，你可以部署 VPN 连接，以便向最终用户提供对组织的网络和资源的远程访问。 部署 Always On VPN 保留客户端和你的组织网络之间的持续性连接远程计算机连接到 Internet。 使用 RAS 网关，你还可以创建不同的位置，在两个服务器之间的站点到站点 VPN 连接如之间主办公室和分支机构，并使用网络地址转换\(NAT\) ，以便内部用户网络可以访问外部资源，如 Internet。 此外，RAS 网关支持边界网关协议 (BGP)，提供动态路由服务时远程机构位置还具有边缘网关支持 BGP。

可以通过使用 Windows PowerShell 命令和远程访问 Microsoft 管理控制台 (MMC) 管理远程访问服务 (RAS) 网关。



## <a name="network-policy-server-nps"></a>网络策略服务器 (NPS)

NPS 允许您创建并实施组织范围的网络连接请求身份验证和授权的访问策略。 将 NPS 用作远程身份验证拨入用户服务 (RADIUS) 服务器，配置网络访问服务器，如 VPN 服务器，为 NPS 中的 RADIUS 客户端。

也可以配置有关使用 NPS 对连接请求进行授权的网络策略，并且可以配置 RADIUS 记帐，以便 NPS 将记帐信息记录到本地硬盘上或 Microsoft SQL Server 数据库中的日志文件。

有关详细信息，请参阅[网络策略服务器 (NPS)](../../../../networking/technologies/nps/nps-top.md)。


## <a name="active-directory-certificate-services"></a>Active Directory 证书服务

证书颁发机构 (CA) 服务器是运行 Active Directory 证书服务的证书颁发机构。 VPN 配置需要基于 Active Directory 的公钥基础结构 (PKI)。

组织可以使用 AD CS 来增强安全性，通过将个人、 设备或服务的标识绑定到相应的公共密钥。 AD CS 还包括可用于管理证书注册及吊销在各种可伸缩环境中的功能。 有关详细信息，请参阅[Active Directory 证书服务概述](https://technet.microsoft.com/library/hh831740.aspx)并[公共密钥基础结构设计指南](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx)。

在完成部署，过程将在 CA 上配置以下证书模板。

-   用户身份验证证书模板

-   VPN 服务器身份验证证书模板

-   NPS 服务器身份验证证书模板

### <a name="certificate-templates"></a>证书模板

证书模板可以大大简化的管理证书颁发机构 (CA) 通过允许您为所选的任务进行了预配置的颁发证书。 证书模板 MMC 管理单元中，可执行以下任务。

-   查看每个证书模板的属性。

-   复制并修改证书模板。

-   控制哪些用户和计算机可以读取模板，并注册证书。

-   执行与证书模板有关其他管理任务。

证书模板是企业证书颁发机构 (CA) 不可或缺的一部分。 它们是一个环境，这是一组规则和格式的证书注册、 使用和管理的证书策略的一个重要元素。

有关详细信息，请参阅[证书模板](https://technet.microsoft.com/library/cc730705.aspx)。

### <a name="digital-server-certificates"></a>数字的服务器证书

此部署指南提供了有关使用 Active Directory 证书服务 (AD CS) 注册和自动注册到远程访问和 NPS 基础结构服务器的证书的说明。 AD CS 允许你构建公钥基础结构 (PKI) 并为你的组织提供公钥加密、 数字证书和数字签名功能。

当网络上的计算机之间进行身份验证使用数字服务器证书时，证书将提供：

1.  通过加密确保机密性。

2.  通过数字签名的完整性。

3.  通过将证书密钥与计算机网络上的计算机、 用户或设备帐户相关联的身份验证。

有关详细信息，请参阅[AD CS 迁移分步指南：两层 PKI 层次结构部署](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)。

## <a name="active-directory-domain-services-ad-ds"></a>Active Directory 域服务 (AD DS)

AD DS 提供了一个分布式数据库，该数据库可以存储和管理有关网络资源的信息，以及启用了目录的应用程序中特定于应用程序的数据。 管理员可以使用 AD DS 将网络元素（如用户、计算机和其他设备）整理到层次内嵌结构。 内嵌层次结构包括 Active Directory 林、林中的域以及每个域中的组织单位 (OU)。 运行 AD DS 的服务器称为域控制器。

AD DS 包含用户帐户、 计算机帐户和用户凭据进行身份验证，并评估 VPN 连接请求的授权需要通过受保护的可扩展身份验证协议 (PEAP) 的帐户属性。 部署 AD DS 有关的信息，请参阅 Windows Server 2016[核心网络指南](../../../../networking/core-network-guide/Core-Network-Guide.md)。



在此部署中的步骤完成时，将域控制器上配置以下各项。

-   启用组策略中的计算机和用户的证书自动注册

-   创建 VPN 用户组

-   创建 VPN 服务器组

-   创建 NPS 服务器组

### <a name="active-directory-users-and-computers"></a>Active Directory 用户和计算机

Active Directory 用户和计算机是包含代表物理实体，如计算机、 用户或安全组的帐户的 AD ds 的组件。 安全组是用户或计算机帐户的管理员可以管理作为一个单元的集合。 属于特定组的用户和计算机帐户称为组成员。

在 Active Directory 用户和计算机的用户帐户具有 NPS 将评估在授权过程的拨入属性，除非**网络访问权限**的用户帐户的属性设置为**控件通过 NPS 网络策略访问**。 这是所有用户帐户的默认设置。 但是，在某些情况下，此设置可能具有阻止用户使用 VPN 连接的不同配置。 若要避免出现这种可能性，可以配置 NPS 服务器为忽略用户帐户拨入属性。

有关详细信息，请参阅[将 NPS 配置为忽略用户帐户拨入属性](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties)。



### <a name="group-policy-management"></a>组策略管理

组策略管理使您能够基于目录的更改和配置管理用户和计算机设置，包括安全和用户信息。 使用组策略来定义组的用户和计算机配置。

使用组策略，可以指定注册表项、 安全性、 软件安装、 脚本、 文件夹重定向、 远程安装服务和 Internet Explorer 维护的设置。 创建组策略设置包含在组策略对象 (GPO)。 通过将 GPO 与所选的 Active Directory 系统容器相关联 — 站点、 域和 Ou，您可以将 GPO 的设置应用于的用户和计算机在这些 Active Directory 容器中的。 若要管理整个企业内的组策略对象，可以使用组策略管理编辑器 Microsoft 管理控制台 (MMC)。


## <a name="windows-10-vpn-clients"></a>Windows 10 VPN 客户端

除了服务器组件，请确保配置为使用 VPN 客户端计算机正在运行 Windows 10 周年更新 （版本 1607年）。 Windows 10 VPN 客户端必须已加入域的 Active Directory 域。


Windows 10 VPN 客户端是高度可配置，并提供了许多选项。 若要更好地说明这种情况下使用的特定功能，表 1 列出了此部署所引用的 VPN 功能类别和特定配置。 通过使用更高版本在此部署中所述的 VPNv2 配置服务提供商 (CSP)，将配置的各种设置的这些功能。 

表 1. VPN 功能和配置此部署中所述

| **VPN 功能** | **部署方案配置**         |
|-----------------|-----------------------------------------------|
| 连接类型 | Native IKEv2                                  |
| 路由         | 分拆隧道                               |
| 名称解析 | 域名称信息列表和 DNS 后缀   |
| 触发      | 始终启用以及受信任网络检测       |
| 身份验证  | PEAP TLS 的 TPM 保护用户证书 |
---

>[!NOTE] 
>PEAP-TLS 和 TPM 分别为"受保护的可扩展身份验证协议使用传输层安全性"和"受信任的平台模块"。

### <a name="vpnv2-csp-nodes"></a>VPNv2 CSP 节点

在此部署中，使用 ProfileXML VPNv2 CSP 节点创建 VPN 配置文件传递到 Windows 10 客户端计算机。 配置服务提供商 (Csp) 是公开 Windows 客户端; 内的各种管理功能的接口从概念上讲，Csp 的工作原理类似于组策略的工作原理。 每个 CSP 具有表示单个设置的配置节点。 此外如组策略设置，您可以将 CSP 设置应用于注册表项、 文件、 权限和等等。 类似于如何使用组策略管理编辑器将配置组策略对象 (Gpo)，则 CSP 将节点配置通过使用 Microsoft Intune 之类的移动设备管理 (MDM) 解决方案。 Intune 等 MDM 产品提供了配置 CSP 在操作系统中的用户友好的配置选项。

![到 CSP 配置移动设备管理](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

但是，不能配置某些 CSP 节点直接通过 Intune 管理控制台中类似的用户界面 (UI)。 在这些情况下，必须配置开放移动联盟统一资源标识符 (OMA-URI) 设置手动。 使用 OMA 设备管理协议 (OMA DM)，大多数现代 Apple、 Android 和 Windows 设备支持的通用设备管理规范配置 OMA Uri。 只要它们符合的 OMA DM 规范，所有 MDM 产品应以相同的方式与这些操作系统的系统都交互。

Windows 10 提供了许多 Csp，但此部署重点介绍使用 VPNv2 CSP 来配置 VPN 客户端。 VPNv2 CSP 通过唯一的 CSP 节点，在 Windows 10 中允许的每个 VPN 配置文件设置的配置。 也包含在 VPNv2 CSP 是名为一个节点*ProfileXML*，这允许您配置的设置在一个节点中而不是单独。 有关 ProfileXML 详细信息，请参阅"ProfileXML 概述"更高版本在此部署中的部分。 有关 VPNv2 CSP 的每个节点的详细信息，请参阅[VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp)。



## <a name="next-steps"></a>后续步骤

- [了解一些高级的 Always On VPN 功能](deploy/always-on-vpn-adv-options.md)

- [开始规划 Always On VPN 部署](deploy/always-on-vpn-deploy-deployment.md)


---

## <a name="related-topics"></a>相关主题
- [对 Microsoft Azure 虚拟机的 Microsoft 服务器软件支持](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines):本文介绍在 Microsoft Azure 虚拟机环境 （基础结构--服务型） 中运行 Microsoft 服务器软件的支持策略。

- [远程访问](../../Remote-Access.md):本主题概述了 Windows Server 2016 中的远程访问服务器角色。

- [Windows 10 VPN 技术指南](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide):本指南介绍将用于在企业 VPN 解决方案中的 Windows 10 客户端以及如何配置你的部署中做出的决定。 本指南引用 VPNv2 配置服务提供商 (CSP)，并提供移动设备管理 (MDM) 适用于 Windows 10 中使用 Microsoft Intune 和 VPN 配置文件模板的配置说明。

- [核心网络指南](../../../../networking/core-network-guide/Core-Network-Guide.md):本指南将说明了如何规划和部署完全正常运行的网络和在新林中的新 Active Directory 域所需的核心组件。

- [域名系统 (DNS)](../../../../networking/dns/dns-top.md):本主题提供的域名系统 (DNS) 的概述。 Windows Server 2016 中 DNS 是可以使用服务器管理器或 Windows PowerShell 命令安装的服务器角色。 如果您正在安装新的 Active Directory 林和域，DNS 会自动安装与 Active Directory 作为林和域的全局目录服务器。 

- [Active Directory 证书服务概述](https://technet.microsoft.com/library/hh831740.aspx):本文档概述了 Windows Server® 2012年中 Active Directory 证书服务 (AD CS)。 AD CS 是允许你构建公钥基础机构 (PKI) 并为你的组织提供公钥加密、数字证书和数字签名功能的服务器角色。

- [公钥基础结构的设计指南](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx):此 wiki 提供了有关设计公共密钥基础结构 (Pki) 指南。 配置在 PKI 和证书颁发机构 (CA) 层次结构之前，应注意的组织的安全策略和证书实行声明 (CPS)。

- [AD CS 迁移分步指南：两层 PKI 层次结构部署](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx):本循序渐进指南介绍设置 Active Directory® 证书服务 (AD CS) 的基本配置在实验室环境所必需的步骤。 Windows Server® 2008 R2 中的 AD CS 提供可自定义服务用于创建和管理采用公钥技术的软件安全系统中使用的公钥证书。

- [网络策略服务器 (NPS)](../../../../networking/technologies/nps/nps-top.md):本主题概述了 Windows Server 2016 中的网络策略服务器。 通过网络策略服务器 (NPS)，你可以针对连接请求身份验证和授权创建并实施组织级网络访问策略。 

---
