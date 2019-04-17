---
title: "对于 OpenID 联系广告 FS 单个登出"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3af10ec139edbc72e75bf80f544ac5b4f1cf9222
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>对于 OpenID 联系广告 FS 单个登出

## <a name="overview"></a>概述
在 Windows Server 2012 R2 的广告 FS 初始 Oauth 支持上生成，广告 FS 2016 引入 OpenId 连接登录的支持。 与[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)，广告 FS 2016 现在支持 OpenId 连接的情况下的单个注销。 这篇文章 OpenId 连接情景中提供的单个登出概述，并提供有关如何使用它购买你 OpenId 连接中的应用程序广告 FS 指南。


## <a name="discovery-doc"></a>发现文档
OpenID 连接使用称为"发现文档"JSON 文档提供配置的详细信息。  这包括 Uri 身份验证、标记、用户信息，以及公众端点。  以下是一种发现文档。

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



以下额外值中可发现文档，以指示前置信道注销的支持：

- frontchannel_logout_supported：值将真
- frontchannel_logout_session_supported：将真的值。
- end_session_endpoint：这是第 OAuth 注销 URI，可使用客户端初始化注销服务器上的。


## <a name="ad-fs-server-configuration"></a>广告 FS 服务器配置
广告 FS 属性 EnableOAuthLogout 将默认启用。  此属性与 SID 启动注销客户端上的告诉广告 FS 服务器浏览的 URL (LogoutURI)。 如果你没有[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)已安装，你可以使用下面的 PowerShell 命令：

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> `EnableOAuthLogout` 参数将标记为已过时安装后[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)。 `EnableOAUthLogout` 将始终如此，并且会产生任何影响注销功能。

>[!NOTE]
>受支持 frontchannel_logout**仅**安装后[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>客户端配置
客户需要实现注销已登录的用户的 url。 管理员可以使用下面的 PowerShell cmdlet 客户配置中配置 LogoutUri。 


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

`LogoutUri`是 AF FS 用于"注销"用户的 url。 实现`LogoutUri`的客户需求，确保它将清除应用程序中的用户身份验证状态、放身份验证，如令牌具有。 广告 FS 将浏览到该 URL 与 SID 为查询参数，信号依赖方 / 应用程序用户注销。 

![](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)


1.  **会话 id OAuth 令牌**：广告 FS 包括 id 会话中的 OAuth 标记 id_token 令牌发放次。 这将用于稍后 AD fs 识别来清理对于用户的相关 SSO cookie。
2.  **用户启动上 App1 注销**：用户都可以启动一次注销从任何已登录的应用程序。 在此示例情况下，用户启动一次从 App1 注销。
3.  **应用程序向广告 FS 发送注销请求**：用户启动注销后，应用程序会向的广告 FS end_session_endpoint 发送获取请求。 作为此请求的参数，应用程序（可选）可以包含 id_token_hint。 如果存在 id_token_hint，广告 FS 将使用它结合会话 ID 图出哪些 URI 客户端应后重定向到注销 (post_logout_redirect_uri)。  Post_logout_redirect_uri 应有效 uri 与广告 FS 使用 RedirectUris 参数注册。
4.  **将退出广告 FS 发送给记录客户端**: 会话标识符值，以查找相关的客户端用户登录到的广告 FS 使用。 识别信息的客户端上与广告 FS 启动一次在客户端注销注册 LogoutUri 发送请求。

## <a name="faqs"></a>常见问题
**问：**看不到发现文档的 frontchannel_logout_supported 和 frontchannel_logout_session_supported 参数。</br>
**答：**确保你拥有[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)安装所有广告 FS 服务器上。 请参阅与 Server 2016 中的单个登出[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)。

**问：**我已经配置单个注销按照，但用户保持登录其他客户端上。</br>
**答：**确保`LogoutUri`已设置为所有客户端用户所在登录。 此外，广告 FS 无法发送注销请求上的已注册的最佳状况尝试`LogoutUri`。 客户端，必须实现处理请求和采取行动注销从应用程序用户的逻辑。</br>

**问：**如果注销后，一个客户端回退到具有有效的刷新标记广告 FS，将会广告 FS 发出访问令牌？</br>
**答：**是。 负责后收到注销请求断开所有经过身份验证的项目的客户端应用程序在注册`LogoutUri`。


## <a name="next-steps"></a>后续步骤
[广告 FS 开发](../../ad-fs/AD-FS-Development.md)  
