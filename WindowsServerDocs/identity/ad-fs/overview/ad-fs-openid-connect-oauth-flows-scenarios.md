---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: AD FS OpenID Connect/OAuth 流和应用程序方案
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e1e0235e50945fadd09fe9dd5ffeaf6d7119e482
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385604"
---
# <a name="ad-fs-openid-connectoauth-flows-and-application-scenarios"></a>AD FS OpenID Connect/OAuth 流和应用程序方案
适用于 AD FS 2016 及更高版本


|应用场景|使用示例的方案演练|OAuth 2.0 Flow/Grant|客户端类型|
|-----|-----|-----|-----|
|单页应用</br> | &bull;[使用 ADAL 的示例](../development/Single-Page-Application-with-AD-FS.md)|[式](#implicit-grant-flow)|Public| 
|登录用户的 Web 应用</br> | &bull;[使用 OWIN 的示例](../development/enabling-openid-connect-with-ad-fs.md)|[授权代码](#authorization-code-grant-flow)|公共、机密|  
|本机应用调用 Web API</br>|&bull;[使用 MSAL 的示例](../development/msal/adfs-msal-native-app-web-api.md)</br>&bull;[使用 ADAL 的示例](../development/native-client-with-ad-fs.md)|[授权代码](#authorization-code-grant-flow)|Public|   
|Web 应用调用 Web API</br>|&bull;[使用 MSAL 的示例](../development/msal/adfs-msal-web-app-web-api.md)</br>&bull;[使用 ADAL 的示例](../development/enabling-oauth-confidential-clients-with-ad-fs.md)|[授权代码](#authorization-code-grant-flow)|信息| 
|Web API 代表（OBO）用户调用另一个 web API</br>|&bull;[使用 MSAL 的示例](../development/msal/adfs-msal-web-api-web-api.md)</br>&bull;[使用 ADAL 的示例](../development/ad-fs-on-behalf-of-authentication-in-windows-server.md)|[代表](#on-behalf-of-flow)|Web 应用充当机密| 
|后台应用程序调用 Web API||[客户端凭据](#client-credentials-grant-flow)|信息| 
|Web 应用使用用户凭据调用 Web API||[资源所有者密码凭据](#resource-owner-password-credentials-grant-flow-not-recommended)|公共、机密| 
|Browserless 应用调用 Web API||[设备代码](#device-code-flow)|公共、机密| 

## <a name="implicit-grant-flow"></a>隐式授权流 
 
对于单页面应用程序（AngularJS、Ember、等），AD FS 支持 OAuth 2.0 隐式授予流。  [OAuth 2.0 规范](https://tools.ietf.org/html/rfc6749#section-4.2)中介绍了隐式流。 它的主要优点在于，它允许应用程序从 AD FS 获取令牌，而无需执行后端服务器凭据交换。 这允许应用在客户端 JavaScript 代码中登录用户、维护会话并获取其他 web Api 的令牌。 在专门围绕 [客户端](https://tools.ietf.org/html/rfc6749#section-10.3)使用隐式流时，需要考虑几个重要的安全注意事项。  
 
如果要使用隐式流和 AD FS 向 JavaScript 应用添加身份验证，请按照以下常规步骤进行操作。  
  
### <a name="protocol-diagram"></a>协议关系图

下图显示了整个隐式登录流的外观，并详细介绍了每个步骤。  

![隐式登录](media/adfs-scenarios-for-developers/implicit.png)

### <a name="request-id-token-and-access-token"></a>请求 ID 令牌和访问令牌 
 
若要最初将用户登录到应用，可以发送 OpenID Connect 身份验证请求，并从 AD FS 终结点获取 id_token 和访问令牌。  
 
```
// Line breaks for legibility only 
 
https://adfs.contoso.com/adfs/oauth2/authorize? 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&response_type=id_token+token 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&scope=openid 
&response_mode=fragment 
&state=12345 
```


|参数|必需/可选|描述| 
|-----|-----|-----|
|client_id|必需的|AD FS 分配给应用的应用程序（客户端） ID。| 
|response_type|必需的|必须包括 `id_token` 用于 OpenID connect 登录。 它还可能包含 response_type @ no__t-0。 使用此处的令牌可让应用立即从授权终结点接收访问令牌，而无需向令牌终结点发出第二次请求。| 
|redirect_uri|必需的|你的应用程序的 redirect_uri，你的应用程序可发送和接收身份验证响应。 它必须与你在 AD FS 中配置的其中一个 redirect_uris 完全匹配。| 
|现时|必需的|由应用生成且包含在请求中的值，将在生成的 id_token 中包含为声明。 然后, 应用程序可以验证此值, 以减少令牌重放攻击。 该值通常是随机的唯一字符串，可用于标识请求的来源。 仅在请求 id_token 时是必需的。|
|scope|可选|以空格分隔的范围列表。 对于 OpenID Connect，它必须包括范围 `openid`。|
|资源|可选|Web API 的 url。</br>注意-如果使用 MSAL 客户端库，则不发送 resource 参数。 而是将资源 url 作为作用域参数的一部分发送：`scope = [resource url]//[scope values e.g., openid]`</br>如果未在此处传递资源，或者在范围中，ADFS 将使用默认资源 urn： microsoft：用户。 不能自定义用户信息资源策略，如 MFA、颁发或授权策略。| 
|response_mode|可选| 指定应该用于将生成的令牌发送回应用的方法。 默认为 `fragment`。| 
|state|可选|请求中包含的值，也会在令牌响应中返回。 它可以是你想要的任何内容的字符串。 随机生成的唯一值通常用于防止跨站点请求伪造攻击。 状态还用于在身份验证请求出现之前，在应用程序中编码有关用户状态的信息，例如它们所在的页面或视图。| 
|prompt|可选|指示所需的用户交互的类型。 此时唯一有效的值是 "登录" 和 "无"。</br>- `prompt=login` 将强制用户在该请求上输入其凭据，取消单一登录。 </br>- `prompt=none` 相反，它将确保用户不会看到任何交互式提示。 如果请求无法通过单一登录静默完成，AD FS 将返回 interaction_required 错误。| 
|login_hint|可选|如果事先知道用户的用户名，可用于预先填充用户登录页的用户名/电子邮件地址字段。 通常，应用会在重新身份验证期间使用此参数，已使用中 `upn` `id_token`的 声明从以前的登录提取用户名。| 
|domain_hint|可选|如果包括，它将跳过用户在登录页面上经历的基于域的发现过程，从而使用户体验稍微更为简单。| 

此时，系统会要求用户输入其凭据并完成身份验证。 用户进行身份验证后，AD FS 授权终结点将使用 response_mode 参数中指定的方法，将响应返回到指定的 redirect_uri。  
 
### <a name="successful-response"></a>成功的响应 
 
 `response_mode=fragment and response_type=id_token+token`使用 的成功响应如下所示  
 
```
// Line breaks for legibility only 
 
GET https://localhost/myapp/# 
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZEstZnl0aEV... 
&token_type=Bearer 
&expires_in=3599 
&scope=openid  
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZstZnl0aEV1Q... 
&state=12345 
```


|参数|描述| 
|-----|-----|
|access_token|如果 response_type 包括 @ no__t-0，则包括。|
|token_type|如果 response_type 包括 @ no__t-0，则包括。 始终为持有者。| 
|expires_in| 如果 response_type 包括 @ no__t-0，则包括。 表示令牌有效的秒数，用于缓存目的。| 
|scope| 指示 access_token 将有效的作用域。|  
|id_token|如果 response_type 包括 @ no__t-0，则包括。 已签名的 JSON Web 令牌（JWT）。 应用可以解码此令牌的段，以请求有关登录用户的信息。 应用可以缓存并显示值，但不应依赖于这些值来获取任何授权或安全边界。| 
|state|如果请求中包含状态参数, 则响应中应显示相同的值。 应用应该验证请求和响应中的状态值是否相同。|

### <a name="refresh-tokens"></a>刷新令牌 
隐式授予不提供刷新令牌。  `id_tokens` 和 `access_tokens`将在短时间后过期，因此应用必须准备好定期刷新这些令牌。 若要刷新任一类型的令牌，可以使用 `prompt=none` 参数执行与上面相同的隐藏 iframe 请求，以控制标识平台的行为。 如果要接收`new id_token`，请确保使用 `response_type=id_token`。 

## <a name="authorization-code-grant-flow"></a>授权代码授予流 
 
可以在 web 应用中使用 OAuth 2.0 授权代码授予来访问受保护的资源，例如 web Api。 Oauth [2.0 规范的第4.1 节](https://tools.ietf.org/html/rfc6749)中介绍了 oauth 2.0 授权代码流。 它用于在大多数应用程序类型（包括 web 应用和本机安装的应用）中执行身份验证和授权。 流使应用程序能够安全地获取可用于访问 AD FS 信任资源的 access_tokens。  
 
### <a name="protocol-diagram"></a>协议关系图 
 
在高级别上，本机应用程序的身份验证流看起来有点类似于：

![授权代码授予流](media/adfs-scenarios-for-developers/authorization.png)

### <a name="request-an-authorization-code"></a>请求授权代码 
 
授权代码流始于客户端将用户定向到/authorize 终结点。 在此请求中，客户端指示它需要从用户获取的权限： 
 
```
// Line breaks for legibility only 
 
https://adfs.contoso.com/adfs/oauth2/authorize? 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&response_type=code 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&response_mode=query 
&resource=https://webapi.com/ 
&scope=openid 
&state=12345 
```

|参数|必需/可选|描述|
|-----|-----|-----| 
|client_id|必需的|AD FS 分配给应用的应用程序（客户端） ID。|  
|response_type|必需的| 必须包括授权代码流的代码。| 
|redirect_uri|必需的|应用`redirect_uri`的，可在其中通过应用发送和接收身份验证响应。 它必须与你在客户端 AD FS 中注册的其中一个 redirect_uris 完全匹配。|  
|资源|可选|Web API 的 url。</br>注意-如果使用 MSAL 客户端库，则不发送 resource 参数。 而是将资源 url 作为作用域参数的一部分发送：`scope = [resource url]//[scope values e.g., openid]`</br>如果未在此处传递资源，或者在范围中，ADFS 将使用默认资源 urn： microsoft：用户。 不能自定义用户信息资源策略，如 MFA、颁发或授权策略。| 
|scope|可选|以空格分隔的范围列表。|
|response_mode|可选|指定应该用于将生成的令牌发送回应用的方法。 可以是下列其中一项： </br>-query </br>-片段 </br>- form_post</br>`query` 提供代码作为重定向 URI 的查询字符串参数。 如果要请求代码，可以使用 query、片段或 form_post。  `form_post` @ no__t-1executes 将包含代码的 POST 发送到重定向 URI。|
|state|可选|请求中包含的值，也会在令牌响应中返回。 它可以是你想要的任何内容的字符串。 随机生成的唯一值通常用于防止跨站点请求伪造攻击。 值还可以在身份验证请求出现之前，在应用程序中编码有关用户状态的信息，例如它们所在的页面或视图。|
|prompt|可选|指示所需的用户交互的类型。 此时唯一有效的值是 "登录" 和 "无"。</br>- `prompt=login` 将强制用户在该请求上输入其凭据，取消单一登录。 </br>- `prompt=none` 相反，它将确保用户不会看到任何交互式提示。 如果请求无法通过单一登录静默完成，AD FS 将返回 interaction_required 错误。|
|login_hint|可选|如果事先知道用户的用户名，可用于预先填充用户登录页的用户名/电子邮件地址字段。 通常，应用会在重新身份验证期间使用此参数，已使用中 `upn` `id_token`的声明从以前的登录提取用户名。|
|domain_hint|可选|如果包括，它将跳过用户在登录页面上经历的基于域的发现过程，从而使用户体验稍微更为简单。|
|code_challenge_method|可选|用于对 code_challenge 参数的 code_verifier 进行编码的方法。 可以是以下值之一： </br>-纯 </br>- S256 </br>如果排除了 @ no__t-0 @ no__t-1，则 code_challenge 将被假定为纯文本。 AD FS 支持普通的和 S256。 有关详细信息，请参阅 [PKCE RFC](https://tools.ietf.org/html/rfc7636)。|
|code_challenge|可选| 用于通过来自 native client 的代码交换（PKCE）的证明密钥来保护授权代码授予。  `code_challenge_method`如果 包含，则为必需。 有关详细信息，请参阅 [PKCE RFC](https://tools.ietf.org/html/rfc7636)|

此时，系统会要求用户输入其凭据并完成身份验证。 用户进行身份验证后，AD FS 将使用 `redirect_uri` 参数 `response_mode`中指定的方法，将响应返回到指定的应用程序。  
 
### <a name="successful-response"></a>成功的响应 
 
使用 response_mode = query 的成功响应如下所示： 
 
```
GET https://adfs.contoso.com/common/oauth2/nativeclient? 
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq... 
&state=12345  
```


|参数|描述|
|-----|-----|
|code|`authorization_code`应用请求的。 应用可以使用授权代码请求目标资源的访问令牌。 Authorization_codes 的生存期较短，通常在约10分钟后过期。|
|state|如果请求中包含参数,则响应中应显示相同的值。`state` 应用应该验证请求和响应中的状态值是否相同。|

### <a name="request-an-access-token"></a>请求访问令牌 
 
现在，你已获取`authorization_code`并已获得用户的权限，你可以将的代码 `access_token` 兑换为所需资源。 通过向/token 终结点发送 POST 请求来实现此目的：  
 
```
// Line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com/ 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr... 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&grant_type=authorization_code 
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for confidential clients (web apps)  
```

|参数|必需/可选|描述|
|-----|-----|-----| 
|client_id|必需的|AD FS 分配给应用的应用程序（客户端） ID。| 
|grant_type|必需的|必须是 `authorization_code` 授权代码流的。| 
|code|必需的|在`authorization_code`流的第一个阶段中获取的。| 
|redirect_uri|必需的|用于获取`redirect_uri`的`authorization_code`相同值。| 
|client_secret|对于 web 应用是必需的|在 AD FS 中的应用注册期间创建的应用程序机密。 不应在本机应用中使用应用程序机密，因为 client_secrets 无法可靠地存储在设备上。 Web 应用和 web Api 是必需的，可将 client_secret 安全地存储在服务器端。 在发送客户端密码之前，必须对其进行 URL 编码。 这些应用还可以通过对 JWT 进行签名并将其添加为 client_assertion 参数来使用基于密钥的身份验证。| 
|code_verifier|可选|用于获取 authorization_code 的相同 @no__t 0。 如果在授权代码授予请求中使用了 PKCE，则为必需项。 有关详细信息，请参阅 [PKCE RFC](https://tools.ietf.org/html/rfc7636)。</br>Note –适用于2019及更高版本 AD FS| 

### <a name="successful-response"></a>成功的响应 
 
成功的令牌响应如下所示: 
 
```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...", 
    "token_type": "Bearer", 
    "expires_in": 3599, 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...", 
} 
```


|参数|描述| 
|-----|-----|
|access_token|请求的访问令牌。 应用可以使用此令牌向受保护的资源（Web API）进行身份验证。| 
|token_type|指示令牌类型值。 AD FS 支持的唯一类型是持有者。
|expires_in|访问令牌有效的时间长度 (以秒为单位)。
|refresh_token|OAuth 2.0 刷新标记。 应用可以使用此令牌在当前访问令牌过期之后获取其他访问令牌。 Refresh_tokens 的生存期很长，可用于长时间保留对资源的访问权限。| 
|refresh_token_expires_in|刷新令牌有效的时间长度（以秒为单位）。| 
|id_token|JSON Web 令牌（JWT）。 应用可以解码此令牌的段，以请求有关登录用户的信息。 应用可以缓存并显示值，但不应依赖于这些值来获取任何授权或安全边界。|

### <a name="use-the-access-token"></a>使用访问令牌 
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q... 
 ```

### <a name="refresh-the-access-token"></a>刷新访问令牌 
 
Access_tokens 的生存期很短，必须在过期后刷新，才能继续访问资源。 为此，可以将另一个 POST 请求提交到 @ no__t-0 @ no__t-1endpoint，这一次提供 refresh_token 而不是代码。 对于客户端已经收到的访问令牌的所有权限，刷新令牌都有效。 
 
刷新令牌没有指定的生存期。 通常，刷新令牌的生存期相对较长。 但是，在某些情况下，刷新令牌会过期、被吊销，或缺少足够的权限来执行所需的操作。 应用程序需要预期并正确处理令牌颁发终结点返回的错误。  
 
尽管刷新令牌在用于获取新的访问令牌时不会被吊销，但你应丢弃旧的刷新令牌。 OAuth 2.0 规范指出："授权服务器可能会发出一个新的刷新令牌，在这种情况下，客户端必须放弃旧的刷新令牌并将其替换为新的刷新令牌。 在向客户端发出新的刷新令牌后，授权服务器可能会撤销旧的刷新令牌。 
 
```
// Line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq... 
&grant_type=refresh_token 
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for confidential clients (web apps)  
```


|参数|必需/可选|描述| 
|-----|-----|-----|
|client_id|必需的|AD FS 分配给应用的应用程序（客户端） ID。| 
|grant_type|必需的|必须是 `refresh_token` 授权代码流的此阶段的。| 
|资源|可选|Web API 的 url。</br>注意-如果使用 MSAL 客户端库，则不发送 resource 参数。 而是将资源 url 作为作用域参数的一部分发送：`scope = [resource url]//[scope values e.g., openid]`</br>如果未在此处传递资源，或者在范围中，ADFS 将使用默认资源 urn： microsoft：用户。 不能自定义用户信息资源策略，如 MFA、颁发或授权策略。|
|scope|可选|以空格分隔的范围列表。| 
|refresh_token|必需的|在流的第二个阶段获取的 refresh_token。| 
|client_secret|对于 web 应用是必需的| 在应用程序注册门户中为应用创建的应用程序机密。 它不应用于本机应用，因为 client_secrets 无法可靠地存储在设备上。 Web 应用和 web Api 是必需的，可将 client_secret 安全地存储在服务器端。 这些应用还可以通过对 JWT 进行签名并将其添加为 client_assertion 参数来使用基于密钥的身份验证。|

### <a name="successful-response"></a>成功的响应 
成功的令牌响应如下所示: 
 
```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...", 
    "token_type": "Bearer", 
    "expires_in": 3599, 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...", 
}  
```
|参数|描述| 
|-----|-----|
|access_token|请求的访问令牌。 应用可以使用此令牌向受保护资源（例如 web API）进行身份验证。| 
|token_type|指示令牌类型值。 AD FS 支持的唯一类型是持有者|
|expires_in|访问令牌有效的时间长度 (以秒为单位)。|
|scope|Access_token 的有效范围。| 
|refresh_token|OAuth 2.0 刷新标记。 应用可以使用此令牌在当前访问令牌过期之后获取其他访问令牌。 Refresh_tokens 的生存期很长，可用于长时间保留对资源的访问权限。| 
|refresh_token_expires_in|刷新令牌有效的时间长度（以秒为单位）。| 
|id_token|JSON Web 令牌（JWT）。 应用可以解码此令牌的段，以请求有关登录用户的信息。 应用可以缓存并显示值，但不应依赖于这些值来获取任何授权或安全边界。|

## <a name="on-behalf-of-flow"></a>代理流 
 
OAuth 2.0 代理流（OBO）提供了一个应用程序，其中应用程序调用服务/web API，而后者又需要调用另一个服务/web API。 其思路是通过请求链传播委托用户标识和权限。 为了使中间层服务向下游服务发出经过身份验证的请求，需要代表用户保护 AD FS 的访问令牌。  
 
### <a name="protocol-diagram"></a>协议关系图 
假设已使用上述 OAuth 2.0 授权代码授予流在应用程序上对用户进行了身份验证。 此时，应用程序有一个访问令牌，其中包含用户的声明，并同意访问中间层 web API （API A）。 请确保客户端请求令牌中的 user_impersonation 范围。 现在，API A 需要向下游 web API （API B）发出经过身份验证的请求。 

接下来的步骤构成了 OBO 流，并通过下图的帮助进行了说明。 

![代理流](media/adfs-scenarios-for-developers/obo.png)

  1. 客户端应用程序使用令牌 A 向 API A 发出请求。  
  注意:在 AD FS 中配置 OBO flow 时，请`user_impersonation`确保选择范围并请求中`user_impersonation`的客户端执行请求范围。 
  2. API A 向 AD FS 令牌颁发终结点进行身份验证，并请求令牌访问 API B。注意：在 AD FS 中配置此流时，请确保 API A 也注册为服务器应用程序，而 clientID 与 API A 中的资源 ID 具有相同的值。有关更多详细信息，请参阅此处的示例 Add link。  
  3. AD FS 令牌颁发终结点使用令牌 A 验证 API A 的凭据，并颁发 API B 的访问令牌（令牌 B）。 
  4. 令牌 B 设置在向 API B 发出的请求的授权标头中。 
  5. API B 返回受保护资源中的数据。 

### <a name="service-to-service-access-token-request"></a>服务到服务访问令牌请求 
 
若要请求访问令牌，请使用以下参数向 AD FS 令牌终结点发出 HTTP POST。  


### <a name="first-case-access-token-request-with-a-shared-secret"></a>第一种情况:使用共享机密的访问令牌请求 
 
使用共享机密时, 服务到服务访问令牌请求包含以下参数: 


|参数|必需/可选|描述|
|-----|-----|-----| 
|grant_type|必需的|令牌请求的类型。 对于使用 JWT 的请求，此值必须为 urn： ietf： params： oauth： grant 类型： JWT-持有者。|  
|client_id|必需的|将第一个 Web API 注册为服务器应用时配置的客户端 ID （中间层应用）。 这应该与第一个阶段中使用的资源 ID 相同，即第一个 Web API 的 url。| 
|client_secret|必需的|在 AD FS 中的服务器注册过程中创建的应用程序机密。| 
|断言|必需的|请求中使用的令牌的值。|  
|requested_token_use|必需的|指定应如何处理请求。 在 OBO 流中，该值必须设置为 on_behalf_of| 
|资源|必需的|将第一个 Web API 注册为服务器应用时提供的资源 ID （中间层应用）。 资源 ID 应为第二个 Web API 中间层应用程序的 url，它将代表客户端调用。|
|scope|可选|用空格分隔的令牌请求范围列表。| 

#### <a name="example"></a>示例 
 
以下`HTTP POST`请求访问令牌和刷新令牌 
 
```
//line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: adfs.contoso.com  
Content-Type: application/x-www-form-urlencoded 
 
grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer 
&client_id=https://webapi.com/ 
&client_secret=BYyVnAt56JpLwUcyo47XODd 
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIm… 
&resource=https://secondwebapi.com/
&requested_token_use=on_behalf_of
&scope=openid    
```

### <a name="second-case-access-token-request-with-a-certificate"></a>第二种情况:使用证书访问令牌请求 
 
带有证书的服务到服务访问令牌请求包含以下参数: 


|参数|必需/可选|描述|
|-----|-----|-----| 
|grant_type|必需的|令牌请求的类型。 对于使用 JWT 的请求，此值必须为 urn： ietf： params： oauth： grant 类型： JWT-持有者。 |
|client_id|必需的|将第一个 Web API 注册为服务器应用时配置的客户端 ID （中间层应用）。 这应该与第一个阶段中使用的资源 ID 相同，即第一个 Web API 的 url。|  
|client_assertion_type|必需的|该值必须为 urn： ietf： params： oauth： client-assertion： jwt-持有者。| 
|client_assertion|必需的|一个断言（一个 JSON web 令牌），需要使用注册为应用程序凭据的证书进行创建和签名。|  
|断言|必需的|请求中使用的令牌的值。| 
|requested_token_use|必需的|指定应如何处理请求。 在 OBO 流中，该值必须设置为 on_behalf_of| 
|资源|必需的|将第一个 Web API 注册为服务器应用时提供的资源 ID （中间层应用）。 资源 ID 应为第二个 Web API 中间层应用程序的 url，它将代表客户端调用。|
|scope|可选|用空格分隔的令牌请求范围列表。|


请注意，这些参数与通过共享机密请求几乎相同，只不过 client_secret 参数替换为以下两个参数： client_assertion_type 和 client_assertion。 

#### <a name="example"></a>示例 
以下 HTTP POST 请求使用证书的 Web API 的访问令牌。

``` 
// line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer 
&client_id= https://webapi.com/ 
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer 
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNS… 
&resource=https://secondwebapi.com/
&requested_token_use=on_behalf_of
&scope= openid 
```    

### <a name="service-to-service-access-token-response"></a>服务到服务访问令牌响应 
 
成功响应是具有以下参数的 JSON OAuth 2.0 响应。 


|参数|描述|
|-----|-----| 
|token_type|指示令牌类型值。 AD FS 支持的唯一类型是持有者。 | 
|scope|令牌中授予的访问权限的范围。| 
|expires_in|访问令牌有效的时间长度（以秒为单位）。| 
|access_token|请求的访问令牌。 调用服务可以使用此令牌向接收方服务进行身份验证。| 
|id_token|JSON Web 令牌（JWT）。 应用可以解码此令牌的段，以请求有关登录用户的信息。 应用可以缓存并显示值，但不应依赖于这些值来获取任何授权或安全边界。| 
|refresh_token|请求的访问令牌的刷新令牌。 在当前访问令牌过期后, 调用服务可以使用此令牌请求另一个访问令牌。|
|Refresh_token_expires_in|刷新令牌有效的时间长度（以秒为单位）。 

### <a name="success-response-example"></a>成功响应示例 
 
下面的示例演示对 web API 的访问令牌请求的成功响应。 

``` 
{ 
  "token_type": "Bearer", 
  "scope": openid, 
  "expires_in": 3269, 
  "access_token": "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1t" 
  "id_token": "aWRfdG9rZW49ZXlKMGVYQWlPaUpLVjFRaUxDSmhiR2NpT2lKU1V6STFOa" 
  "refresh_token": "OAQABAAAAAABnfiG…" 
  "refresh_token_expires_in": 28800, 
} 
```  
 
 
使用访问令牌访问受保护的资源现在，中间层服务可以使用上面获取的令牌向下游 web API 发出身份验证请求，方法是在授权标头中设置令牌。  

#### <a name="example"></a>示例 
``` 
GET /v1.0/me HTTP/1.1 
Host: https://secondwebapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQ… 
``` 

## <a name="client-credentials-grant-flow"></a>客户端凭据授予流 
 
你可以使用[RFC 6749](https://tools.ietf.org/html/rfc6749#section-4.4)中指定的 OAuth 2.0 客户端凭据授予来使用应用程序的标识访问 web 承载的资源。 这种类型的授权通常用于必须在后台运行的服务器到服务器交互，而不会立即与用户交互。 这些类型的应用程序通常称为 "守护程序" 或 "服务帐户"。 

OAuth 2.0 客户端凭据授权流允许 web 服务（机密客户端）在调用其他 web 服务时使用其自己的凭据（而不是模拟用户）进行身份验证。 在此方案中，客户端通常是中间层 web 服务、后台程序服务或网站。 为了获得更高的保障级别，AD FS 还允许调用服务使用证书（而不是共享机密）作为凭据。 

### <a name="protocol-diagram"></a>协议关系图 

下图显示了客户端凭据授予流。 

![客户端凭据](media/adfs-scenarios-for-developers/credentials.png)

### <a name="request-a-token"></a>请求令牌 
 
若要使用客户端凭据 grant 获取令牌，请将`POST`请求发送到/token AD FS 终结点：  
 
### <a name="first-case-access-token-request-with-a-shared-secret"></a>第一种情况:使用共享机密的访问令牌请求 
 
```
POST /adfs/oauth2/token HTTP/1.1            
//Line breaks for clarity 
 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
client_id=535fb089-9ff3-47b6-9bfb-4f1264799865 
&client_secret=qWgdYAmab0YSkuL1qKv5bPX 
&grant_type=client_credentials 
```

|参数|必需/可选|描述|
|-----|-----|-----| 
|client_id|必需的|AD FS 分配给应用的应用程序（客户端） ID。| 
|scope|可选|希望用户同意的范围的空格分隔列表。| 
|client_secret|必需的|在应用注册门户中为应用生成的客户端密码。 在发送客户端密码之前，必须对其进行 URL 编码。| 
|grant_type|必需的|必须设置为 `client_credentials`。|

### <a name="second-case-access-token-request-with-a-certificate"></a>第二种情况:使用证书访问令牌请求 

``` 
POST /adfs/oauth2/token HTTP/1.1                
 
// Line breaks for clarity 
 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05 
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer 
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg 
&grant_type=client_credentials  
```

|参数|必需/可选|描述| 
|-----|-----|-----|
|client_assertion_type|必需的|该值必须设置为 urn： ietf： params： oauth： client-assertion： jwt-持有者。| 
|client_assertion|必需的|一个断言（一个 JSON web 令牌），需要使用注册为应用程序凭据的证书进行创建和签名。|  
|grant_type|必需的|必须设置为 `client_credentials`。|
|client_id|可选|AD FS 分配给应用的应用程序（客户端） ID。 这是 client_assertion 的一部分，因此不需要在此处传递。| 
|scope|可选|希望用户同意的范围的空格分隔列表。| 

### <a name="use-a-token"></a>使用令牌 
 
获取令牌后，请使用令牌向资源发出请求。 当令牌过期时，对/token 终结点重复此请求以获取新的访问令牌。  
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...  
```

## <a name="resource-owner-password-credentials-grant-flow-not-recommended"></a>资源所有者密码凭据授予流（不推荐） 
 
资源所有者密码凭据（ROPC） grant 允许应用程序通过直接处理密码来登录用户。 ROPC 流需要高度的信任和用户暴露，只应在其他更安全的流不能使用时使用此流。  
 
### <a name="protocol-diagram"></a>协议关系图 
 
下图显示了 ROPC 流。

![ROPC flow](media/adfs-scenarios-for-developers/resource.png)

### <a name="authorization-request"></a>授权请求 
ROPC 流是单个请求，它将客户端标识和用户的凭据发送到 IDP，然后接收返回的令牌。 在执行此操作之前，客户端必须请求用户的电子邮件地址（UPN）和密码。 成功请求后，客户端应立即从内存中安全地释放用户的凭据。 它永远不能保存它们。  

```
// Line breaks and spaces are for legibility only. 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com  
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&scope= openid  
&username=myusername@contoso.com 
&password=SuperS3cret 
&grant_type=password 
```


|参数|必需/可选|描述| 
|-----|-----|-----|
|client_id|必需的|客户端 ID| 
|grant_type|必需的|必须设置为 password。| 
|username|必需的|用户的电子邮件地址。| 
|password|必需的|用户的密码。| 
|scope|可选|以空格分隔的范围列表。|

### <a name="successful-authentication-response"></a>身份验证成功响应 
下面的示例显示成功的令牌响应： 

```
{ 
    "token_type": "Bearer", 
    "scope": "openid", 
    "expires_in": 3599, 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIn...", 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDR..." 
}  
```


|参数|描述| 
|-----|-----|
|token_type|始终设置为持有者。| 
|scope|如果返回访问令牌，此参数会列出访问令牌有效的范围。| 
|expires_in|包含的访问令牌有效的秒数。| 
|access_token|为请求的作用域颁发。| 
|id_token|JSON Web 令牌（JWT）。 应用可以解码此令牌的段，以请求有关登录用户的信息。 应用可以缓存并显示值，但不应依赖于这些值来获取任何授权或安全边界。| 
|refresh_token_expires_in|包含的刷新令牌对有效的秒数。| 
|refresh_token|如果原始作用域参数包含 offline_access，则发出。|

你可以使用刷新令牌获取新的访问令牌，并使用上面的 "身份验证代码授予流" 一节中所述的相同流来刷新令牌。   

## <a name="device-code-flow"></a>设备代码流 
 
设备代码授予允许用户登录到受输入约束的设备，例如智能电视、IoT 设备或打印机。 若要启用此流, 设备需要用户在另一台设备上的浏览器中访问某个网页, 以便登录。 用户登录后, 设备能够根据需要获取访问令牌和刷新令牌。 
 
### <a name="protocol-diagram"></a>协议关系图 
 
整个设备代码流类似于下图。 本文稍后介绍每个步骤。 
 
![设备代码流](media/adfs-scenarios-for-developers/device.png)

### <a name="device-authorization-request"></a>设备授权请求 
客户端必须首先向身份验证服务器核实用于启动身份验证的设备和用户代码。 客户端从/devicecode 终结点收集此请求。 在此请求中, 客户端还应包括从用户获取所需的权限。 从发送此请求的那一刻起，用户只有15分钟的时间登录（expires_in 的常用值），因此，仅当用户指示他们已准备好登录时才发出此请求。 

```
// Line breaks are for legibility only. 
 
POST https://adfs.contoso.com/adfs/oauth2/devicecode 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
scope=openid 
```


|参数|条件|描述|
|-----|-----|-----| 
|client_id|必需的|AD FS 分配给应用的应用程序（客户端） ID。| 
|scope|可选|以空格分隔的范围列表。|

### <a name="device-authorization-response"></a>设备授权响应 
成功的响应是一个 JSON 对象, 其中包含允许用户登录所需的信息。 


|参数|描述|
|-----|-----| 
|device_code|用于验证客户端和授权服务器之间的会话的长字符串。 客户端使用此参数来请求授权服务器的访问令牌。| 
|user_code|显示给用户的简短字符串, 用于标识辅助设备上的会话。| 
|verification_uri|用户为了登录而应与 user_code 一起提供的 URI。| 
|verification_uri_complete|用户为了登录而应与 user_code 一起提供的 URI。 这是通过 user_code 预填充的，因此用户无需输入 user_code| 
|expires_in|Device_code 和 user_code 过期之前的秒数。| 
|间隔|客户端在轮询请求之间应等待的秒数。| 
|消息|用户可读的字符串, 其中包含有关用户的说明。 这可以通过在格式为 "有关 = xx-XX" 的请求中包括查询参数来进行本地化，并填写相应的语言区域性代码。  

### <a name="authenticating-the-user"></a>对用户进行身份验证 
收到 user_code 和 verification_uri 之后，客户端会向用户显示这些用户，指示他们使用移动电话或 PC 浏览器登录。 此外，客户端可以使用 QR 码或类似的机制来显示 verfication_uri_complete，这将采取进入用户的 user_code 的步骤。 当用户在 verification_uri 上进行身份验证时，客户端应使用 device_code 轮询请求令牌的/token 终结点。 

```
POST https://adfs.contoso.com /adfs/oauth2/token 
Content-Type: application/x-www-form-urlencoded 
 
grant_type: urn:ietf:params:oauth:grant-type:device_code 
client_id: 6731de76-14a6-49ae-97bc-6eba6914391e 
device_code: GMMhmHCXhWEzkobqIHGG_EnNYYsAkukHspeYUk9E8 
```


|参数|必需的|描述|
|-----|-----|-----| 
|grant_type|必需的|必须为 urn： ietf： params： oauth： grant-type： device_code| 
|client_id|必需的|必须与初始请求中使用的 client_id 匹配。| 
|code|必需的|设备授权请求中返回的 device_code。|

### <a name="successful-authentication-response"></a>身份验证成功响应 
成功的令牌响应如下所示:  


|参数|描述|
|-----|-----| 
|token_type|始终为 "持有者"。| 
|scope|如果返回访问令牌, 则会列出访问令牌有效的范围。| 
|expires_in|在包含的访问令牌有效之前经过的秒数。| 
|access_token|为请求的作用域颁发。| 
|id_token|当原始作用域参数包含 openid 作用域时发出。| 
|refresh_token|如果原始作用域参数包含 offline_access，则发出。| 
|refresh_token_expires_in|为提供的刷新令牌有效之前等待的秒数。| 


## <a name="related-content"></a>相关内容  
请参阅演练[AD FS 开发](../AD-FS-Development.md)，其中提供了有关如何使用相关流的分步说明。 
