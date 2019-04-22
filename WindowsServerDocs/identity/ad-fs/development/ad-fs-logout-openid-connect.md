---
title: 使用 AD FS 的 OpenID Connect 单个注销
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3af10ec139edbc72e75bf80f544ac5b4f1cf9222
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825768"
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>使用 AD FS 的 OpenID Connect 单个注销

## <a name="overview"></a>概述
基于 Windows Server 2012 R2 中的 AD FS 中的初始 Oauth 支持，AD FS 2016 引入了对 OpenId Connect 登录支持。 与[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)，AD FS 2016 现在支持单一注销的 OpenId Connect 方案。 这篇文章 OpenId Connect 方案提供单一注销的概述，并提供有关如何使用 OpenId Connect 应用程序中的 AD FS 的指南。


## <a name="discovery-doc"></a>发现文档
OpenID Connect 使用名为"发现文档"的 JSON 文档提供有关配置详细信息。  这包括身份验证、 令牌、 userinfo 和公共终结点的 Uri。  下面是发现文档的示例。

```
{
"issuer":"https://fs.fabidentity.com/adfs",
"authorization_endpoint":"https://fs.fabidentity.com/adfs/oauth2/authorize/",
"token_endpoint":"https://fs.fabidentity.com/adfs/oauth2/token/",
"jwks_uri":"https://fs.fabidentity.com/adfs/discovery/keys",
"token_endpoint_auth_methods_supported":["client_secret_post","client_secret_basic","private_key_jwt","windows_client_authentication"],
"response_types_supported":["code","id_token","code id_token","id_token token","code token","code id_token token"],
"response_modes_supported":["query","fragment","form_post"],
"grant_types_supported":["authorization_code","refresh_token","client_credentials","urn:ietf:params:oauth:grant-type:jwt-bearer","implicit","password","srv_challenge"],
"subject_types_supported":["pairwise"],
"scopes_supported":["allatclaims","email","user_impersonation","logon_cert","aza","profile","vpn_cert","winhello_cert","openid"],
"id_token_signing_alg_values_supported":["RS256"],
"token_endpoint_auth_signing_alg_values_supported":["RS256"],
"access_token_issuer":"http://fs.fabidentity.com/adfs/services/trust",
"claims_supported":["aud","iss","iat","exp","auth_time","nonce","at_hash","c_hash","sub","upn","unique_name","pwd_url","pwd_exp","sid"],
"microsoft_multi_refresh_token":true,
"userinfo_endpoint":"https://fs.fabidentity.com/adfs/userinfo",
"capabilities":[],
"end_session_endpoint":"https://fs.fabidentity.com/adfs/oauth2/logout",
"as_access_token_token_binding_supported":true,
"as_refresh_token_token_binding_supported":true,
"resource_access_token_token_binding_supported":true,
"op_id_token_token_binding_supported":true,
"rp_id_token_token_binding_supported":true,
"frontchannel_logout_supported":true,
"frontchannel_logout_session_supported":true
} 
 
```



以下的其他值将为发现文档，用于指示对前端通道注销的支持中提供：

- frontchannel_logout_supported： 值将为 'true'
- frontchannel_logout_session_supported： 值将为 'true'。
- end_session_endpoint： 这是 OAuth 注销客户端可以使用启动注销的服务器上的 URI。


## <a name="ad-fs-server-configuration"></a>AD FS 服务器配置
默认情况下，将启用 AD FS 属性 EnableOAuthLogout。  此属性告知使用 SID 来启动客户端上的注销的 AD FS 服务器，若要浏览的 URL (LogoutURI)。 如果还没有[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)安装可以使用以下 PowerShell 命令：

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> `EnableOAuthLogout` 参数将标记为过时安装后[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)。 `EnableOAUthLogout` 始终为 true，将不会影响对注销功能。

>[!NOTE]
>支持 frontchannel_logout**仅**的安装后[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>客户端配置
客户端必须实现注销已登录用户的 url。 管理员可以使用以下 PowerShell cmdlet 在客户端配置来配置 LogoutUri。 


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

`LogoutUri`是 AF FS 用于""将用户登录的 url。 用于实现`LogoutUri`，以确保它的客户端需要清除应用程序中的用户的身份验证状态，例如，删除身份验证令牌，它有。 AD FS 将浏览到该 URL，与作为查询参数发出信号的信赖方的 SID / 应用程序中注销用户。 

![](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)


1.  **使用会话 ID 的 OAuth 令牌**:AD FS 时的 id_token 令牌颁发 OAuth 令牌中包含会话 id。 这将用于更高版本的 AD FS 标识相关的 SSO cookie，若要清理的用户。
2.  **用户启动在 App1 上的注销**:用户可以启动从任何已登录的应用程序注销。 在此示例方案中，用户启动从 App1 注销。
3.  **应用程序将注销请求发送到 AD FS**:用户启动注销后，应用程序将 GET 请求发送到的 AD FS end_session_endpoint。 应用程序可以选择性地包含 id_token_hint，作为此请求的参数。 如果 id_token_hint 存在，AD FS 将中使用它与会话 ID 一起使用来找出哪些 URI 客户端应重定向到后注销 (post_logout_redirect_uri)。  Post_logout_redirect_uri 应是有效的 uri 注册到 AD FS 使用 RedirectUris 参数。
4.  **向登录的客户端注销的 AD FS 发送**:AD FS 使用的会话标识符值以查找用户登录到的相关客户端。 标识客户端上注册到 AD FS 以启动客户端上的注销 LogoutUri 发送请求。

## <a name="faqs"></a>常见问题
**问：** 看不到在发现文档中的 frontchannel_logout_supported 和 frontchannel_logout_session_supported 参数。</br>
**答：** 请确保已[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)所有 AD FS 服务器上安装。 单一注销与 Server 2016 中是指[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)。

**问：** 我已配置了单一注销的指引，但用户保持登录的其他客户端上。</br>
**答：** 确保`LogoutUri`设置的所有客户端登录的用户所在。 AD FS 还，执行发送注销请求的已注册的最有利情况下尝试`LogoutUri`。 客户端必须实现逻辑以处理请求并从应用程序将用户转到注销的操作。</br>

**问：** 如果注销之后，客户端之一可回溯到具有有效的刷新令牌的 AD FS，将 AD FS 可以颁发访问令牌？</br>
**答：** 是。 要接收的注销请求后将删除所有经过身份验证的项目的客户端应用程序负责在已注册`LogoutUri`。


## <a name="next-steps"></a>后续步骤
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  
