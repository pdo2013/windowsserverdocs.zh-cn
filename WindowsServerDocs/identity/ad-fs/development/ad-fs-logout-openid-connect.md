---
title: 使用 AD FS 的 OpenID Connect 单个注销
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5f0127e60243ca81f7e25282adc79e01c54b4b32
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407850"
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>使用 AD FS 的 OpenID Connect 单个注销

## <a name="overview"></a>概述
AD FS 2016 在 Windows Server 2012 R2 的 AD FS 中的初始 Oauth 支持基础上构建，引入了对 OpenId Connect 登录的支持。 对于[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)，AD FS 2016 现在支持针对 OpenId connect 方案的单一登录。 本文概述了 OpenId Connect 方案的单一登录，并提供了有关如何在 AD FS 中使用 OpenId connect 应用程序的指导。


## <a name="discovery-doc"></a>发现文档
OpenID Connect 使用称为 "发现文档" 的 JSON 文档来提供有关配置的详细信息。  这包括身份验证、令牌、用户信息和公用终结点的 Uri。  下面是发现文档的一个示例。

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



以下附加值将在发现文档中提供，以指示支持前声道注销：

- frontchannel_logout_supported：值将为 "true"
- frontchannel_logout_session_supported：值将为 "true"。
- end_session_endpoint：这是客户端可用于在服务器上启动注销的 OAuth 注销 URI。


## <a name="ad-fs-server-configuration"></a>AD FS 服务器配置
默认情况下，将启用 AD FS 属性 EnableOAuthLogout。  此属性告知 AD FS 服务器通过 SID 查找要在客户端上启动注销的 URL （LogoutURI）。 如果尚未安装[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) ，可以使用以下 PowerShell 命令：

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> 安装[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)后，参数 @no__t 将被标记为过时。 `EnableOAUthLogout` 将始终为 true，并且不会影响注销功能。

>[!NOTE]
>frontchannel_logout**仅**支持 Vcredist 的[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>客户端配置
客户端需要实现一个 url，该 url 是已登录用户的 "注销"。 管理员可以使用以下 PowerShell cmdlet 在客户端配置中配置 LogoutUri。 


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

@No__t-0 是 AF FS 用于 "注销" 用户的 url。 对于实现 `LogoutUri`，客户端需要确保它清除应用程序中用户的身份验证状态，例如，删除其拥有的身份验证令牌。 AD FS 将浏览到该 URL，其中包含 SID 作为查询参数，并通知信赖方/应用程序注销用户。 

![](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)


1.  **包含会话 ID 的 OAuth 令牌**：AD FS 在 id_token 令牌颁发时在 OAuth 令牌中包含会话 id。 稍后 AD FS 将使用此方法来确定要为用户清理的相关 SSO cookie。
2.  **用户在 App1 上启动注销**：用户可以从任何已登录的应用程序启动注销。 在此示例方案中，用户启动了 App1 的注销。
3.  **应用程序将注销请求发送到 AD FS**：用户启动注销后，应用程序会将 GET 请求发送到 AD FS 的 end_session_endpoint。 应用程序可以选择包含 id_token_hint 作为此请求的参数。 如果 id_token_hint 存在，AD FS 会将其与会话 ID 结合使用，以确定注销后应将客户端重定向到的 URI （post_logout_redirect_uri）。  Post_logout_redirect_uri 应是使用 RedirectUris 参数注册 AD FS 的有效 uri。
4.  **AD FS 将注销发送到已登录的客户端**：AD FS 使用会话标识符值查找用户登录到的相关客户端。 标识的客户端将在 LogoutUri AD FS 注册的上发送请求，以在客户端启动注销。

## <a name="faqs"></a>常见问题
**：** 我在发现文档中看不到 frontchannel_logout_supported 和 frontchannel_logout_session_supported 参数。</br>
**答：** 确保所有 AD FS 服务器上都安装了[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) 。 请参阅服务器2016中的单一注销，其中包含[KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)。

**：** 我已经按顺序配置了单一注销，但用户仍在其他客户端上保持登录。</br>
**答：** 确保为用户登录的所有客户端设置 `LogoutUri`。 此外，AD FS 会尝试在已注册的 @no__t 上发送注销请求。 客户端必须实现逻辑来处理请求，并采取措施从应用程序中注销用户。</br>

**：** 如果在注销后，其中一个客户端将返回到 AD FS 并且使用有效的刷新令牌，将 AD FS 发出访问令牌？</br>
**答：** 是。 在注册 `LogoutUri` 接收到注销请求之后，客户端应用程序负责删除所有经过身份验证的项目。


## <a name="next-steps"></a>后续步骤
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  
