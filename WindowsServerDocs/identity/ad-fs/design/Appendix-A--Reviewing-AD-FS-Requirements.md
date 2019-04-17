---
ms.assetid: 39ecc468-77c5-4938-827e-48ce498a25ad
title: "附录 A-检查广告 FS 要求"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d5cfb5de77843eebfc152b9c79ac55bab1fa7727
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-a-reviewing-ad-fs-requirements"></a>附录 a：检查广告 FS 要求

>适用于：Windows Server 2012

以便在你的 Active Directory 联合身份验证服务 (广告 FS) 部署组织合作伙伴可以成功协作，你必须先确保你的企业网络基础结构配置的帐户、 名称分辨率和证书支持广告 FS 要求。 广告 FS 具有以下类型的要求：  
  
> [!TIP]  
> 你可以找到其他广告 FS 资源的链接[广告 FS 内容地图](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx)Microsoft TechNet Wiki 上的页面。 本页由广告 FS 社区的成员，并由广告 FS 产品团队定期监控。  
  
## <a name="hardware-requirements"></a>硬件要求  
以下最低和推荐的硬件要求适用于在联合身份验证的服务器和联盟服务器代理计算机。  
  
|硬件要求|最低要求。|推荐的要求|  
|------------------------|-----------------------|---------------------------|  
|CPU 速度|单核 1 千兆赫 (GHz)|四核 2 g h z|  
|RAM|1 GB|4 GB|  
|磁盘空间|50 MB|100 MB|  
  
## <a name="software-requirements"></a>软件要求  
广告 FS 依赖于服务器内置于 Windows Server 2012 的® 操作系统的功能。  
  
> [!NOTE]  
> 同一台计算机的联合身份验证服务和联合身份验证服务代理角色服务不能同时存在。  
  
## <a name="certificate-requirements"></a>证书要求  
证书播放保护联合身份验证的服务器、 联合身份验证的服务器代理、 索赔支持应用程序和 Web 客户端之间的通信的最重要角色。 证书要求有所不同，具体取决于是否你设置的联合身份验证的服务器或联盟服务器代理计算机上，在此部分中所述。  
  
### <a name="federation-server-certificates"></a>联合身份验证的服务器证书  
联合身份验证的服务器需要在下表中的证书。  
  
|证书类型|描述|你需要知道部署之前|  
|--------------------|---------------|------------------------------------------|  
|安全套接字层 (SSL) 证书|这是用于保护联合身份验证的服务器和客户端之间进行通信的标准安全套接字层 (SSL) 证书。|此证书必须联合服务器或联合身份验证的服务器代理绑定到默认网站 Internet 信息服务 (IIS)。  服务器联合身份验证的代理服务器，必须之前已成功运行联合身份验证的服务器的代理配置向导中 IIS 配置绑定。<br /><br />**建议：**广告 FS 的客户必须信任该证书，因为使用的是公用 （第三方） 证书颁发机构颁发 （加拿大），例如，VeriSign 服务器身份验证证书。 **提示：**该证书的对象名称用于代表你部署的广告 FS 的每个实例联合身份验证服务名称。 出于此原因，你可能希望考虑选择任何新 CA 颁发证书上的最佳表示你的公司或组织向合作伙伴的名称的对象名称。|  
|服务通信证书|该证书允许 WCF 消息安全的保护联合身份验证的服务器之间进行通信。|默认情况下，SSL 证书用作服务通信证书。  这可以更改使用广告 FS 管理控制台。|  
|令牌签名证书|这是标准 X509 证书用于安全地登录联合身份验证的服务器问题的所有标记。|令牌签名证书必须包含专用的键，并且它应到联合身份验证服务中受信任根链接。 默认情况下，广告 FS 创建自签名的证书。 但是，你可以更改此稍后到 CA 颁发证书通过使用广告 FS 管理贴靠中，具体取决于你的组织的需求。|  
|标记解密证书|这是用于解密加密的合作伙伴联盟服务器任何传入令牌标准 SSL 证书。 它还会发布联盟元数据中。|默认情况下，广告 FS 创建自签名的证书。 但是，你可以更改此稍后到 CA 颁发证书通过使用广告 FS 管理贴靠中，具体取决于你的组织的需求。|  
  
> [!CAUTION]  
> 用于令牌签名和令牌解密证书至关重要联合身份验证服务的稳定性。 丢失或未计划的任何的删除配置实现此目的的证书可能会服务中断，因为你应备份实现此目的配置任何证书。  
  
有关使用服务器联合身份验证证书的详细信息，请参阅[服务器联合身份验证的证书要求](Certificate-Requirements-for-Federation-Servers.md)。  
  
### <a name="federation-server-proxy-certificates"></a>联合身份验证的代理服务器证书  
联合身份验证的服务器代理需要在下表中的证书。  
  
|证书类型|描述|你需要知道部署之前|  
|--------------------|---------------|------------------------------------------|  
|服务器身份验证证书|这是用于保护 Internet 客户端和服务器联合身份验证的代理之间进行通信计算机的标准安全套接字层 (SSL) 证书。|此证书必须到默认网站 Internet 信息服务 (IIS) 绑定之前，你将可以成功运行广告 FS 联合身份验证的代理配置向导。<br /><br />**建议：**广告 FS 的客户必须信任该证书，因为使用的是公用 （第三方） 证书颁发机构颁发 （加拿大），例如，VeriSign 服务器身份验证证书。<br /><br />**提示：**该证书的对象名称用于代表你部署的广告 FS 的每个实例联合身份验证服务名称。 出于此原因，你可能希望考虑选择最佳表示你的公司或组织向合作伙伴的名称的对象名称。|  
  
有关联合身份验证的服务器代理使用证书的详细信息，请参阅[证书要求联合身份验证的服务器代理](Certificate-Requirements-for-Federation-Server-Proxies.md)。  
  
## <a name="browser-requirements"></a>浏览器要求  
尽管 JavaScript 功能任何当前的 Web 浏览器可用于担任广告 FS 客户端，默认情况下提供网页经过仅针对 Internet Explorer 7.0 8.0，9.0，Mozilla Firefox 3.0，并在 Windows 上的 Safari 3.1 的版本。 JavaScript 必须为启用状态，并且 cookie 必须已启用的基于浏览器登录和注销才能正常工作。  
  
在 Microsoft 的广告 FS 产品团队成功地测试下表中的浏览器和操作系统的配置。  
  
|浏览器|Windows 7|Windows Vista|  
|-----------|-------------|-----------------|  
|Internet Explorer 7.0|X|X|  
|Internet Explorer 8.0|X|X|  
|Internet Explorer 9.0|X|没有测试|  
|FireFox 3.0|X|X|  
|Safari 3.1|X|X|  
  
> [!NOTE]  
> 广告 FS 支持 32 位和 64 位版本的上表中显示的所有浏览器。  
  
### <a name="cookies"></a>Cookie  
广告 FS 创建必须提供登录、 退出、 单一登录 (SSO)，以及其他功能的客户端计算机存储的、 基于会话和持续 cookie。 因此，客户端浏览必须配置接受 cookie。 用于身份验证的 cookie 始终都目标读者为起始服务器的安全超文本传输协议 (HTTPS) 会话 cookie。 如果没有配置客户端浏览器，允许这些 cookie，广告 FS 无法正常工作。 永久性 cookie 用于保留的索赔提供商的用户选择。 您可以禁用这些广告 FS 登录页面配置文件中使用配置设置。  
  
出于安全原因，SSL TLS 月的支持所。  
  
## <a name="network-requirements"></a>网络要求  
相应地配置以下网络服务至关重要的广告 FS 在你的组织的成功部署。  
  
### <a name="tcpip-network-connectivity"></a>TCP/IP 网络连接  
有关广告 FS，之间客户端，则必须存在了 TCP/IP 网络连接域控制器;和主机联合身份验证服务、 联合身份验证服务代理 （使用） 时和广告 FS Web 代理计算机。  
  
### <a name="dns"></a>DNS  
广告 FS、 Active Directory 域服务 (广告 DS)，以外的运行至关重要的主网络服务是域名系统 (DNS)。 DNS 部署时，用户可以使用很容易记住，若要连接到计算机和其他资源 IP 网络上的友好的计算机名称。  
  
 Windows Server 2008 名称而不是基于 Windows NT 4.0，网络中使用的 Windows Internet 名称服务 (WINS) NetBIOS 名称分辨率的分辨率使用 DNS。 它是仍有可能使用 WINS 为需要该应用程序。 但是，广告 DS 和广告 FS 需要 DNS 名称分辨率。  
  
配置 DNS 支持广告 FS 的过程会有所不同，具体取决于：  
  
-   你的组织已经有现有 DNS 基础结构。 在大多数情况下，DNS 已经配置整个你的网络，以便在你的企业网络 Web 浏览器的客户端将有权访问 Internet。 由于 Internet 访问和名称分辨率广告 FS 的要求，该基础架构假定会在广告 FS 部署的位置。  
  
-   想要添加到你的企业网络联盟的服务器。 对于企业网络中的用户身份验证，必须配置内部企业网络森林中的 DNS 服务器返回 CNAME 内部联合身份验证服务运行的服务器。 有关详细信息，请参阅[联合身份验证的服务器的名称分辨率要求](Name-Resolution-Requirements-for-Federation-Servers.md)。  
  
-   想要添加到外围网络联盟的服务器代理。 当你想要验证位于你的身份的合作伙伴公司的公司的网络的用户帐户时，必须配置内部企业网络森林中的 DNS 服务器返回内部联合身份验证的服务器代理服务器 CNAME。 有关如何配置 DNS 容纳联合身份验证的服务器代理添加的信息，请参阅[联合身份验证的服务器代理名称分辨率要求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
-   你设置 DNS 的测试实验环境。 如果你打算所在权威没有单根 DNS 服务器的测试实验环境中使用广告 FS，则可能需要 DNS 转发器设置，以便将相应地转发查询到两个或多个林之间的名称。 有关如何进行设置的广告 FS 的测试实验环境的一般信息，请参阅[广告分步和对如何到指南](https://go.microsoft.com/fwlink/?LinkId=180357)。  
  
## <a name="attribute-store-requirements"></a>特性官方商城要求  
广告 FS 需要至少一个特性官方商城的用户身份验证，并提取安全提起的索赔这些用户使用。 有关的特性列表存储广告 FS 支持，请参阅[角色的特性存储](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)广告 FS 设计指南中。  
  
> [!NOTE]  
> 默认情况下，广告 FS 会自动创建 Active Directory 特性应用商店。  
  
特性官方商城要求取决于你的组织是否充当帐户合作伙伴 （举办联盟的用户） 或 （举办联合应用程序） 的资源合作伙伴。  
  
### <a name="ad-ds"></a>广告 DS  
对于广告 FS 成功运营，必须运行帐户合作伙伴公司或资源合作伙伴公司中的域控制器 Windows Server 2003 SP1、 Windows Server 2003 R2、 Windows Server 2008 或 Windows Server 2012。  
  
当广告 FS 已安装并配置已加入域的电脑上时，该域的 Active Directory 用户帐户应用商店是可作为可选特性应用商店。  
  
> [!IMPORTANT]  
> 由于广告 FS 需要安装 Internet 信息服务 (IIS)，我们建议你无法上为安全起见生产环境中的域控制器安装广告 FS 软件。 但是，此配置受 Microsoft 客户服务支持。  
  
#### <a name="schema-requirements"></a>方案要求  
广告 FS 不需要方案更改或广告 DS 功能级别改动。  
  
#### <a name="functional-level-requirements"></a>功能级别要求  
大多数广告 FS 功能确实需要广告 DS 功能级别修改成功运营。 但是，Windows Server 2008 域功能层，或更高版本，才能进行客户端证书身份验证的证书显式映射到广告 DS 中的用户帐户如果成功运营。  
  
#### <a name="service-account-requirements"></a>服务帐户要求  
如果你要创建联盟服务器电场的日落，必须首先联合身份验证服务可以使用的广告 DS 中创建专用的域基于服务帐户。 更高版本，在使用此帐户场配置每个联合身份验证的服务器。 有关如何执行此操作的详细信息，请参阅[手动联合身份验证的服务器场配置服务帐户](../../ad-fs/deployment/Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)广告 FS 部署指南中。  
  
### <a name="ldap"></a>LDAP  
当你使用的其他便捷目录访问协议 LDAP 基于属性官方商城时，则你必须连接到 LDAP 服务器支持 Windows 集成的身份验证。 RFC 2255 中所述，还必须 LDAP URL，格式编写 LDAP 连接字符串。  
  
### <a name="sql-server"></a>SQL Server  
对于广告 FS 成功运营，Microsoft SQL Server 2005 或 SQL Server 2008 必须运行宿主结构化查询语言 (SQL) 服务器特性官方商城的计算机。 当你使用的 SQL 基于属性官方商城时，您还必须配置连接字符串。  
  
### <a name="custom-attribute-stores"></a>自定义特性官方商城  
你可以开发自定义特性官方商城启用高级的方案。 内置广告 FS 策略语言可以参考自定义特性官方商城，以便可以增强任何以下情形：  
  
-   创建本地经过身份验证的用户的声明  
  
-   补充进行外部身份验证的用户的声明  
  
-   在授权的用户，若要获取标记  
  
-   在授权服务获取的用户的行为标记  
  
当你使用的自定义特性应用商店中时，你可能需要配置连接字符串。 在此情况下，你可以输入你喜欢任何自定义代码，这允许你自定义特性官方商城的连接。 在此情况下连接字符串是一组解释实现的自定义特性官方商城的开发人员的姓名中的值对。  
  
有关开发和使用自定义特性官方商城的详细信息，请参阅[属性官方商城概述](https://go.microsoft.com/fwlink/?LinkId=190782)。  
  
## <a name="application-requirements"></a>应用程序要求  
联合身份验证的服务器可以与通信，并保护联合身份验证应用程序，如索赔识别的应用程序。  
  
## <a name="authentication-requirements"></a>身份验证要求  
广告 FS 自然地与集成现有 Windows 身份验证，例如，Kerberos 身份验证、 NTLM、 智能卡和 X.509 v3 客户端的证书。 联合身份验证的服务器使用标准 Kerberos 身份验证来验证针对域的用户。 客户可以使用基于形式的身份验证、 智能卡身份验证和 Windows 集成的身份验证，具体取决于你如何配置身份验证身份验证。  
  
广告 FS 联盟服务器代理角色使该用户验证外部使用 SSL 客户身份验证的方案。 你还可以配置联盟服务器角色，可以要求 SSL 客户身份验证、 通过尽管通常最无缝的用户体验配置集成 Windows 身份验证帐户联盟服务器。 在此情况下，广告 FS 具有的 Windows 桌面的登录的用户采用了哪些凭据无法进行控制。  
  
### <a name="smart-card-logon"></a>智能卡登录  
广告 FS 可以实施的它所使用的身份验证 （密码、 SSL 客户身份验证、 或 Windows 的集成身份验证） 的凭据类型，虽然它不直接强制使用智能卡身份验证。 因此，广告 FS 不提供的客户端的用户界面 (UI) 以获取号码 (PIN) 的凭据智能卡个人身份。 这是因为基于 Windows 的客户端有意不会提供给联合身份验证的服务器或 Web 服务器用户凭据详细信息。  
  
### <a name="smart-card-authentication"></a>智能卡身份验证  
智能卡身份验证使用 Kerberos 协议验证帐户联盟服务器的身份。 不能广告 FS 扩展添加新的验证方法。 智能卡中的证书不需要最多受信任的根客户端计算机上的链接。 使用智能卡基于与广告 FS 证书要求满足以下条件：  
  
-   阅读器和智能卡加密服务提供商 (CSP) 在浏览器的计算机必须起作用。  
  
-   智能卡证书必须链接帐户联合身份验证的服务器和帐户联合身份验证的服务器代理受信任的根到。  
  
-   证书必须映射到广告 DS 中的用户帐户，通过以下方法之一：  
  
    -   证书主题名称与广告 DS 中的用户帐户 LDAP 辨别名称相对应。  
  
    -   证书主题 altname 扩展具有广告 DS 中的用户帐户的主要用户名 (UPN)。  
  
若要在某些方案中支持某些身份验证强度要求，它也可能是配置广告 FS 创建指示用户已进行身份验证的索赔。 依赖方可以使用此声明权决定授权。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
