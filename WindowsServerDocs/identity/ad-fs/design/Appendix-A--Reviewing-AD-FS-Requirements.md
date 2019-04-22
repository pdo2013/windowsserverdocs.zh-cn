---
ms.assetid: 39ecc468-77c5-4938-827e-48ce498a25ad
title: 附录 A-查看 AD FS 要求
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d5cfb5de77843eebfc152b9c79ac55bab1fa7727
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818168"
---
# <a name="appendix-a-reviewing-ad-fs-requirements"></a>附录 A：查看 AD FS 要求

>适用于：Windows Server 2012

以便在 Active Directory 联合身份验证服务 (AD FS) 部署中的组织伙伴可以成功地协作，您必须首先确保配置企业网络基础结构以支持对帐户的 AD FS 要求，命名解析和证书。 AD FS 具有以下类型的要求：  
  
> [!TIP]  
> 可以在 Microsoft TechNet Wiki 上的 [AD FS 内容导航图](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) 页面中找到更多 AD FS 资源链接。 此页面由 AD FS 社区的成员管理，并由 AD FS 产品团队定期进行监视。  
  
## <a name="hardware-requirements"></a>硬件要求  
以下的最低和推荐硬件要求适用于联合身份验证服务器和联合服务器代理计算机。  
  
|硬件要求|最低要求|建议的要求|  
|------------------------|-----------------------|---------------------------|  
|CPU 速度|单核 1 千兆赫 (GHz)|四核 2 GHz|  
|RAM|1 GB|4 GB|  
|磁盘空间|50 MB|100 MB|  
  
## <a name="software-requirements"></a>软件要求  
AD FS 依赖于内置到 Windows Server® 2012年的操作系统的服务器功能。  
  
> [!NOTE]  
> 联合身份验证服务和联合身份验证服务代理角色服务无法共存于同一台计算机上。  
  
## <a name="certificate-requirements"></a>证书要求  
证书在联合服务器、联合服务器代理、声明感知应用程序和 Web 客户端之间起到保护通信安全的最重要作用。 对证书的要求各不相同，具体取决于你要设置的是联合服务器计算机还是联合服务器代理计算机，如本部分中所述。  
  
### <a name="federation-server-certificates"></a>联合服务器证书  
联合服务器要求下表中的证书。  
  
|证书类型|描述|在部署之前你需要知道的内容|  
|--------------------|---------------|------------------------------------------|  
|安全套接字层 (SSL) 证书|这是用于保护联合服务器和客户端之间通信安全的标准安全套接字层 (SSL) 证书。|此证书必须绑定到 Internet 信息服务 (IIS) 中的联合服务器或联合服务器代理的默认网站。  对于联合服务器代理，必须在成功运行联合服务器代理配置向导之前，在 IIS 中配置该绑定。<br /><br />**建议：** 因为此证书必须受 AD FS 的客户端信任，所以请使用由公共（第三方）证书颁发机构 (CA)（例如 VeriSign）颁发的服务器身份验证证书。 **更改暂存文件夹路径**此证书的使用者名称用于表示部署的 AD FS 的每个实例的联合身份验证服务名称。 出于此原因，你可能要考虑为任何新的 CA 颁发的证书选择最能代表你的公司或伙伴组织名称的使用者名称。|  
|服务通信证书|此证书用于保护联合服务器之间的通信安全以确保 WCF 消息的安全性。|默认情况下，将 SSL 证书用作服务通信证书。  这可以使用 AD FS 管理控制台进行更改。|  
|令牌签名证书|这是一种用于安全地对联合服务器颁发的所有令牌进行签名的标准 X509 证书。|令牌签名证书必须包含私钥，而且它应链接到联合身份验证服务中受信任的根。 默认情况下，AD FS 创建自签名证书。 但是，你以后可以使用“AD FS 管理”管理单元将其更改为 CA 颁发的证书，具体取决于你的组织的需求。|  
|令牌解密证书|这是用于解密由伙伴联合服务器加密的任何传入令牌的标准 SSL 证书。 该证书也在联合元数据中发布。|默认情况下，AD FS 创建自签名证书。 但是，你以后可以使用“AD FS 管理”管理单元将其更改为 CA 颁发的证书，具体取决于你的组织的需求。|  
  
> [!CAUTION]  
> 用于令牌签名和令牌解密的证书对联合身份验证服务的稳定性至关重要。 由于任何为此目的配置的证书的丢失或非计划删除都可能中断服务，因此应备份任何为此目的配置的证书。  
  
有关联合服务器使用的证书的详细信息，请参阅[联合服务器的证书要求](Certificate-Requirements-for-Federation-Servers.md)。  
  
### <a name="federation-server-proxy-certificates"></a>联合服务器代理证书  
联合服务器代理要求下表中的证书。  
  
|证书类型|描述|在部署之前你需要知道的内容|  
|--------------------|---------------|------------------------------------------|  
|服务器身份验证证书|这是用于保护联合服务器代理和 Internet 客户端计算机之间通信安全的标准安全套接字层 (SSL) 证书。|此证书必须绑定到 Internet 信息服务 (IIS) 中的默认网站，才能成功运行 AD FS 联合服务器代理配置向导。<br /><br />**建议：** 因为此证书必须受 AD FS 的客户端信任，所以请使用由公共（第三方）证书颁发机构 (CA)（例如 VeriSign）颁发的服务器身份验证证书。<br /><br />**更改暂存文件夹路径**此证书的使用者名称用于表示部署的 AD FS 的每个实例的联合身份验证服务名称。 出于此原因，你可能要考虑选择最能代表你的公司或伙伴组织名称的使用者名称。|  
  
有关联合服务器代理使用证书的详细信息，请参阅[联合服务器代理的证书要求](Certificate-Requirements-for-Federation-Server-Proxies.md)。  
  
## <a name="browser-requirements"></a>浏览器要求  
尽管可以采取任何当前具有 JavaScript 功能的 Web 浏览器作为 AD FS 客户端进行工作，但默认情况下提供的网页仅在 Windows 上针对 Internet Explorer 版本 7.0、8.0 和 9.0、Mozilla Firefox 3.0、以及 Safari 3.1 进行过测试。 必须启用 JavaScript，而且必须启用 Cookie，以便基于浏览器的登录和注销正常工作。  
  
Microsoft 的 AD FS 产品团队已成功测试下表中的浏览器和操作系统配置。  
  
|浏览器|Windows 7|Windows Vista|  
|-----------|-------------|-----------------|  
|Internet Explorer 7.0|X|X|  
|Internet Explorer 8.0|X|X|  
|Internet Explorer 9.0|X|未测试|  
|FireFox 3.0|X|X|  
|Safari 3.1|X|X|  
  
> [!NOTE]  
> AD FS 支持上表中显示的所有浏览器的 32 位和 64 位版本。  
  
### <a name="cookies"></a>Cookie  
AD FS 将创建必须存储在客户端计算机上的基于会话的 Cookie 和永久 Cookie，以提供登录、注销、单一登录 (SSO) 和其他功能。 因此，必须将客户端浏览器配置为接受 Cookie。 用于身份验证的 Cookie 始终是为原始服务器编写的安全超文本传输协议 (HTTPS) 会话 Cookie。 如果未将客户端浏览器配置为允许使用这些 Cookie，则 AD FS 不能正常工作。 永久 Cookie 用于保留用户选择的声明提供方。 你可以通过在 AD FS 登录页的配置文件中使用配置设置来禁用它们。  
  
出于安全原因，对 TLS/SSL 的支持是必需的。  
  
## <a name="network-requirements"></a>网络要求  
正确配置以下网络服务对于成功部署你的组织中的 AD FS 至关重要。  
  
### <a name="tcpip-network-connectivity"></a>TCP/IP 网络连接  
客户端; 之间存在 TCP/IP 网络连接必须为 AD FS 正常，域控制器;以及承载联合身份验证服务、 联合身份验证服务代理 （如果使用） 和 AD FS Web 代理的计算机。  
  
### <a name="dns"></a>DNS  
AD FS 中，Active Directory 域服务 (AD DS) 以外的操作至关重要的主网络服务是域名系统 (DNS)。 在部署 DNS 时，用户可以使用很容易记住的友好的计算机名称来连接到 IP 网络上的计算机以及其他资源。  
  
 Windows Server 2008 使用 DNS 进行名称解析，而不基于 Windows NT 4.0 网络中使用的 Windows Internet 名称服务 (WINS) NetBIOS 名称解析。 仍可能将 WINS 用于需要它的应用程序。 但是，AD DS 和 AD FS 需要 DNS 名称解析。  
  
配置 DNS 以支持 AD FS 的过程会有所不同，具体取决于是否：  
  
-   你的组织已经具有现有的 DNS 基础结构。 在大多数情况下，DNS 已配置在整个网络中，以便你企业网络中的 Web 浏览器客户端有权访问 Internet。 由于 Internet 访问权限和名称解析的 AD FS 的要求，假设此基础结构已在 AD FS 部署。  
  
-   你要将联合服务器添加到企业网络。 为了在企业网络中对用户进行身份验证，必须将企业网络林中的内部 DNS 服务器配置为返回运行联合身份验证服务的内部服务器的 CNAME。 有关详细信息，请参阅 [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md)。  
  
-   你要将联合服务器代理添加到外围网络。 当你想要对位于标识伙伴组织的企业网络中的用户帐户进行身份验证时，则必须配置公司网络林状结构中的内部 DNS 服务器为返回内部联合身份验证服务器代理的 CNAME。 有关如何配置 DNS 以容纳新增的联合服务器代理的信息，请参阅[联合服务器代理的名称解析要求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
-   你要设置测试实验室环境的 DNS。 如果你打算使用 AD FS 测试实验室环境中任何单个根 DNS 服务器为权威，则可能需要设置 DNS 转发器，以便两个或多个林之间的名称查询将进行相应地转发。 有关如何设置 AD FS 测试实验室环境的常规信息，请参阅[AD FS 分步指导和方法指南](https://go.microsoft.com/fwlink/?LinkId=180357)。  
  
## <a name="attribute-store-requirements"></a>属性存储要求  
AD FS 要求至少一个属性存储用于进行用户身份验证以及为这些用户提取安全声明。 有关 AD FS 支持的属性存储的列表，请参阅 AD FS 设计指南中的[属性存储的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)。  
  
> [!NOTE]  
> 默认情况下，AD FS 会自动创建 Active Directory 属性存储。  
  
属性存储要求取决于你的组织充当的是帐户伙伴（承载联合用户）还是资源伙伴（承载联合应用程序）。  
  
### <a name="adds"></a>AD DS  
为 AD FS 中成功运行，必须运行域控制器在帐户伙伴组织或资源伙伴组织中的 Windows Server 2003 SP1、 Windows Server 2003 R2、 Windows Server 2008 或 Windows Server 2012。  
  
在加入域的计算机上安装和配置 AD FS 时，该域的 Active Directory 用户帐户存储可作为可选属性存储。  
  
> [!IMPORTANT]  
> 由于 AD FS 要求安装 Internet 信息服务 (IIS)，我们建议你不在生产环境中以安全为目的在域控制器上安装 AD FS 软件。 但是，此配置受 Microsoft 客户服务支持部门的支持。  
  
#### <a name="schema-requirements"></a>架构要求  
AD FS 不需要架构更改或对 AD DS 功能级别修改。  
  
#### <a name="functional-level-requirements"></a>功能级别要求  
大多数 AD FS 功能不要求 AD DS 功能级别的修改就可以成功运行。 但是，如果证书明确映射到 AD DS 中的用户帐户，则 Windows Server 2008 域功能级别或更高版本是要成功运行客户端证书身份验证所必需的。  
  
#### <a name="service-account-requirements"></a>服务帐户要求  
如果要创建联合服务器场，则必须首先在联合身份验证服务可以使用的 AD DS 中创建一个专用的基于域的服务帐户。 然后配置服务器场中的每个联合服务器以使用此帐户。 有关如何执行此操作的详细信息，请参阅 AD FS 部署指南中的[手动配置联合服务器场的服务帐户](../../ad-fs/deployment/Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)。  
  
### <a name="ldap"></a>LDAP  
使用其他基于轻型目录访问协议 (LDAP) 的属性存储时，你必须连接到支持 Windows 集成身份验证的 LDAP 服务器。 必须采用如 RFC 2255 中所述的 LDAP URL 格式编写 LDAP 连接字符串。  
  
### <a name="sql-server"></a>SQL Server  
为 AD FS 中成功运行，必须运行承载结构化查询语言 (SQL) 服务器属性存储的计算机的 Microsoft SQL Server 2005 或 SQL Server 2008。 使用基于 SQL 的属性存储时，还必须配置连接字符串。  
  
### <a name="custom-attribute-stores"></a>自定义属性存储  
你可以开发自定义属性存储来启用高级方案。 内置于 AD FS 的策略语言可以引用自定义属性存储，以便可以增强以下任何方案：  
  
-   创建本地身份验证的用户的声明  
  
-   补充外部身份验证的用户的声明  
  
-   授权用户获取令牌  
  
-   授权服务，以获取用户行为的令牌  
  
使用自定义属性存储时，可能还需要配置连接字符串。 在此情况下，你可以输入你想要的并且能够连接到你的自定义属性存储的任何自定义代码。 在此情况下，连接字符串是一组解释为自定义属性存储的开发人员实现的名称/值对。  
  
有关开发和使用自定义属性存储的详细信息，请参阅[属性存储概述](https://go.microsoft.com/fwlink/?LinkId=190782)。  
  
## <a name="application-requirements"></a>应用程序要求  
联合服务器可以与联合应用程序相互通信并保护联合应用程序，如声明感知应用程序。  
  
## <a name="authentication-requirements"></a>身份验证要求  
AD FS 集成天生就与现有的 Windows 身份验证，例如，Kerberos 身份验证、 NTLM、 智能卡和 X.509 v3 客户端证书。 联合服务器使用标准 Kerberos 身份验证对域用户进行身份验证。 客户端可以通过使用基于表单的身份验证、智能卡身份验证和 Windows 集成身份验证进行身份验证，具体取决于配置身份验证的方式。  
  
AD FS 联合服务器代理角色使得使用 SSL 客户端身份验证从外部进行用户身份验证变为可能。 你还可以将联合服务器角色配置为要求 SSL 客户端身份验证，尽管通常最无缝的用户体验是通过配置 Windows 集成身份验证的帐户联合服务器来实现。 在这种情况下，AD FS 不能控制用户使用的 Windows 桌面登录的凭据。  
  
### <a name="smart-card-logon"></a>智能卡登录  
尽管 AD FS 可以强制执行的用于身份验证 （密码、 SSL 客户端身份验证或 Windows 集成身份验证） 的凭据类型，但它不会直接强制使用智能卡进行身份验证。 因此，AD FS 不提供客户端用户界面 (UI) 来获取智能卡个人标识号 (pin) 凭据。 这是因为基于 Windows 的客户端有意不提供用户凭据对联合身份验证服务器或 Web 服务器的详细信息。  
  
### <a name="smart-card-authentication"></a>智能卡身份验证  
智能卡身份验证使用 Kerberos 协议对帐户联合身份验证服务器进行身份验证。 AD FS 不能进行扩展以添加新的身份验证方法。 智能卡中的证书不需要链接到客户端计算机上的受信任的根。 使用基于智能卡证书与 AD FS 需要满足以下条件：  
  
-   智能卡的读取器和加密服务提供程序 (CSP) 必须在浏览器所在的计算机上工作。  
  
-   智能卡证书必须链接到帐户联合身份验证服务器和帐户联合服务器代理上受信任的根。  
  
-   必须通过以下方法之一将该证书映射到 AD DS 中的用户帐户：  
  
    -   证书使用者名称对应于 AD DS 中的用户帐户的 LDAP 可分辨名称。  
  
    -   证书使用者 Altname 扩展具有 AD DS 中用户帐户的用户主体名称 (UPN)。  
  
若要在某些情况下支持某些身份验证强度要求，也可以配置 AD FS 以创建指示用户通过身份验证的方式的声明。 然后，信赖方可以使用此声明做出授权决定。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
