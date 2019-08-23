---
title: AD FS OpenID Connect/OAuth 概念
description: 了解 AD FS 新式身份验证概念。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7c25a2faba9b660ddba9439c519dcbf51847ef0a
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2019
ms.locfileid: "69983665"
---
# <a name="ad-fs-openid-connectoauth-concepts"></a>AD FS OpenID Connect/OAuth 概念
适用于 AD FS 2016 及更高版本
 
## <a name="modern-authentication-actors"></a>新式身份验证参与者 

|参与者| 描述|
|-----|-----|
|最终用户|这是需要访问资源的安全主体 (用户、应用程序、服务和组)。|  
|客户端|这是你的 web 应用程序, 由其客户端 ID 标识。 客户端通常是最终用户与之交互的参与方, 并从授权服务器请求令牌。
|授权服务器/标识提供者 (IdP)| 这是你的 AD FS 服务器。 它负责验证组织目录中存在的安全主体的标识。 成功对这些安全主体进行身份验证后, 它会颁发安全令牌 (持有者访问令牌、ID 令牌和刷新令牌)。
|资源服务器/资源提供程序/信赖方| 这是资源或数据所在的位置。 它信任授权服务器, 以安全地对客户端进行身份验证和授权, 并使用持有者访问令牌来确保可以授予对资源的访问权限。

下图提供了执行组件之间的最基本关系:

![新式身份验证参与者](media/adfs-modern-auth-concepts/concept1.png)

## <a name="application-types"></a>应用程序类型 
 

|应用程序类型|描述|Role|
|-----|-----|-----|
|本机应用程序|有时称为 "**公共客户端**", 这旨在作为在电脑或设备上运行并与用户交互的客户端应用。|请求授权服务器 (AD FS) 提供的用于访问资源的用户的令牌。 使用令牌作为 HTTP 标头, 将 HTTP 请求发送到受保护的资源。| 
|服务器应用程序 (Web 应用)|在服务器上运行并且用户通常通过浏览器访问的 web 应用程序。 由于它可以维护自己的客户端 "机密" 或凭据, 因此有时称为**机密客户端**。 |请求授权服务器 (AD FS) 提供的用于访问资源的用户的令牌。 请求令牌之前, 客户端 (Web 应用) 需要使用其机密进行身份验证。 | 
|Web API|用户正在访问的最终资源。 将这些作为 "信赖方" 的新表示形式。|使用客户端获取的持有者访问令牌| 

## <a name="application-group"></a>应用程序组 
 
配置有 AD FS 的每个 OAuth 客户端 (本机或 web 应用) 或资源 (web api) 都需要与应用程序组相关联。 可以将应用程序组中的客户端配置为访问同一组中的资源。 一个应用程序组可包含多个客户端和资源。  

## <a name="security-tokens"></a>安全令牌 
 
新式身份验证使用以下令牌类型: 
- **id_token**: 由授权服务器 (AD FS) 颁发并由客户端使用的 JWT 令牌。 ID 令牌中的声明将包含有关用户的信息, 以便客户端可以使用该信息。  
- **access_token**: 由授权服务器 (AD FS) 颁发并供资源使用的 JWT 令牌。 此令牌的 "aud" 或听众声明必须与资源或 Web API 的标识符匹配。  
- **refresh_token**: 这是由 AD FS 颁发的令牌, 供客户端在需要刷新 id_token 和 access_token 时使用。 令牌对客户端是不透明的, 并且只能由 AD FS 使用。  

## <a name="scopes"></a>作用 
 
在 AD FS 中注册资源时, 可以将作用域配置为允许 AD FS 执行特定操作。 除了配置范围之外, 还需要在请求中发送范围值, 以便 AD FS 执行该操作。 例如, 管理员需要在资源注册过程中将范围配置为 openid, 应用程序 (客户端) 需要将范围 = openid 发送到身份验证请求中的 AD FS 颁发 ID 令牌。 下面提供了 AD FS 中可用范围的详细信息 
 
- aza-如果 对 [代理客户端使用 OAuth 2.0 协议扩展](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706), 并且 scope 参数包含作用域 "aza", 则服务器将发出新的主刷新令牌并在响应的 refresh_token 字段中设置该令牌, 并将如果强制执行, 则为新的主刷新令牌的生存期的 refresh_token_expires_in 字段。 
- openid-允许应用程序请求使用 OpenID Connect 授权协议。 
- logon_cert-logon_cert 范围允许应用程序请求登录证书, 这些证书可用于以交互方式登录经过身份验证的用户。 AD FS 服务器忽略响应中的 access_token 参数, 而是提供 base64 编码的 CMS 证书链或 CMC 完整 PKI 响应。  [此处](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e)提供了更多详细信息。
- user_impersonation-必须使用 user_impersonation 范围, 才能成功地从 AD FS 请求代表访问令牌。 有关如何使用此作用域的详细信息, 请参阅使用[OAuth 作为 AD FS 2016 的代表构建多层应用程序 (OBO)](ad-fs-on-behalf-of-authentication-in-windows-server.md)。 
- allatclaims – allatclaims 范围允许应用程序在访问令牌中请求要添加到 ID 令牌中的声明。   
- vpn_cert-vpn_cert 范围允许应用程序请求 VPN 证书, 这些证书可用于通过 EAP-TLS 身份验证建立 VPN 连接。 这不再受支持。 
- 电子邮件-允许应用程序请求已登录用户的电子邮件声明。  
- 配置文件-允许应用程序请求登录用户的配置文件相关声明。  

## <a name="claims"></a>声明 
 
AD FS 颁发的安全令牌 (访问和 ID 令牌) 包含有关已进行身份验证的使用者的声明或断言。 应用程序可以对各种任务使用声明, 其中包括: 
- 验证令牌 
- 标识使用者的目录租户 
- 显示用户信息 
- 确定使用者的授权任何给定安全令牌中存在的声明都依赖于令牌的类型、用于对用户进行身份验证的凭据类型和应用程序配置。  
 
## <a name="high-level-ad-fs-authentication-flow"></a>高级别 AD FS 身份验证流 

![AD FS 身份验证流](media/adfs-modern-auth-concepts/adfsauthflow.png)


 1. AD FS 接收来自客户端的身份验证请求。  
 
 2. AD FS 验证身份验证请求中的客户端 ID 和客户端 ID 在 AD FS 中的客户端和资源注册过程中获取的客户端 id。 如果使用机密客户端, 则 AD FS 还会验证身份验证请求中提供的客户端密码。 AD FS 还验证客户端的重定向 uri。 
 
 3. AD FS 标识客户端要通过身份验证请求中传递的资源参数访问的资源。 如果使用 MSAL 客户端库, 则不发送 resource 参数。 而是作为作用域参数的一部分发送资源 url:*范围 = [资源 url]//[范围值, openid]* 。 

    如果未使用资源或作用域参数传递资源, ADFS 将使用默认资源 urn: 无法配置策略 (例如, MFA、颁发或授权策略) 的用户信息。 
 
 4. 接下来 AD FS 验证客户端是否有权访问该资源。 AD FS 还验证在身份验证请求中传递的作用域是否与注册资源时配置的作用域相匹配。 如果客户端没有权限, 或者未在身份验证请求中发送正确的作用域, 则将终止身份验证流。   
 
 5. 验证权限和作用域后, AD FS 使用已配置的[身份验证方法](../operations/configure-authentication-policies.md)对用户进行身份验证。   

 6. 如果根据资源策略或全局身份验证策略需要[其他身份验证方法](../operations/configure-additional-authentication-methods-for-ad-fs.md), AD FS 会触发附加身份验证。 

 7. AD FS 使用[AZURE mfa](../operations/configure-ad-fs-and-azure-mfa.md)或[第三方 MFA](../operations/additional-authentication-methods-ad-fs.md)来执行身份验证。   
 
 8. 对用户进行身份验证后, AD FS 将应用[声明规则](../deployment/configuring-claim-rules.md)(确定作为安全令牌的一部分发送给资源的声明) 和[访问控制策略](../operations/ad-fs-client-access-policies.md)(确定用户已满足访问资源所需的条件)。   

 9. 接下来, AD FS 生成访问令牌和刷新令牌。 

 10. AD FS 还会生成 ID 令牌。 
 
 11. 如果身份验证请求中包含范围 = allatclaims,[则会自定义 ID 令牌](custom-id-tokens-in-ad-fs.md), 以在基于定义的声明规则的访问令牌中包含声明。 
    
 12. 生成并自定义所需的令牌后, AD FS 对包括令牌的客户端的响应。 仅当身份验证请求包括范围 = openid 时, 响应中包含 ID 标记。 客户端始终可以使用令牌终结点获取身份验证后的 ID 令牌。 

## <a name="types-of-libraries"></a>库的类型 
  
两种类型的库与 AD FS 一起使用: 
- **客户端库**:本机客户端和服务器应用程序使用客户端库获取访问令牌, 以调用资源 (如 Web API)。 使用 AD FS 2019 时, Microsoft 身份验证库 (MSAL) 是最新和推荐的客户端库。 建议为 2016 AD FS Active Directory 身份验证库 (ADAL)。  

- **服务器中间件库**:Web 应用使用服务器中间件库进行用户登录。 Web Api 使用服务器中间件库验证本机客户端或其他服务器发送的令牌。 建议的中间件库是 OWIN (用于 .NET 的开放式 Web 界面)。 

## <a name="customize-id-token-additional-claims-in-id-token"></a>自定义 ID 令牌 (ID 令牌中的其他声明)
 
在某些情况下, Web 应用 (客户端) 可能需要 ID 令牌中的其他声明来帮助实现此功能。 可以通过使用以下选项之一来实现此目的。 

**选项 1：** 使用公用客户端时, 如果 web 应用没有尝试访问的资源, 则应使用。 选项要求 
1.  response_mode 设置为 form_post 
2.  信赖方标识符 (Web API 标识符) 与客户端标识符相同

![AD FS 自定义令牌选项1](media/adfs-modern-auth-concepts/option1.png)

**选项 2：** 当 web 应用有一个尝试访问的资源, 并且需要通过 ID 令牌传递其他声明时, 应使用。 可以使用公共和机密客户端。 选项要求 
1.  response_mode 设置为 form_post 
2.  KB4019472 安装在 AD FS 服务器上 
3.  分配给客户端– RP 对的作用域 allatclaims。 可以使用 ADFSApplicationPermission (使用 AdfsApplicationPermission (如果已授予一次) PowerShell cmdlet 来分配作用域, 如以下示例中所示: 

    ``` powershell
    Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
    ```

![AD FS 自定义令牌选项2](media/adfs-modern-auth-concepts/option2.png)

若要更好地了解如何在 ADFS 中配置 Web 应用以获取自定义的 ID 令牌, 请参阅[使用 OpenID connect 或 OAuth 与 AD FS 2016 或更高版本时, 自定义要在 id_token 中发出的声明](Custom-Id-Tokens-in-AD-FS.md)。

## <a name="single-log-out"></a>单一登录

单个注销会导致使用会话 id 结束所有客户端会话。AD FS 2016 和更高版本支持 OpenID Connect/OAuth 的单一登录。 有关更多详细信息, 请参阅用于[OpenID connect 的单一注销 AD FS](ad-fs-logout-openid-connect.md)。


## <a name="ad-fs-endpoints"></a>AD FS 终结点

|AD FS 终结点|描述|
|-----|-----|
|/authorize|AD FS 返回可用于获取访问令牌的授权代码|
|/token|AD FS 返回可用于访问资源的访问令牌 (Web API)|
|/userinfo|AD FS 返回有关经过身份验证的用户的声明|
|/devicecode|AD FS 返回设备代码和用户代码|
|/logout|AD FS 注销用户|
|/keys|用于对响应进行签名 AD FS 公钥|
|/.well-known/openid-configuration|AD FS 返回 OAuth/OpenID Connect 元数据|
