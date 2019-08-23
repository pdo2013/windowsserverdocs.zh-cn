---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: AD FS 常见问题
description: AD FS 常见问题
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/17/2019
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7bc98a8c9a57b2b7f63523f0411d648ca82137aa
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2019
ms.locfileid: "69980334"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>AD FS 常见问题 (FAQ)


以下文档是与 Active Directory 联合身份验证服务有关的常见问题的主页。  该文档已根据问题类型拆分为组。

## <a name="deployment"></a>部署

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>如何从以前版本的 AD FS 升级/迁移
您可以使用以下操作之一升级 AD FS:


- Windows Server 2012 R2 AD FS 到 Windows Server 2016 AD FS 或更高版本。 请注意, 如果从 Windows Server 2016 AD FS 升级到 Windows Server 2019 AD FS, 则该方法相同。 
    - [使用 WID 数据库升级到 Windows Server 2016 中的 AD FS](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [使用 SQL 数据库升级到 Windows Server 2016 中的 AD FS](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD FS Windows Server 2012 R2 AD FS
    - [迁移到 Windows Server 2012 R2 上的 AD FS](https://technet.microsoft.com/library/dn486815.aspx)
- AD FS 2.0 到 Windows Server 2012 AD FS
    - [迁移到 Windows Server 2012 上的 AD FS](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1.x 到 AD FS 2。0
    - [从 AD FS 1.x 升级到 AD FS 2。0](https://technet.microsoft.com/library/ff678035.aspx)

如果需要从 AD FS 2.0 或 2.1 (Windows Server 2008 R2 或 Windows Server 2012) 升级, 则必须使用内置脚本 (位于 C:\Windows\ADFS 中)。

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>为什么 AD FS 安装需要重新启动服务器？

Windows Server 2016 中添加了 HTTP/2 支持, 但 HTTP/2 不能用于客户端证书身份验证。  由于许多 AD FS 方案都使用客户端证书身份验证, 因此, 大量客户端不支持使用 HTTP/1.1 重试请求, AD FS 场配置将本地服务器的 HTTP 设置重新配置为 HTTP/1.1。  这需要重新启动服务器。  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>使用 Windows 2016 WAP 服务器将 AD FS 场发布到 internet, 而不升级受支持的后端 AD FS 场？
是的, 此配置受支持, 但此配置不支持新的 AD FS 2016 功能。  此配置在从 AD FS 2012 R2 到 AD FS 2016 的迁移阶段中是临时的, 不应长时间部署。

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>是否可以在不将代理发布到 Office 365 的情况下部署 Office 365 AD FS？
是的, 支持这种情况。 然而, 作为副作用

1. 你将需要手动管理更新令牌签名证书, 因为 Azure AD 将无法访问联合元数据。 有关手动更新令牌签名证书的详细信息, 请参阅[续订 Office 365 和 Azure Active Directory 的联合身份验证证书](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)
2. 你将无法利用旧的身份验证流 (例如 Exo 除外代理身份验证流)

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>AD FS 和 WAP 服务器的负载平衡要求是什么？

AD FS 是无状态系统。 因此, 对于登录名, 负载均衡非常简单。 下面是负载平衡系统的关键建议。


- 不应为负载均衡器配置 IP 关联。 在某些 Exchange Online 方案中, 这可能会使服务器子集上的负载更不适当。
- 负载均衡器不得终止 HTTPS 连接, 并重新启动与 ADFS 服务器建立新的连接。
- 负载均衡器应确保在将连接 IP 地址发送到 ADFS 时, 应将连接 IP 地址转换为 HTTP 数据包中的源 IP。 如果负载平衡器无法在 HTTP 数据包中发送源 IP, 则负载均衡器必须向 x 转发的标头添加 (或在现有情况下附加) IP 地址。 如果正确配置了某些 IP 相关功能 (禁止的 IP、Extranet 智能锁定,...), 可能会导致安全性降低, 这是必需的。
- 负载均衡器应支持 SNI。 如果不是这样, 请确保将 AD FS 配置为创建 HTTPS 绑定以处理不支持 SNI 的客户端。
- 负载均衡器应使用 AD FS HTTP 运行状况探测终结点来检测 AD FS 或 WAP 服务器是否已启动并正在运行, 如果未返回 200 OK, 则排除这些服务器。

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>AD FS 支持哪些多林配置？

AD FS 支持多林配置, 并依赖底层 AD DS 信任网络对多个受信任领域中的用户进行身份验证。 我们强烈建议使用双向林信任, 因为这是一个更简单的设置, 可确保信任子系统正确工作且不会出现问题。 附加

- 如果林信任这样的一种方式 (例如, 包含合作伙伴标识的 DMZ 林), 我们建议在 corp 林中部署 ADFS, 并将外围网络林视为通过 LDAP 连接的另一个本地声明提供程序信任。 在这种情况下, Windows 集成身份验证不适用于 DMZ 林用户, 并且需要执行密码身份验证, 因为这是唯一受支持的 LDAP 机制。 如果无法采用此选项, 则需要在 DMZ 林中设置另一个 ADFS, 并将其添加为 corp 林中 ADFS 中的声明提供方信任。 用户需要进行主领域发现, 但 Windows 集成身份验证和密码身份验证都将起作用。 请对 DMZ 林中的 ADFS 中的颁发规则进行适当的更改, corp 林中的 ADFS 会无法从 DMZ 林获取有关用户的额外用户信息。
- 当域级别信任受支持并且可以工作时, 我们强烈建议你迁移到林级别信任模型。 此外, 还需要确保名称的 UPN 路由和 NETBIOS 名称解析需要正确地工作。



## <a name="design"></a>设计

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>哪些第三方多因素身份验证提供程序可用于 AD FS？
AD FS 为第三方 MFA 提供商提供集成的扩展性机制。 没有适用于此的任何设置认证计划。 假定供应商已在发布前执行了必需的验证。 

已收到 Microsoft 通知的供应商列表在[AD FS 的 MFA 提供程序](..\operations\Configure-Additional-Authentication-Methods-for-AD-FS.md)中发布。  可能始终存在我们不了解的提供程序, 我们将在了解这些提供程序时更新列表。

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>AD FS 是否支持第三方代理？
是的, 第三方代理可以放置在 Web 应用程序代理的前面, 但任何第三方代理都必须支持用于替代 Web 应用程序代理的[ADFSPIP 协议](https://msdn.microsoft.com/library/dn392811.aspx)。

下面是我们注意到的第三方提供商的列表。  可能始终存在我们不了解的提供程序, 我们将在了解这些提供程序时更新列表。

- [F5 访问策略管理器](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)


### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>AD FS 2016 的容量规划大小调整电子表格位于何处？
可在[此处](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)下载 AD FS 2016 版电子表格。
这也可用于 Windows Server 2012 R2 中的 AD FS。

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>如何确保 AD FS 和 WAP 服务器支持 Apple 的 ATP 要求？

Apple 已发布一组称为 "应用传输安全 (ATS)" 的要求, 这些要求可能会影响对 AD FS 进行身份验证的 iOS 应用的调用。  可以确保 AD FS 和 WAP 服务器符合, 方法是确保它们支持[使用 ATS 进行连接的要求](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57)。  
具体而言, 应验证 AD FS 和 WAP 服务器是否支持 TLS 1.2, 以及 TLS 连接的协商密码套件是否支持完全向前保密。

可以[在 AD FS 中使用 "管理 Ssl 协议](../operations/Manage-SSL-Protocols-in-AD-FS.md)" 来启用和禁用 ssl 2.0 和3.0 以及 TLS 版本1.0、1.1 和1.2。

若要确保 AD FS 和 WAP 服务器仅协商支持 ATP 的 TLS 密码套件, 可以禁用不在[ATP 兼容密码套件列表](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57)中的所有密码套件。  为此, 请使用[WINDOWS TLS PowerShell cmdlet](https://technet.microsoft.com/itpro/powershell/windows/tls/index)。

## <a name="developer"></a>开发人员

### <a name="when-generating-an-id_token-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-id_token"></a>为用户根据 AD 进行身份验证时, 为用户生成 id_token 时, 如何在 id_token 中生成 "sub" 声明？
"Sub" 声明的值是客户端 ID 和定位声明值的哈希。

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>用户通过 WS 馈送/SAML-P 上的远程声明提供方信任登录时, 刷新令牌/访问令牌的生存期是什么？
刷新令牌的生存期将是 ADFS 从远程声明提供方信任获取的令牌的生存期。 访问令牌的生存期将是为其颁发访问令牌的信赖方的令牌生存期。

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>除了 OpenId 范围外, 我还需要返回配置文件和电子邮件作用域。 是否可以使用作用域获取其他信息？ 如何在 AD FS 中执行此操作？
你可以使用自定义的 id_token 在 id_token 中添加相关信息。 有关详细信息, 请参阅文章[自定义要在 id_token 中发出的声明](../development/Custom-Id-Tokens-in-AD-FS.md)。

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>如何在 JWT 令牌中颁发 json blob？
AD FS 2016 中添加了<http://www.w3.org/2001/XMLSchema#json>此的特殊 ValueType ("") 和转义符 (\x22)。 请参阅下面的示例, 了解颁发规则以及访问令牌的最终输出。

示例颁发规则:

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

在访问令牌中颁发的声明:

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>能否传递资源值作为范围值的一部分, 如如何对 Azure AD 进行请求呢？
通过服务器2019上的 AD FS, 现在可以传递嵌入在 scope 参数中的资源值。 作用域参数现在可以组织为以空格分隔的列表, 其中每个条目都作为资源/作用域的结构。 例如  
**< 创建一个有效的示例请求 >**

### <a name="does-ad-fs-support-pkce-extension"></a>AD FS 是否支持 PKCE 扩展？
服务器2019中的 AD FS 支持 OAuth 授权代码授予流的代码交换 (PKCE) 的证明密钥

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>AD FS 支持哪些允许的范围？
- aza-如果对[代理客户端使用 OAuth 2.0 协议扩展](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706), 并且 scope 参数包含作用域 "aza", 则服务器将发出新的主刷新令牌并在响应的 refresh_token 字段中设置该令牌, 并将 refresh_token_ 设置为如果强制执行, 则为新的主刷新令牌的生存期的 expires_in 字段。
- openid-允许应用程序请求使用 OpenID Connect 授权协议。
- logon_cert-logon_cert 范围允许应用程序请求登录证书, 这些证书可用于以交互方式登录经过身份验证的用户。 AD FS 服务器忽略响应中的 access_token 参数, 而是提供 base64 编码的 CMS 证书链或 CMC 完整 PKI 响应。 [此处](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e)提供了更多详细信息。 
- user_impersonation-必须使用 user_impersonation 范围, 才能成功地从 AD FS 请求代表访问令牌。 有关如何使用此作用域的详细信息, 请参阅使用[OAuth 作为 AD FS 2016 的代表构建多层应用程序 (OBO)](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md)。
- vpn_cert-vpn_cert 范围允许应用程序请求 VPN 证书, 这些证书可用于通过 EAP-TLS 身份验证建立 VPN 连接。 这不再受支持。
- 电子邮件-允许应用程序请求已登录用户的电子邮件声明。 这不再受支持。 
- 配置文件-允许应用程序请求登录用户的配置文件相关声明。 这不再受支持。 


## <a name="operations"></a>操作

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>如何实现替换 AD FS 的 SSL 证书？
AD FS SSL 证书与 AD FS 管理 "管理单元中找到的 AD FS 服务通信证书不同。  若要更改 AD FS SSL 证书, 需要使用 PowerShell。 按照以下文章中的指导进行操作:

[管理 AD FS 和 WAP 2016 中的 SSL 证书](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>如何启用或禁用 AD FS 的 TLS/SSL 设置
若要禁用或启用 SSL 协议和密码套件, 请使用以下内容:

[在 AD FS 中管理 SSL 协议](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>代理 SSL 证书是否必须与 AD FS SSL 证书相同？  
对于代理 SSL 证书和 AD FS SSL 证书, 请使用以下指南:


- 如果代理用于代理使用 Windows 集成身份验证的 AD FS 请求, 则代理 SSL 证书必须与联合服务器 SSL 证书相同 (使用相同密钥)
- 如果启用了 AD FS 属性 "ExtendedProtectionTokenCheck" (AD FS 中的默认设置), 则代理 SSL 证书必须与联合服务器 SSL 证书相同 (使用相同的密钥)
- 否则, 代理 SSL 证书可以与 AD FS SSL 证书具有不同的密钥, 但必须满足相同的[要求](../overview/AD-FS-2016-Requirements.md)

### <a name="why-do-i-only-see-a-password-login-on-ad-fs-and-not-my-other-authentication-methods-that-i-have-configured"></a>为什么只能在 AD FS 而不是我配置的其他身份验证方法上看到密码登录？ 
当应用程序明确需要映射到已配置和已启用的身份验证方法的特定身份验证 URI 时, AD FS 仅在登录屏幕中显示单个身份验证方法。 这会在 WS 联合身份验证请求的 "wauth" 参数和 SAML 协议请求中的 "RequestedAuthnCtxRef" 参数进行传达。 因此, 只显示请求的身份验证方法 (例如密码登录)。

将 AD FS 用于 Azure AD 时, 应用程序通常会将 prompt = login 参数发送到 Azure AD。 默认情况下, Azure AD 会将其转换为请求基于密码的全新登录名以 AD FS。 这是在网络中的 AD FS 上查看密码登录的最常见原因, 或者不显示使用证书登录的选项。 通过在 Azure AD 中更改联合域设置, 可以轻松地纠正这种情况。 

有关如何配置此操作的信息, 请参阅[Active Directory 联合身份验证服务 prompt = login 参数支持](../operations/AD-FS-Prompt-Login.md)。

### <a name="how-can-i-change-the-ad-fs-service-account"></a>如何更改 AD FS 服务帐户？
若要更改 AD FS 服务帐户, 请按照使用 AD FS 工具箱[服务帐户 Powershell 模块](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule)中的说明进行操作。

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>如何将浏览器配置为使用 Windows 集成身份验证 (WIA) 与 AD FS？

有关如何配置浏览器的信息, 请参阅[将浏览器配置为使用 Windows 集成身份验证 (WIA) 与 AD FS](../operations/Configure-AD-FS-Browser-WIA.md)。

### <a name="can-i-turn-off-browserssoenabled"></a>能否关闭 BrowserSsoEnabled？
如果你没有基于 ADFS 上的设备或使用 ADFS 的 Windows Hello 企业版证书注册的访问控制策略, 则为;可以关闭 BrowserSsoEnabled。 BrowserSsoEnabled 允许 ADFS 从包含设备信息的客户端收集 PRT (主要刷新令牌)。 如果没有 ADFS 的设备身份验证, 将无法在 Windows 10 设备上运行。

### <a name="how-long-are-ad-fs-tokens-valid"></a>AD FS 令牌的有效期有多长？

通常, 此问题意味着, 用户无需输入新凭据即可获得单一登录 (SSO) 的时间, 以及如何将我作为管理控制？  [AD FS 单一登录设置一](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings)文中介绍了此行为以及控制该行为的配置设置。

下面列出了各种 cookie 和令牌的默认生存期 (以及控制生存期的参数):

**已注册设备**

- PRT 和 SSO cookie:最多90天, 由 PSSOLifeTimeMins 管辖。 (提供的设备至少每14天使用一次, 由 DeviceUsageWindow 控制)

- 刷新令牌: 根据上述内容计算, 提供一致的行为

- access_token:默认情况下为1小时, 基于信赖方

- id_token: 与访问令牌相同

**未注册的设备**

- SSO cookie:默认情况下为8小时, 受 SSOLifetimeMins 管辖。  启用 "使我保持登录状态 (KMSI)" 时, 默认值为24小时, 可通过 KMSILifetimeMins 进行配置。


- 刷新令牌:默认为8小时。 启用 KMSI 24 小时

- access_token:默认情况下为1小时, 基于信赖方

- id_token: 与访问令牌相同

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>AD FS 是否支持 HTTP 严格传输安全性 (HSTS)？  

HTTP 严格传输安全性 (HSTS) 是一种 web 安全策略机制, 可帮助减少具有 HTTP 和 HTTPS 终结点的服务的协议降级攻击和 cookie 劫持。 它允许 web 服务器声明 web 浏览器 (或其他符合用户代理) 只应使用 HTTPS 与其交互, 从不通过 HTTP 协议进行交互。

Web 身份验证流量的所有 AD FS 终结点都是通过 HTTPS 以独占方式打开的。  因此, AD FS 会有效地缓解 HTTP 严格传输安全策略机制提供的威胁 (按照设计, 由于 HTTP 中没有侦听器, 因此不会降级到 HTTP)。 此外, AD FS 通过使用安全标志标记所有 cookie 来阻止将 cookie 发送到具有 HTTP 协议终结点的另一台服务器。

因此, 不需要在 AD FS 服务器上实现 HSTS, 因为它永远不会降级。  出于符合性目的, AD FS 服务器会满足这些要求, 因为它们永远不会使用 HTTP, 并且所有 cookie 都标记为安全。

此外, AD FS 2016 (具有最新修补程序) 和 AD FS 2019 支持发出 HSTS 标头。 若要配置此设置, 请参阅[自定义 HTTP 安全响应标头 AD FS](../operations/customize-http-security-headers-ad-fs.md)

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X 毫秒-转发的客户端-ip 不包含客户端的 IP, 而是在代理前面包含防火墙的 IP。 在哪里可以获得正确的客户端 IP？
不建议在 WAP 之前进行 SSL 终止。 如果 SSL 终止在 WAP 前面完成, 则将在 WAP 之前包含网络设备的 IP, 即, X 毫秒转发的客户端 ip 将包含该网络设备的 IP。 下面是 AD FS 支持的各种 IP 相关声明的简要说明:
 - x-ms-客户端-ip:连接到 STS 的设备的网络 IP。  对于 extranet 请求, 此项始终包含 WAP 的 IP。
 - x 毫秒-转发的客户端-ip:多值声明, 其中包含 Exchange Online 转发给 ADFS 的任何值以及连接到 WAP 的设备的 IP 地址。
 - Userip:对于 extranet 请求, 此声明将包含 x 毫秒转发的客户端 ip 的值。  对于 intranet 请求, 此声明将包含与 x-ms-客户端 ip 相同的值。

 此外, 在 AD FS 2016 (具有最新修补程序) 和更高版本中, 还支持捕获 x 转发的标头。 任何未在第3层转发的负载均衡器或网络设备 (保留 IP) 应将传入客户端 IP 添加到行业标准的 x 转发的标头。 

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>我尝试获取用户信息终结点上的其他声明, 但其唯一返回的是主题。 如何获取其他声明？
AD FS 的用户用户终结点始终按 OpenID 标准中指定的方式返回主语声明。 AD FS 不提供通过用户信息终结点请求的其他声明。 如果在 ID 令牌中需要其他声明, 请参阅[AD FS 中的自定义 ID 令牌](../development/custom-id-tokens-in-ad-fs.md)。

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>为什么我在 AD FS 服务器上看到了许多1021错误？
对于资源 00000003-0000-0000-c000-000000000000 的 AD FS, 通常会记录此事件的资源访问权限无效。 此错误是由于客户端尝试获取 Azure AD Graph 服务的访问令牌而导致的错误行为造成的。 由于 AD FS 上没有资源, 这会导致 AD FS 服务器上出现事件 ID 1021。 可以安全地忽略 AD FS 上的资源 00000003-0000-0000-000000000000 的任何警告或错误。

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>为什么向企业密钥管理员组添加 AD FS 服务帐户失败时, 会出现警告？
仅当域中存在具有 FSMO PDC 角色的 Windows 2016 域控制器时, 才创建此组。 若要解决此错误, 您可以手动创建组, 并按照以下步骤在将服务帐户添加为组的成员后提供所需的权限。
1.  打开“Active Directory 用户和计算机”。
2.  在导航窗格中**右键单击**你的域名, 然后**单击**"属性"。
3.  **单击**安全性 (如果缺少 "安全" 选项卡, 请从 "视图" 菜单中打开 "高级功能")。
4.  **单击**高. **单击**把. **单击**选择主体。
5.  此时将显示 "选择用户、计算机、服务帐户或组" 对话框。  在 "输入要选择的对象名称" 文本框中, 键入密钥管理组。  单击“确定”。
6.  在 "应用到" 列表框中, 选择 "**后代用户对象**"。
7.  使用滚动条滚动到页面底部, 然后单击 "全部清除 **"** 。
8.  在 "**属性**" 部分, 选择 "**读取 KeyCredentialLink** " 和 "**编写 KeyCredentialLink**"。

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>如果服务器未发送与 SSL 证书链中的所有中间证书, 则 Android 设备的新式验证为何会失败？

对于使用 Android ADAL 库失败的应用, 联合用户可能会遇到 Azure AD 的身份验证。 当应用程序尝试显示登录页时, 会收到**AuthenticationException** 。 在 chrome 中, AD FS 登录页可能被称为 "不安全"。

Android-跨所有版本和所有设备-不支持从证书的**authorityInformationAccess**字段下载其他证书。 这也适用于 Chrome 浏览器。 如果未从 AD FS 传递整个证书链, 则缺少中间证书的任何服务器身份验证证书都将导致此错误。

此问题的一个正确解决方案是将 AD FS 和 WAP 服务器配置为发送必要的中间证书和 SSL 证书。

从一台计算机导出要导入到 AD FS 和 WAP 服务器的 SSL 证书时, 请确保导出私钥, 然后选择 "**个人信息交换-PKCS #12**"。

必须选中 "**如果可能, 在证书路径中包括所有证书**" 复选框, 并**导出所有扩展属性**, 这一点很重要。  

在 Windows 服务器上运行 certlm.msc, 并导入 *。PFX 登录到计算机的个人证书存储。 这会导致服务器将整个证书链传递到 ADAL 库。

>[!NOTE]
> 还应更新网络负载平衡器的证书存储, 使其包含整个证书链 (如果存在)

### <a name="does-ad-fs-support-head-requests"></a>AD FS 是否支持 HEAD 请求？
AD FS 不支持 HEAD 请求。  应用程序不应使用 HEAD 请求来处理 AD FS 终结点。  这可能会导致意外和/或延迟的 HTTP 错误响应。  此外, 在 AD FS 事件日志中可能会出现意外错误事件。

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>为什么我在使用远程 IdP 登录时看不到刷新令牌？
如果 IdP 颁发的令牌的 validty 少于1小时, 则不会发出刷新令牌。 若要确保发出刷新令牌, 请将 IdP 发出的令牌的有效性提高到1小时以上。

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>是否有任何方法可更改 RP 令牌加密算法？
默认情况下, RP 令牌加密设置为 AES256, 并且无法将其更改为其他任何值。

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>在混合模式场中, 尝试使用 Set-adfssslcertificate-Thumbprint 设置新的 SSL 证书时出现错误。 如何在 AD FS 场中更新混合模式下的 SSL 证书？
混合模式 AD FS 场应为 transitionary 状态。 在计划中, 建议在升级过程之前滚动 SSL 证书, 或完成此过程并在更新 SSL 证书之前提高场行为级别。 如果未执行此操作, 则可以通过以下说明来更新 SSL 证书。 

在 WAP 服务器上, 仍然可以使用 WebApplicationProxySslCertificate。 在 ADFS 服务器上, 需要使用 netsh。 按照下面的步骤操作:

1. 选择用于维护的 ADFS 2016 服务器子集 (例如, 从负载均衡器中删除)
2. 在 #1 中选择的服务器上, 通过 MMC 导入新证书
3. 删除现有证书

    a. netsh http delete sslcert hostnameport =: 443 b。 netsh http delete sslcert hostnameport = localhost: 443 c。 netsh http delete sslcert hostnameport = 49443

4.  添加新证书

    a. netsh http add sslcert hostnameport = certhash: 443 = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

    b. netsh http add sslcert hostnameport = localhost: 443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

    c. netsh http add sslcert hostnameport = 49443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

5. 在选定的服务器上重启 ADFS 服务
6. 删除 WAP 服务器的子集以进行维护
7. 在所选 WAP 服务器上, 通过 MMC 导入新证书
8. 使用 cmdlet 在 WAP 上设置新证书

    a. WebApplicationProxySslCertificate-Thumbprint "CERTTHUMBPRINT"

9. 在所选 WAP 服务器上重新启动服务
10. 将所选 WAP 和 AD FS 服务器置于生产环境中。

以类似的方式在 AD FS 和 WAP 服务器的其余部分上执行更新。

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>当 Web 应用程序代理 (WAP) 服务器位于 Azure Web 应用程序防火墙 (WAF) 后时是否支持 ADFS？
ADFS 和 Web 应用程序服务器支持不在终结点上执行 SSL 终止的任何防火墙。 此外, ADFS/WAP 服务器使用内置机制来防止常见的 web 攻击, 如跨站点脚本、ADFS 代理, 并满足由[ADFSPIP 协议](https://msdn.microsoft.com/library/dn392811.aspx)定义的所有要求。

### <a name="i-am-seeing-an-event-441-a-token-with-a-bad-token-binding-key-was-found-what-should-i-do-to-resolve-this"></a>我看到的是 "事件 441:找到具有错误令牌绑定密钥的令牌。 如何解决此问题？
在 AD FS 2016 中, 令牌绑定会自动启用, 并导致代理和联合身份验证方案出现多个已知问题, 这会导致此错误。 若要解决此问题, 请运行以下 Powershell 命令, 并删除令牌绑定支持。

`Set-AdfsProperties -IgnoreTokenBinding $true`

### <a name="i-have-upgraded-my-farm-from-ad-fs-in-windows-server-2016-to-ad-fs-in-windows-server-2019-the-farm-behavior-level-for-the-ad-fs-farm-has-been-successfully-raised-to-2019-but-the-web-application-proxy-configuration-is-still-displayed-as-windows-server-2016"></a>我已经从 Windows Server 2016 中的 AD FS 将我的场升级到 Windows Server 2019 中 AD FS。 AD FS 场的场行为级别已成功提升为 2019, 但 Web 应用程序代理配置仍显示为 Windows Server 2016？
升级到 Windows Server 2019 后, Web 应用程序代理的配置版本将继续显示为 Windows Server 2016。 Web 应用程序代理没有适用于 Windows Server 2019 的特定版本特定功能, 如果在 AD FS 上成功引发了场行为级别, 则 Web 应用程序代理将继续按设计方式显示为 Windows Server 2016。 
