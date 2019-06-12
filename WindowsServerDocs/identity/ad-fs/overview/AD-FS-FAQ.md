---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: AD FS 2016 常见问题
description: 为 AD FS 2016 的常见问题
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/17/2019
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f3f84a5c18589d38606825ee064cfb729003a05d
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719692"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>AD FS Frequently Asked Questions (FAQ)


以下文档可能会在 Active Directory 联合身份验证服务方面的常见问题主页。  文档已被拆分为基于的问题类型的组。

## <a name="deployment"></a>部署

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>如何可以升级/迁移从以前版本的 AD FS
你可以升级 AD FS 使用以下项之一：


- 到 Windows Server 2016 AD FS 的 Windows Server 2012 R2 AD FS
    - [使用 WID 数据库升级到 Windows Server 2016 中的 AD FS](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [使用 SQL 数据库升级到 Windows Server 2016 中的 AD FS](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD FS，为 Windows Server 2012 R2 AD FS
    - [将迁移到 Windows Server 2012 R2 上的 AD FS](https://technet.microsoft.com/library/dn486815.aspx)
- 到 Windows Server 2012 AD FS 的 AD FS 2.0
    - [将迁移到 Windows Server 2012 上的 AD FS](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1.x 到 AD FS 2.0
    - [从 AD FS 升级到 AD FS 2.0 的 1.x](https://technet.microsoft.com/library/ff678035.aspx)

如果需要从 AD FS 2.0 或 2.1 （Windows Server 2008 R2 或 Windows Server 2012） 升级，则必须使用的现成脚本 （位于 C:\Windows\ADFS）。

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>安装 AD FS 为什么需要重新启动服务器？

在 Windows Server 2016 中，添加了 HTTP/2 支持，但 HTTP/2 不能用于客户端证书身份验证。  因为许多 AD FS 方案使客户端证书身份验证，使用和大量的客户端支持重试请求使用 HTTP/1.1，AD FS 场配置重新配置本地服务器的 HTTP 设置，将 HTTP/1.1。  这需要重新启动服务器。  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>正在使用 Windows 2016 WAP 服务器以将 AD FS 场发布到 internet，而无需升级后端的 AD FS 场支持？
是的支持此配置，但是没有新的 AD FS 2016 功能将在此配置中受支持。  此配置旨在从 AD FS 2012 R2 ad FS 2016 在迁移阶段是暂时性的不应部署的时间很长时间。

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>是否可以为 Office 365 部署 AD FS，而不发布到 Office 365 的代理？
是的这是受支持。 但是，作为副作用

1. 你将需要手动管理更新令牌签名证书，因为 Azure AD 将无法再访问联合身份验证元数据。 有关手动更新令牌签名证书的详细信息请阅读[续订 Office 365 和 Azure Active Directory 联合身份验证证书](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)
2. 你将不能利用旧身份验证流 （例如 ExO 代理身份验证流）

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>AD FS 和 WAP 服务器的负载平衡要求是什么？

AD FS 是无状态系统。 因此，负载平衡是相当简单的登录名。 以下是有关负载平衡系统的关键建议。


- 不应使用 IP 关联配置负载均衡器。 这可能会使不必要的负载中某些 Exchange Online 的情况下的服务器的子集。
- 不，负载均衡器必须终止 HTTPS 连接，并重新启动到 ADFS 服务器的新连接。
- 负载均衡器应确保发送到 ADFS 时应作为 HTTP 数据包中的源 IP 转换连接的 IP 地址。 在的负载均衡器不能将源 IP 发送 HTTP 数据包中，负载均衡器必须添加 （或追加在现有） x 转发的标头的 IP 地址。 特定 IP 的正确处理相关功能 （禁止的 IP、 Extranet 智能锁定，...），并可能会导致降低安全性，如果配置不正确，这是必需的。
- 负载均衡器应支持 SNI。 在事件不具有，请确保 AD FS 配置为创建 HTTPS 绑定以处理非 SNI 支持客户端。
- 负载均衡器应使用 AD FS HTTP 运行状况探测终结点检测的 AD FS 或 WAP 服务器已启动并运行而如果不是确定返回 200 中排除它们。

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>多林配置 AD FS 支持？

AD FS 支持多个多林配置，依赖于基础的 AD DS 信任网络，以跨多个受信任领域对用户进行身份验证。 由于这是更简单的安装程序，以确保信任子系统正常运行而不出现问题，我们强烈建议双向林信任关系。 此外，

- 出现如外围网络林包含合作伙伴标识的单向林信任时，我们建议部署 ADFS，在 corp 林和外围网络林视为通过 LDAP 连接的另一个本地声明提供方信任。 在这种情况下 Windows 集成身份验证将不能用于外围网络林用户以及他们将需要执行密码身份验证，因为这是 ldap 唯一受支持的机制。 事件不能采用此选项，您需要在外围网络林中另一个 ADFS 的设置并将其添加为声明提供方信任中将 ADFS corp 林中。 用户将需要执行主领域发现，但 Windows 集成身份验证和密码身份验证将起作用。 请在外围网络林 ADFS 中的颁发规则中进行适当更改，如 ADFS，在 corp 林将无法再从外围网络林获取有关用户的额外用户信息。
- 虽然域级别信任受支持，并且可以工作，我们强烈建议您将转移到林级别信任模型。 此外，您需要确保的 UPN 路由的名称的 NETBIOS 名称解析需要准确地工作。



## <a name="design"></a>设计

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>适用于 AD FS 提供了哪些第三方多重身份验证提供程序？
下面是我们已经注意到的第三方提供程序的列表。  可能始终有我们不知道有关，而我们将更新列表，如我们在此了解有关可用提供程序。

- [Gemalto 身份标识和安全服务](http://www.gemalto.com/identity)
- [inWebo 企业身份验证服务](http://www.inwebo.com/)
- [Login People MFA API 连接](https://www.loginpeople.com)
- [Microsoft Active Directory 联合身份验证服务的 RSA SecurID 身份验证代理](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)
- [适用于 AD FS 的 SafeNet 身份验证服务 (SAS) 代理](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)
- [Swisscom 移动 ID 身份验证服务](http://swisscom.ch/mid)
- [Symantec 验证和 ID 保护服务 (VIP)](http://www.symantec.com/vip-authentication-service)

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>使用 AD FS 支持第三方代理？
是的第三方代理可以放前面 Web 应用程序代理，但任何第三方代理必须支持[MS ADFSPIP 协议](https://msdn.microsoft.com/library/dn392811.aspx)用于取代 Web 应用程序代理。

### <a name="what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip"></a>适用于支持 MS ADFSPIP 的 AD FS 提供了哪些第三方代理？

下面是我们已经注意到的第三方提供程序的列表。  可能始终有我们不知道有关，而我们将更新列表，如我们在此了解有关可用提供程序。

- [F5 访问策略管理器](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)

### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>其中是容量规划大小调整电子表格为 AD FS 2016？
可以下载 AD FS 2016 版本的电子表格[此处](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)。
这还可为 Windows Server 2012 R2 中的 AD FS。

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>如何确保我的 AD FS 和 WAP 服务器支持 Apple 的 ATP 要求？

Apple 已发布一的组要求调用应用程序传输安全 (ATS) 可能会影响对 AD FS 进行身份验证的 iOS 应用的调用的。  您可以确保你的 AD FS 和 WAP 服务器来确保它们支持遵循[连接使用 ATS 要求](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57)。  
具体而言，应验证你的 AD FS 和 WAP 服务器支持 TLS 1.2 和 TLS 连接协商的密码套件将支持完全向前保密。

可以启用和禁用 SSL 2.0、 3.0 和 TLS 版本 1.0、 1.1 和 1.2 使用[AD FS 中管理 SSL 协议](../operations/Manage-SSL-Protocols-in-AD-FS.md)。

若要确保你的 AD FS 和 WAP 服务器协商仅支持 ATP 的 TLS 密码套件，则可以禁用不在的所有密码套件[ATP 符合密码套件列表](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57)。  若要执行此操作，请使用[Windows TLS PowerShell cmdlet](https://technet.microsoft.com/itpro/powershell/windows/tls/index)。

## <a name="developer"></a>开发人员

### <a name="when-generating-an-idtoken-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-idtoken"></a>在生成时对 AD 进行身份验证的用户使用 ADFS id_token，如何"sub"声明生成的 id_token 中？
"Sub"声明的值为客户端 ID 的哈希 + 定位点声明值。

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>当用户登录时通过远程声明提供方信任通过 WS 次进纸 SAML P，刷新令牌/访问令牌的生存期是什么？
刷新令牌的生存期将从远程声明提供方信任的 ADFS 获取的令牌的生存期。 访问令牌的生存期将为其颁发访问令牌的信赖方的令牌生存期。

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>我需要返回也除了 OpenId 范围的配置文件和电子邮件的作用域。 可以获得使用作用域的其他信息？ 如何在 AD FS 中执行此操作？
可以使用自定义的 id_token 本身在 id_token 中添加相关信息。 有关详细信息，请参阅文章[自定义声明在 id_token 中发出](../development/Custom-Id-Tokens-in-AD-FS.md)。

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>如何颁发 JWT 令牌中的 json blob？
特殊值类型 ("<http://www.w3.org/2001/XMLSchema#json>") 和转义 character(\x22)，对于这已在 AD FS 2016 中添加。 请颁发规则以及访问令牌的最终输出的示例如下。

颁发规则示例：

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

访问令牌中颁发的声明：

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>可以像对 Azure AD 中如何执行请求的范围值的一部分传递资源值？
与 AD FS 服务器 2019年中，您现在可以传递作用域参数中嵌入的资源值。 作用域参数现在可以按以空格分隔的列表，其中每个条目是与资源/作用域的结构。 例如  
**< 创建有效的示例请求 >**

### <a name="does-ad-fs-support-pkce-extension"></a>AD FS 是否支持 PKCE 扩展？
服务器 2019年中的 AD FS 支持证明密钥，为 Code Exchange (PKCE) 用于 OAuth 授权代码授予流

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>AD FS 支持哪些允许的作用域？
- aza-如果使用[Broker 客户端的 OAuth 2.0 协议扩展](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706)和作用域参数包含作用域"aza"，如果服务器发出新的主刷新令牌，并将其设置的响应，以及设置的 refresh_token 字段中如果一个新的主刷新令牌的生存期 refresh_token_expires_in 字段会强制实施。
- openid-允许应用程序请求使用 OpenID Connect 授权协议。
- logon_cert-logon_cert 范围允许应用程序请求登录证书，可用于以交互方式登录身份验证的用户。 AD FS 服务器忽略响应中的 access_token 参数，并改为提供的 base64 编码 CMS 证书链或 CMC 完整 PKI 响应。 更多详细信息可用[此处](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e)。 
- user_impersonation-user_impersonation 作用域，才可成功请求从 AD FS 上的委托访问令牌。 有关如何使用此作用域的详细信息，请参阅[构建多层应用程序使用代理 (OBO) 与 AD FS 2016 使用 OAuth](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md)。
- vpn_cert-vpn_cert 范围允许应用程序可用于建立 VPN 连接使用 EAP-TLS 身份验证的请求 VPN 证书。 这是不再支持。
- 电子邮件-允许应用程序请求已登录用户的电子邮件声明。 这是不再支持。 
- 配置文件-允许应用程序请求配置文件相关的登录用户的声明。 这是不再支持。 


## <a name="operations"></a>操作

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>如何将 AD FS SSL 证书？
AD FS SSL 证书不在 AD FS 管理管理单元中找到的 AD FS 服务通信证书相同。  若要更改 AD FS SSL 证书，你将需要使用 PowerShell。 请按照以下文章中的指南操作：

[管理 AD FS 和 WAP 2016 中的 SSL 证书](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>如何启用或禁用 TLS/SSL 设置适用于 AD FS
若要禁用或启用 SSL 协议和密码套件，使用以下命令：

[管理 AD FS 中的 SSL 协议](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>代理 SSL 证书是否一定要与 AD FS SSL 证书相同？  
使用相对代理 SSL 证书和 AD FS SSL 证书的以下指南：


- 如果为使用的代理使用 Windows 集成身份验证，代理 SSL 证书的 AD FS 代理请求必须相同 （使用相同的密钥） 与联合身份验证服务器 SSL 证书
- 如果 AD FS 属性"ExtendedProtectionTokenCheck"是启用 （默认设置中的 AD FS），代理 SSL 证书必须相同 （使用相同的密钥） 与联合身份验证服务器 SSL 证书
- 否则为代理 SSL 证书可以具有不同的密钥从 AD FS SSL 证书，但必须满足相同[要求](../overview/AD-FS-2016-Requirements.md)

### <a name="how-can-i-configure-promptlogin-behavior-for-ad-fs"></a>如何配置提示适用于 AD FS = 登录行为？
了解如何配置提示 = 登录名，请参阅[Active Directory 联合身份验证服务提示 = 登录名参数支持](../operations/AD-FS-Prompt-Login.md)。

### <a name="how-can-i-change-the-ad-fs-service-account"></a>如何更改 AD FS 服务帐户？
若要更改 AD FS 服务帐户，请按照的说明使用 AD FS 工具箱[服务帐户 Powershell 模块](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule)。

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>如何配置与 AD FS 配合使用 Windows 集成身份验证 (WIA) 的浏览器？

有关如何配置浏览器的信息，请参阅[配置为使用 AD FS Windows 集成身份验证 (WIA) 的浏览器](../operations/Configure-AD-FS-Browser-WIA.md)。

### <a name="can-i-turn-off-browserssoenabled"></a>可以关闭 BrowserSsoEnabled？
如果你没有访问控制策略基于设备上 ADFS 或 Windows hello 企业版证书注册使用 ADFS;您可以关闭 BrowserSsoEnabled。 BrowserSsoEnabled 允许 ADFS 从客户端，其中包含设备信息收集 PRT （主刷新令牌）。 未安装该设备的 ADFS 身份验证将不适用于 Windows 10 设备。

### <a name="how-long-are-ad-fs-tokens-valid"></a>多长时间是有效的 AD FS 令牌？

此问题通常意味着多长时间执行用户单一登录 (SSO) 而无需输入新凭据，并且我作为管理员如何才能控制的？  此行为，并控制，配置设置本文所述[AD FS 单一登录设置](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings)。

下面列出的各个 cookie 和令牌的默认生存期了 （以及控制生存期的参数）：

**已注册的设备**

- PRT 和 SSO cookie:由 PSSOLifeTimeMins 控制最大值的 90 天。 （提供使用设备至少每 14 天，由 DeviceUsageWindow 控制）

- 刷新令牌： 基于上面提供一致的行为进行计算

- access_token:默认情况下，基于信赖方的 1 小时

- id_token： 与访问令牌相同

**取消注册的设备**

- SSO cookie:默认情况下，受 SSOLifetimeMins 8 小时。  启用使我的登录状态 (KMSI) 时，默认值为 24 小时，可通过 KMSILifetimeMins 进行配置。


- 刷新令牌：默认情况下的 8 小时。 24 小时内启用 KMSI

- access_token:默认情况下，基于信赖方的 1 小时

- id_token： 与访问令牌相同

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>AD FS 是否支持 HTTP 严格传输安全性 (HSTS)？  

HTTP 严格传输安全性 (HSTS) 是可帮助缓解协议降级攻击和 cookie 劫持具有 HTTP 和 HTTPS 终结点服务的 web 安全策略机制。 它允许 web 服务器来声明，web 浏览器 （或其他符合要求的用户代理） 应仅交互使用它使用 HTTPS 并永远不会通过 HTTP 协议。

以独占方式通过 HTTPS，则打开所有 AD FS 终结点的 web 身份验证流量。  因此，AD FS 有效地减轻 HTTP 严格传输安全性策略机制提供的威胁 （按照设计不存在任何降级到 HTTP 由于在 HTTP 中没有任何侦听器）。 此外，AD FS 阻止 cookie 发送到另一台服务器使用 HTTP 协议终结点的标记的安全标志的所有 cookie。

因此，在 AD FS 服务器上实现 HSTS 是不需要的因为它永远不会被降级。  出于符合性，AD FS 服务器满足这些要求，因为它们可以永远不会使用 HTTP，而所有 cookie 标记都为安全。

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-ms-转发的客户端的 ip 不包含客户端的 IP，但包含 IP 的防火墙代理的前面。 哪里可以获得正确的客户端的 IP？
建议不要执行之前 WAP SSL 终止。 如果 SSL 终止是 WAP 的前面，X-ms-转发的客户端的 ip 将包含前面 WAP 的网络设备的 IP。 以下是各种 IP 的简短说明相关的受 AD FS 的声明：
 - x-ms-客户端的 ip:网络连接到 STS 的设备的 IP。  如果是 extranet 的请求始终可以包含 WAP 的 IP。
 - x-ms-转发的客户端的 ip:多值的声明它将包含 Exchange Online 以及连接到 WAP 的设备的 IP 地址转发到 ADFS 的任何值。
 - Userip:对于 extranet 的请求此声明将包含 x 的 ms-转发的客户端的 ip 的值。  对于 intranet 的请求，此声明将包含 x ms-客户端 ip 与相同的值。

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>我正尝试获取其他声明上用户信息终结点，但其只返回主题。 如何获取其他声明？
ADFS userinfo 终结点始终返回 OpenID 标准中指定的使用者声明。 AD FS 不提供通过 UserInfo 终结点请求的其他声明。 如果需要 ID 令牌中的其他声明，请参阅[AD FS 中的自定义 ID 令牌](../development/custom-id-tokens-in-ad-fs.md)。

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>为什么我的 AD FS 服务器上看了很多 1021年错误？
AD FS 资源 00000003-0000-0000-c000-000000000000 的无效的资源访问通常记录此事件。 此错误引起的错误行为，即尝试获取访问令牌的 Azure AD Graph 服务的客户端。 由于资源不存在于 AD FS 上，这会导致 AD FS 服务器上的事件 ID 1021。 它可以安全地忽略任何警告或错误的 AD FS 上的资源 00000003-0000-0000-c000-000000000000。

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>为何会出现故障，将 AD FS 服务帐户添加到企业密钥管理组的警告？
在域中存在具有 PDC FSMO 角色的 Windows 2016 域控制器时，才创建此组。 若要解决此错误，你可以手动创建组，并执行下面提供的服务帐户添加为组的成员后所需的权限。
1.  打开“Active Directory 用户和计算机”  。
2.  **右键单击**你在导航窗格中的域名和**单击**属性。
3.  **单击**安全 （如果缺少的安全选项卡，打开高级功能从视图菜单）。
4.  **单击**高级。 **单击**添加。 **单击**选择主体。
5.  选择用户、 计算机、 服务帐户或组对话框。  在输入对象名称来选择文本框中，键入密钥管理组。  单击“确定”。
6.  在适用于列表框中，选择**下级用户对象**。
7.  使用滚动条，滚动到页面底部并**单击**全部清除。
8.  在中**属性**部分中，选择**读取 Msds-keycredentiallink**并**编写 Msds-keycredentiallink**。

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>为什么从 Android 设备的新式身份验证会失败，如果服务器不会发送与 SSL 证书链中的所有中间证书？

联合的用户可能会使用 Android ADAL 库有故障的应用的 Azure AD 身份验证。 应用程序将获得**AuthenticationException**当它尝试显示登录页。 在 chrome 中的 AD FS 登录页可能标出为不安全。

Android-跨所有版本和所有设备-不支持下载从其他证书**authorityInformationAccess**证书的字段。 这是在 Chrome 浏览器的则返回 true。 如果从 AD FS 不传递整个证书链，任何缺少中间证书的服务器身份验证证书将导致此错误。

此问题的正确解决方案是配置 AD FS 和 WAP 服务器以发送所需的中间证书和 SSL 证书。

从一台计算机上要导入到计算机的个人存储中的 AD FS 和 WAP 服务器中导出 SSL 证书，请务必导出私钥并且选择**个人信息交换-PKCS #12**。

非常重要的复选框向**尽可能包括证书路径中的所有证书**处于选中状态，以及**导出所有扩展的属性**。  

在 Windows 服务器上运行 certlm.msc 和导入 *。计算机的个人证书存储到 PFX。 这将导致服务器将整个证书链的 ADAL 库。

>[!NOTE]
> 证书存储区的网络负载均衡器也应更新，包含整个证书链，如果存在

### <a name="does-ad-fs-support-head-requests"></a>AD FS 是否支持 HEAD 请求？
AD FS 不支持 HEAD 请求。  应用程序不应使用对 AD FS 终结点的 HEAD 请求。  这可能会导致意外和/或延迟的 HTTP 错误响应。  此外，可能会看到 AD FS 事件日志中的意外的错误事件。

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>为什么无法看到刷新令牌时我正在登录远程 IdP？
IdP 颁发的令牌是否少于 1 小时的 validty 不颁发刷新令牌。 若要确保颁发刷新令牌，增加到超过 1 小时的 IdP 颁发的令牌的有效性。

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>我们是否有任何方式来更改 RP 令牌加密算法？
默认情况下，RP 令牌加密设置为 AES256 和则无法更改为任何其他值。

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>在混合模式场时收到错误尝试设置使用 Set-adfssslcertificate 的新 SSL 证书的指纹。 如何更新在混合模式下 AD FS 场的 SSL 证书？
WAP 服务器上仍可以使用组 WebApplicationProxySslCertificate。 在 ADFS 服务器，您需要使用 netsh。 请按照如下所示的步骤操作：

1. 选择维护的 ADFS 2016 服务器的子集 （例如从负载均衡器中删除）
2. #1 中所选服务器，在导入新证书通过 MMC
3. 删除现有证书

    a. netsh http delete sslcert hostnameport=fs.contoso.com:443 b。 netsh http delete sslcert hostnameport = localhost:443 c。 netsh http delete sslcert hostnameport=fs.contoso.com:49443

4.  添加新的证书

    a. netsh http 添加 sslcert hostnameport=fs.contoso.com:443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = 我 sslctlstorename = AdfsTrustedDevices

    b. netsh http add sslcert hostnameport=localhost:443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

    c. netsh http 添加 sslcert hostnameport=fs.contoso.com:49443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = 我 sslctlstorename = AdfsTrustedDevices

5. 重新启动所选服务器上的 ADFS 服务
6. 删除维护的 WAP 服务器的子集
7. 在所选的 WAP 服务器上导入新证书通过 MMC
8. 在使用 cmdlet 的 WAP 上设置新的证书

    a. Set-WebApplicationProxySslCertificate -Thumbprint " CERTTHUMBPRINT"

9. 重新启动所选的 WAP 服务器上的服务
10. 将所选的 WAP 和 AD FS 服务器放回生产环境中。

AD FS 的其余部分和类似的方式中的 WAP 服务器上执行更新。

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>当 Web 应用程序代理 (WAP) 服务器位于 Azure Web 应用程序 Firewall(WAF) 支持 ADFS？
ADFS 和 Web 应用程序服务器支持不会在终结点执行 SSL 终止的任何防火墙。 此外，ADFS/WAP 服务器具有内置的机制来防止跨站点脚本、 ADFS 代理的常见 web 攻击并满足定义的所有要求[MS ADFSPIP 协议](https://msdn.microsoft.com/library/dn392811.aspx)。

### <a name="i-am-seeing-an-event-441-a-token-with-a-bad-token-binding-key-was-found-what-should-i-do-to-resolve-this"></a>我看到"事件 441:具有错误的标记绑定密钥的令牌找到。" 应该我做什么才能解决此问题？
在 AD FS 2016 中的标记绑定会自动启用，会导致多个已知的问题与代理和联合身份验证方案的结果中此错误。 若要解决此问题，运行以下 Powershell 命令并删除令牌绑定支持。

`Set-AdfsProperties -IgnoreTokenBinding $true`
