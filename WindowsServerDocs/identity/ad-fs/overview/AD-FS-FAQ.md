---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: "广告 FS 2016 常见问题"
description: "对于广告 FS 2016：常见问题"
author: jenfieldmsft
ms.author: billmath
manager: femila
ms.date: 03/06/2018
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 313447d2c92c15505434ec5c39898ca84aef46db
ms.sourcegitcommit: 556361fe7c73c75d6cdc46a67dc88679fbe89c61
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>广告 FS 常见问题（常见问题）

>适用于：Windows Server 2016

以下文档可能会在 Active Directory 联合身份验证服务方面的常见问题的家庭。  文档已分为组根据类型的问题。

## <a name="deployment"></a>部署 

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>如何可以升级月迁移从以前版本的广告 FS
你可以升级广告 FS 使用下列情况之一：


- Windows Server 2012 对 Windows Server 2016 广告 FS R2 广告 FS
    - [升级到 Windows Server 2016 使用 WID 数据库广告 FS](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [升级到 Windows Server 2016 使用 SQL 数据库广告 FS](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 对 Windows Server 2012 R2 广告 FS 广告 FS
    - [迁移到在 Windows Server 2012 R2 上广告 FS](https://technet.microsoft.com/library/dn486815.aspx)
- 对 Windows Server 2012 广告 FS 广告 FS 2.0
    - [Windows Server 2012 上的广告 FS 到迁移](https://technet.microsoft.com/library/jj647765.aspx)
- 广告 FS 1.x 广告 FS 2.0 到 
    - [从广告 FS 升级到广告 FS 2.0 1.x](https://technet.microsoft.com/library/ff678035.aspx)

如果你需要升级从广告 FS 2.0 或 2.1（Windows Server 2008 R2 或 Windows Server 2012），必须使用（位于 C:\Windows\ADFS）的收件箱脚本。

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>广告 FS 安装为什么需要重新启动服务器？

HTTP 月 2 的支持已添加 Windows Server 2016，但 HTTP 2 月不能用于客户证书身份验证。  因为许多广告 FS 方案中使客户证书身份验证、使用和客户端执行操作的大量不支持重新尝试收取费用请求使用 HTTP 月 1.1 广告 FS 电场的日落配置重新配置到 HTTP 月 1.1 本地服务器 HTTP 设置。  这需要重新启动的服务器。  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>使用 Windows 2016 WAP 服务器发布到 Internet 的广告 FS 电场的日落，无需升级后端广告 FS 电场的日落受支持？
是，此配置支持，但会在这种配置支持广告 FS 2016 的新功能。  此配置旨在为在从广告 FS 2012 R2 迁移阶段广告 FS 2016 到临时并不应部署很长一段时间。

## <a name="design"></a>设计

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>哪些第三方多重身份验证的提供商是否可用于广告 FS？ 
下面是我们会注意到的第三方提供商的列表。  始终可能提供商的可用，我们不知道，并且我们将更新列表中，当我们获知它们。

- [Gemalto 身份与安全服务](http://www.gemalto.com/identity)
- [inWebo 企业级身份验证服务](http://www.inwebo.com/)
- [登录人脉 MFA API 接头](https://www.loginpeople.com)
- [Microsoft Active Directory 联合身份验证服务 RSA SecurID 验证代理](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)
- [广告 fs SafeNet 身份验证服务 (SAS) 代理](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)
- [Swisscom 移动版 ID 身份验证服务](http://swisscom.ch/mid)
- [Symantec 验证和 ID 保护服务 (VIP)](http://www.symantec.com/vip-authentication-service) 

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>第三方代理支持广告 FS？
是的第三方代理服务器可以将放置放 Web 应用程序代理服务器，但必须支持任何第三方代理[MS ADFSPIP 协议](https://msdn.microsoft.com/library/dn392811.aspx)来替代 Web 应用程序代理。

### <a name="what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip"></a>适用于支持 MS ADFSPIP 的广告 FS 哪些第三方代理？

下面是我们会注意到的第三方提供商的列表。  始终可能提供商的可用，我们不知道，并且我们将更新列表中，当我们获知它们。

- [F5 访问策略管理器](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)

### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>在正在容量计划大小电子表格的广告 FS 2016？
可以下载电子表格的广告 FS 2016 版本[此处](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)。
这也可用于在 Windows Server 2012 R2 的广告 FS。

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>如何确保我广告金融服务和 WAP 服务器支持 Apple 的 ATP 要求？

Apple 发布了一套称为应用传输安全性 (ATS) 可能会影响 iOS 应用到广告 FS 进行身份验证电话的要求。  你可以确保你的广告 FS 并 WAP 服务器遵循通过确保他们所支持[要求使用 ATS 连接](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57)。  
特别是，你应该验证您的广告金融服务和 WAP 服务器支持 TLS 1.2 和 TLS 连接协商的密码套件将支持完美向前保密。

你可以启用和禁用 1.0，1.1、1.2 SSL 2.0 3.0 和 TLS 版本使用[广告 FS 中管理 SSL 协议](../operations/Manage-SSL-Protocols-in-AD-FS.md)。

若要确保您的广告和 WAP 请服务器协调仅 TLS 密码套件支持 ATP，，您可以禁用中并非在所有密码套件[ATP 符合密码套件列表](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57)。  若要执行此操作，请使用[Windows TLS PowerShell cmdlet](https://technet.microsoft.com/itpro/powershell/windows/tls/index)。 


## <a name="operations"></a>操作

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>如何替换广告 FS SSL 证书？ 
广告 FS SSL 证书不是广告 FS 管理贴靠中找到广告 FS 服务通信证书相同。  若要更改的广告 FS SSL 证书，你将需要使用 PowerShell。 按照下面的文章中的指南：

[管理 SSL 广告金融服务和 WAP 2016 中的证书](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>如何启用或禁用广告 fs TLS 月 SSL 设置
若要禁用或启用 SSL 协议和密码套件，使用以下方法：

[管理广告 FS 中的 SSL 协议](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>代理 SSL 证书是否一定要广告 FS SSL 证书相同？  
使用以下方面的代理 SSL 证书和广告 FS SSL 证书指南：


- 如果该代理使用到使用 Windows 集成验证的代理 SSL 证书的代理广告 FS 请求必须相同（使用同一个键）联合身份验证的服务器 SSL 证书
- 如果广告 FS 属性"ExtendedProtectionTokenCheck"已启用（设置中广告 FS 默认），代理 SSL 证书必须相同（使用同一个键）联合身份验证的服务器 SSL 证书
- 否则为代理 SSL 证书有不同的密钥从广告 FS SSL 证书，但必须满足相同[要求](../overview/AD-FS-2016-Requirements.md)

### <a name="how-can-i-configure-promptlogin-behavior-for-ad-fs"></a>如何配置提示符广告 fs = 登录行为？
有关如何配置提示符 = 登录，请参阅[Active Directory 联合身份验证服务提示 = 登录参数支持](../operations/AD-FS-Prompt-Login.md)。

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>如何将配置浏览器与广告 FS 使用 Windows 集成身份验证 (WIA)？

有关如何配置浏览器的信息，请参阅[配置浏览器与广告 FS 使用 Windows 集成身份验证 (WIA)](../operations/Configure-AD-FS-Browser-WIA.md)。


### <a name="how-long-are-ad-fs-tokens-valid"></a>多长时间有多大广告 FS 标记有效？

此问题通常意味着时长执行用户获取单一登录 (SSO) 而无需输入新的凭据，并如何以管理员身份控制，？  这种情况，和配置设置来控制它，述文章[此处](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings)。

下面列出的各种 cookie 和令牌默认寿命了（以及参数控制生存期）：

**已注册的设备**

- Prt 的参照和 SSO cookie: 90 天内的最大，由 PSSOLifeTimeMins。 （提供的设备使用至少每个 14 天后，它受 DeviceUsageWindow）

- 刷新令牌：计算基于上述提供一致的行为

- access_token: 1 小时默认情况下，根据依赖方

- id_token：相同访问标记为

**未注册的设备**

- SSO cookie: SSOLifetimeMins 默认情况下的 8 小时约束。  启用保持我已登录 (KMSI) 后，默认为 24 小时，可以通过 KMSILifetimeMins 配置。


- 刷新令牌：默认情况下的 8 小时。 24 小时内 KMSI 启用

- access_token: 1 小时默认情况下，根据依赖方

- id_token：相同访问标记为

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>广告 FS 是否支持 HTTP 严格传输安全性 (HSTS)？  

HTTP 严格传输安全性 (HSTS) 是一 web 的安全策略机制，这有助于减少协议降级攻击和 cookie 拦截有 HTTP 和 HTTPS 端点的服务。 它使 web 服务器来声明，web 浏览器（或其他符合要求用户代理）应仅交互永远不会通过 HTTP 协议方式和与之使用 HTTPS。

仅通过 HTTPS 打开所有广告 FS 端点 web 身份验证的交通。  因此，广告 FS 有效地减少 HTTP 严格传输安全性策略机制提供威胁（设计没有任何降级到 HTTP 由于 HTTP 中不有任何侦听程序）。 此外，广告 FS 标记所有 cookie 与安全标志发送到另一个具有 HTTP 协议端点服务器阻止 cookie。

因此，广告 FS 服务器上实现 HSTS 则不需要的因为它可永远不会降级。  遵从性的广告 FS 服务器满足以下要求，因为它们可以永远不会使用 HTTP，并且所有 cookie 都标记安全。

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X ms-转发的客户端-ip 不包含的客户端 IP，但包含 IP 的代理服务器放防火墙。 在哪里可以获取客户端右的 IP？
不建议执行之前 WAP SSL 终止。 放 WAP 完成 SSL 终止，以防 X ms-转发的客户端-ip 将包含放 WAP 网络设备的 IP。 下面介绍各种 IP 的简要说明相关受广告 FS 的索赔：
 - X ms 客户端 ip：网络的设备连接到 STS IP。  如果外部网站请求始终可以包含 WAP IP。
 - X ms-转发的客户端-ip：将包含通过 Exchange Online 以及 WAP 到连接的设备的 IP 地址转发到 ADFS 任何值多值的索赔。
 - Userip：联网请求此声明将包含 x ms-转发的客户端-ip 值。  对于 intranet 请求，此声明将包含 x ms 客户端 ip 相同的值。

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>我将尝试获取其他索赔上的用户信息端点，但它只退回主题。 如何才能获得其他索赔？
ADFS 用户信息端点始终返回主题声明规定 OpenID 标准。 广告 FS 不提供其他索赔通过用户信息端点请求。 如果你需要在 ID 令牌其他索赔，请参阅[广告 FS 中自定义 ID 标记](../development/custom-id-tokens-in-ad-fs.md)。

### <a name="why-do-i-see-alot-of-1021-errors-on-my-ad-fs-servers"></a>为什么我广告 FS 服务器上看到了大量 1021 年错误？
广告 FS 的资源 00000003-0000-0000-c000-000000000000 的无效的资源访问的通常记录该事件。 此错误被致的客户端尝试 Azure AD 图服务获得访问的令牌出现错误的行为。 由于资源上不存在广告 FS，这将导致事件 ID 1021 广告 FS 服务器上。 它是安全地忽略任何警告或错误的资源 00000003-0000-0000-c000-000000000000 广告 FS 上。 

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>为什么我会看到警告失败添加对企业键管理员组广告 FS 服务帐户？
此组仅时 FSMO PDC 角色与 Windows 2016 域控制器在域中存在创建。 若要解决此错误，你可以手动创建组，然后按照下方提供所需的权限后添加为组成员服务帐户。
1.  打开**Active Directory 用户和计算机**。 
2.  **右键单击**导航窗格从你的域名和**单击**属性。
3.  **单击**安全（如果缺少的安全选项卡上，将打开高级功能从查看菜单）。
4.  **单击**高级。 **单击**添加。 **单击**选择主体。
5.  选择用户、计算机、服务帐户或组对话框。  在来选择文本框中输入的对象名称中，键入键管理员组。  单击确定。
6.  在适用于列表框中，选择**后代用户对象**。
7.  使用滚动条，滚动到页面的底部和**单击**清除所有。
8.  在**属性**部分中，选择**阅读 msDS KeyCredentialLink**和**编写 msDS KeyCrendentialLink**。

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>从 Android 设备现代身份验证为何如果服务器不在使用 SSL 的证书链发送所有中间证书？

联盟的用户可能会遇到的身份验证到 Azure AD 使用 Android ADAL 库失败的应用。 获取该应用将**AuthenticationException**时它将尝试来显示登录页。 在 chrome 广告 FS 登录页可能称为为不安全。

Android-跨所有版本和所有设备不支持下载从其他证书**authorityInformationAccess**字段证书。 这也是如此 Chrome 浏览器。 如果不将整个的证书链传递从广告 FS，缺少中间证书任何服务器身份验证证书将导致此错误。 

正确解决此问题是配置广告金融服务和 WAP 服务器发送以及 SSL 证书必要中间证书。 

从一台计算机，若要导入到计算机的个人的应用商店的广告金融服务和 WAP 服务器，导出 SSL 证书，请确保导出专用键，选择**个人信息 Exchange-PKCS #12**。 

非常重要的复选框到**如果可能，包括所有证书中的证书路径**处于选中状态，以及**导出全部扩展的属性**。 

运行 Windows 的服务器上 certlm.msc 和导入 *。PFX 到计算机的个人证书的应用商店。 这将导致传递到 ADAL 库整个的证书链的服务器。 

>[!NOTE]
> 此外应更新包括证书官方商城的网络负载平衡以包含整个的证书链如果存在

### <a name="does-ad-fs-support-head-requests"></a>广告 FS 是否支持头请求？
广告 FS 不支持头请求。  不应使用应用程序对广告 FS 端点头请求。  这可能导致意外和/或延迟的 HTTP 错误响应。  此外，你可能会看到广告 FS 事件日志中的意外的错误事件。

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>为什么我不会看到刷新令牌我已登录使用远程 IdP 时？
如果颁发 IdP 的标记有少于 1 小时的 validty 没有颁发刷新标记。 若要确保发出刷新标记，增加为多 1 小时颁发的 IdP 标记的有效性。
