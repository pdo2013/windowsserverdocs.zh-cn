---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: 面向开发人员的 AD FS 方案
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3a3156eefc4af52fb7daefb618c689b78fef5efc
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188816"
---
# <a name="ad-fs-scenarios-for-developers"></a>面向开发人员的 AD FS 方案


Windows Server 2016 [AD FS 2016] 中的 AD FS，可添加的行业标准 OpenID Connect 和 OAuth 2.0 基于身份验证和授权对应用程序进行开发，并让用户直接对 AD FS 进行身份验证这些应用程序。    
  
AD FS 2016 还支持 Ws-federation、 Ws-trust 和 SAML 协议和配置文件我们已在早期版本中受支持。  如果您有兴趣对这些协议的开发人员指南，请参阅以下文章。  本文将重点介绍如何使用和受益于更高版本的协议支持。  
  
## <a name="why-modern-authentication"></a>为什么新式身份验证  
虽然您可以继续使用 AD FS 进行登录上与 WS 联合身份验证，WS 信任和 SAML 协议，就像您具有之前，请使用较新协议时，你可以获取下列权益：  
  
* **保持简单性和一致性**  
    * 使用同一组 Api 和模式以启用登录为：   
        *   多个类型的应用程序 （服务器、 桌面、 移动、 浏览器）  
        *   多个平台 (android、 iOS、 Windows)  
        *   在企业网络内部的应用程序或托管在云中  
    * 使用一组相同的库已可用于针对 Azure AD 的用户进行身份验证  
* **大的灵活性**  
    * 除了标准用户授权，如启用更复杂的方案：      
        * ? 3 重式登录流在其中用户授权一个 web 应用程序或服务位于与另一个 web 应用或服务的访问资源。    
        * ? 服务器到服务器流中的中间层服务访问后端 API  
        * ? 基于 JavaScript 的单页应用程序 (SPA)  
* **行业支持**  
    * OAuth 2.0 和 OpenID Connect 享受宽利用率跨行业的这样一种模式的知识将帮助你启用身份验证和 Active Directory 环境外的授权  
  
## <a name="how-it-works-the-basics"></a>它的工作原理：基础知识  
可以将 AD FS 新式身份验证添加到应用程序使用的相同的一套工具和库已可用于针对 Azure AD 的用户进行身份验证。   
  
在 AD FS 方案当然，它是 AD FS，并不是 Azure AD 作为标识提供者和授权服务器。  否则概念是完全相同的： 用户提供其凭据并获取令牌，直接或通过中介，对资源进行访问。  
  
最基本的方案包括用户或"资源所有者"，并浏览器来访问 web 应用程序进行交互的：  
  
![面向开发人员的 AD FS](media/ADFS_DEV_1.png)  
  
Web 应用程序称为"客户端"，因为它对资源的访问令牌启动对授权服务器 (AD FS) 的请求。  资源可能托管的 web 应用程序本身也可能是网络或 internet 上的某个位置的 web API 的可访问性。   "资源所有者"的用户授权的客户端 web 应用，可通过提供凭据向授权服务器接收该访问令牌。    
  
## <a name="how-it-works-components"></a>其工作原理： 组件  
OAuth 2.0 和 OpenID Connect 方案中 AD FS 进行的同一组工具和库使用 Azure AD 是标识提供程序时使用。  这些组件包括：  
* Active Directory 身份验证库 (ADAL): 客户端库的促进收集用户凭据、 创建和提交令牌请求和检索生成的令牌。    
* (Open Web Interface for.NET) OWIN 中间件：OWIN 是一个基于社区项目，而 Microsoft 开发了一组服务器端库，用于保护 web 应用程序和 web Api 使用 OpenID Connect 和 OAuth 2.0  
  
下图显示了这些组件的角色：  
  
![面向开发人员的 AD FS](media/ADFS_DEV_2.png)  
  
## <a name="modeling-these-scenarios-in-ad-fs-2016"></a>在 AD FS 2016 中建模这些方案  
  
### <a name="application-groups"></a>应用程序组  
若要表示 AD FS 策略在这些方案，我们引入了名为应用程序组的新概念。  应用程序组可以包含任意数量和应用程序的以下基本类型的组合：  
  
  
  
应用程序组 / 应用程序类型  |描述  |角色    
---------|---------|---------  
本机应用程序     |  有时称为公共客户端，目的是为运行在电脑或设备上和与用户进行交互的客户端应用。       | 请求资源的授权服务器 (AD FS) 的用户的访问权限的令牌。  将 HTTP 请求发送到受保护的资源，为 HTTP 标头中使用的标记。        
服务器应用程序     |   在服务器上运行，通常可供用户通过浏览器访问 web 应用程序。  因为它是可维护其自己的客户端机密或凭据，它有时称为机密客户端。      | 请求资源的授权服务器 (AD FS) 的用户的访问权限的令牌。  将 HTTP 请求发送到受保护的资源，为 HTTP 标头中使用的标记。               
Web API     |  访问用户的最终资源。 将它们视为新的表示形式的"信赖方"。| 使用客户端获取的令牌  
  
### <a name="differences-from-ad-fs-2012-r2"></a>从 AD FS 2012 R2 的差异  
应用程序组将 AD FS 2012 R2 分别公开为信赖方、 客户端和应用程序权限的信任和授权元素组合。  
  
下表比较依据相应应用程序信任对象中创建 AD FS 2012 R2 AD FS 2016 vs 的方法：  
  
Windows Server 2012 R2 中的 AD FS|在 PowerShell 中|AD FS 管理  
---------|---------|---------  
添加本机客户端|Add-AdfsClient|NA  
添加服务器应用程序作为客户端|Add-AdfsClient|NA  
添加 Web API / 资源|Add-AdfsRelyingPartyTrust|创建信赖方信任  
  
AD FS 2016|在 PowerShell 中|AD FS 管理  
---------|---------|---------  
添加本机客户端|Add-AdfsNativeClientApplication|添加本机应用程序到应用程序组  
添加服务器应用程序作为客户端|Add-AdfsServerApplication|添加服务器应用程序到应用程序组  
添加 Web API / 资源|Add-AdfsWebApiApplication|添加 Web API 应用程序到应用程序组  
  
### <a name="application-permissions-and-consent"></a>应用程序权限和许可  
默认情况下，允许应用程序组中的客户端访问同一组中的资源。  管理员无需配置特定的应用程序的权限。  应用程序组还允许管理员可以指定允许，如 openid 或 user_impersonation 作用域。  下面的方案说明指定确切的作用域所需的哪种方案。  
  
由于 AD FS 使用的模型管理员同意的情况下，不会提示用户同意访问资源时。  通过配置应用程序组，管理员实际上提供了代表应用程序的所有用户同意。  
  
## <a name="supported-scenarios"></a>支持的方案  
以下部分介绍了我们更详细地支持的方案。  
  
### <a name="tokens-used"></a>使用的标记  
这些方案都要使用的三个标记类型：  
  
* **id_token:** JWT 令牌，用于表示用户的标识。 Id_token 的 aud 或受众声明匹配的本机或服务器应用程序的客户端 ID。  
* **access_token:** 使用的 JWT 令牌中的 Oauth 和 OpenID connect 的方案和预期使用的资源。  此令牌的 aud 或受众声明必须匹配的资源或 Web API 的标识符。  
* **refresh_token:** 此令牌提交来代替收集用户凭据来提供单一登录体验。  此令牌进行颁发和供 AD FS 和不可读的客户端或资源。    
  
### <a name="native-client-to-web-api"></a>本机客户端到 Web API  
此方案，要调用的 AD FS 2016 受保护的 Web API 的本机客户端应用程序的用户。  
* 本机客户端应用程序使用 ADAL 发送授权和令牌向 AD FS 中，提示输入凭据时根据需要，用户请求后将其发送到 Web API 请求的 HTTP 标头作为生成的令牌  
* [此部分是仅用于演示目的]Web API 从客户端，发送的访问令牌的结果，将其发送回客户端的 ClaimsPrincipal 对象读取声明。  
  
![协议流的说明](media/ADFS_DEV_3.png)  
  
1.  本机客户端应用程序启动的 ADAL 库调用流。  这会触发基于浏览器到 AD FS HTTP GET 的授权终结点：  
  
**授权请求：**  
获取 https://fs.contoso.com/adfs/oauth2/authorize?  
  
参数|值  
---------|---------  
response_type|"代码"  
资源|RP ID （标识符） 的应用程序组中的 Web API  
client_id|客户端应用程序组中的本机应用程序 Id  
redirect_uri|应用程序组中的本机应用程序的重定向 URI  
  
**授权请求的响应：**  
如果用户未登录之前，系统会提示用户输入凭据。    
AD FS 通过作为 redirect_uri 的查询组件中的"code"参数返回授权代码进行响应。  例如：HTTP/1.1 302 已找到位置：  **http://redirect_uri:80/?code=&lt; 代码&gt;。**  
  
2.  本机客户端然后将代码，以及以下参数发送到 AD FS 令牌终结点：  
  
**令牌请求：**  
发布 https://fs.contoso.com/adfs/oauth2/token  
  
参数|ReplTest1  
---------|---------  
grant_type|"authorization_code" 
code|从 1 的授权代码  
资源|RP ID （标识符） 的应用程序组中的 Web API  
client_id|客户端应用程序组中的本机应用程序 Id  
redirect_uri|应用程序组中的本机应用程序的重定向 URI  
  
**令牌请求的响应：**  
AD FS 使用 access_token，refresh_token，并在正文中的 id_token 使用 HTTP 200 响应。  
  
3.  然后，本机应用程序将作为 HTTP 请求中的 Authorization 标头的 access_token，上述响应一部分发送到 web API。  
  
### <a name="single-sign-on-behavior"></a>在行为上的单一登录  
（默认情况下） 的 1 小时内的 access_token 仍将有效在缓存中，以及新的请求不会触发到 AD FS 的任何流量，后续的客户端请求中。  通过 ADAL 从缓存将会自动将提取 access_token。  
  
访问令牌过期后，ADAL 将自动刷新令牌基于的请求发送到 AD FS 令牌终结点 （正在跳过授权请求自动）。  
**刷新令牌请求：**  
发布 https://fs.contoso.com/adfs/oauth2/token
   

参数|ReplTest1|
---------|---------
grant_type|"refresh_token"|
资源|RP ID （标识符） 的应用程序组中的 Web API|
client_id|客户端应用程序组中的本机应用程序 Id
refresh_token|初始令牌请求的响应中的 AD FS 颁发的刷新令牌

  
  
**刷新令牌请求响应：**  
如果 < SSO_period > 内的刷新令牌，则请求将导致新的访问令牌。 不提示用户输入凭据。  有关详细信息 SSO 设置，请参阅[AD FS 单一登录设置](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)  
  
如果刷新令牌已过期，请求会导致 HTTP 401 错误"invalid_grant"，"error_description"与"MSIS9615:Refresh_token 参数中收到的刷新令牌已过期"。 在这种情况下，ADAL 会自动将提交新的授权请求看起来就像上面的第 1。    
  
### <a name="web-browser-to-web-app"></a>Web 浏览器到 Web 应用   
在此方案中，使用浏览器用户需要访问托管的 web 应用程序资源。    
有两种方案来实现此目的。  
  
#### <a name="oauth-confidential-client"></a>Oauth 机密客户端  
此方案中是类似于上面中没有跟令牌交换的代码的授权请求。  Web 应用程序 （建模为 AD FS 中的服务器应用程序） 启动授权请求通过在浏览器，并交换令牌的代码 （通过直接连接到 AD FS）  
  
![协议流的说明](media/ADFS_DEV_4.png)  
  
1.  授权请求中通过浏览器中，可将 HTTP GET 发送到 AD FS Web 应用程序启动的授权终结点  
**授权请求**:  
获取 https://fs.contoso.com/adfs/oauth2/authorize?  
  
参数|值  
---------|---------  
response_type|"代码"  
资源|RP ID （标识符） 的应用程序组中的 Web API  
client_id|客户端应用程序组中的本机应用程序 Id  
redirect_uri|应用程序组中的 web 应用 （应用程序服务器） 的重定向 URI  
  
授权请求的响应：  
如果用户未登录之前，系统会提示用户输入凭据。  
AD FS 通过例如的 redirect_uri，查询组件中的"code"参数作为返回授权代码响应：HTTP/1.1 302 已找到位置： https://webapp.contoso.com/?code=&lt; 代码&gt;。  
  
2.  由于上面的 302，而在浏览器启动 HTTP GET 到 web 应用，例如：获取 http://redirect_uri:80/?code=&lt; 代码&gt;。   
  
3.  Web 应用，让收到代码，此时启动到的 AD FS 令牌终结点，发送以下请求  
**令牌请求：**  
发布 https://fs.contoso.com/adfs/oauth2/token  
  
参数|ReplTest1  
---------|---------  
grant_type|"authorization_code"  
code|从上面的第 2 授权代码  
资源|RP ID （标识符） 的应用程序组中的 Web API  
client_id|应用程序组中的 web 应用程序 （服务器应用程序） 的客户端 Id  
redirect_uri|应用程序组中的 web 应用 （应用程序服务器） 的重定向 URI  
client_secret|Web 应用 （应用程序服务器） 在应用程序组中的机密。 **注意：客户端的凭据不需要为 client_secret。AD FS 支持的功能也使用证书或 Windows 集成身份验证。**  
  
**令牌请求的响应：**  
AD FS 使用 access_token，refresh_token，并在正文中的 id_token 使用 HTTP 200 响应。  
声明  
4.  Web 应用程序，则可以使用 access_token 一部分 （在 web 应用本身在其中托管资源的情况），上述响应或以其他方式将其作为发送 HTTP 请求中的 Authorization 标头到 web API。  
  
#### <a name="single-sign-on-behavior"></a>在行为上的单一登录  
访问令牌将仍将有效但 1 小时 （默认） 在客户端的缓存中，您可能认为，第二个请求将正常工作的本机客户端方案更高版本的新的请求不会触发到 AD FS 的任何流量，也会自动将访问令牌将从缓存中提取的 ADAL。  但是，就可以使 web 应用程序可以发送不同的授权和令牌请求，前者通过不同的 URL 链接，如我们的示例中所示。  
  
在这种情况下，它是允许 AD FS 颁发新的授权代码，而不提示用户输入凭据的 AD FS 浏览器 SSO cookie。 然后，web 应用程序调用到 AD FS 以交换新的访问令牌的新授权代码。  不提示用户输入凭据。  
  
否则，如果 web 应用是智能足以知道是否用户已通过身份验证，可以跳过授权请求并将：  
* 缓存的访问令牌中，如果未过期，是检索到并使用，或   
* 请求的令牌基于的请求可以发送到 AD FS 令牌终结点，如下所述  
  
**刷新令牌请求：**  
发布 https://fs.contoso.com/adfs/oauth2/token
   
参数|ReplTest1  
---------|---------  
grant_type|"refresh_token"  
资源|RP ID （标识符） 的应用程序组中的 Web API  
client_id|应用程序组中的 web 应用程序 （服务器应用程序） 的客户端 Id  
refresh_token|刷新由对初始令牌请求的响应中的 AD FS 颁发的令牌  
client_secret|应用程序组中的 web 应用 （应用程序服务器） 机密  
  
**刷新令牌请求响应：**  
如果 < SSO_period > 内的刷新令牌，则请求将导致新的访问令牌。 不提示用户输入凭据。 有关详细信息 SSO 设置，请参阅[AD FS 单一登录设置](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)   
  
如果刷新令牌已过期，请求会导致 HTTP 401 错误"invalid_grant"，"error_description"与"MSIS9615:Refresh_token 参数中收到的刷新令牌已过期"。 在这种情况下，ADAL 会自动将提交新的授权请求看起来就像上面的第 1。    
  
#### <a name="openid-connect-hybrid-flow"></a>OpenID 连接：混合流  
此方案中有类似于上面的中，通过浏览器重定向，并从 web 应用到 AD FS 的令牌交换的代码的 web 应用程序启动授权请求。  在此方案中的区别是 AD FS 作为初始授权请求响应的一部分颁发 id_token。  
  
![协议流的说明](media/ADFS_DEV_5.png)  
  
1.  授权请求中通过浏览器中，可将 HTTP GET 发送到 AD FS Web 应用程序启动的授权终结点  
  
**授权请求：**  
获取 https://fs.contoso.com/adfs/oauth2/authorize?  
  
参数|值  
---------|---------  
response_type|"code+id_token"  
response_mode|"form_post"  
资源|RP ID （标识符） 的应用程序组中的 Web API  
client_id|应用程序组中的 web 应用程序 （服务器应用程序） 的客户端 Id  
redirect_uri|应用程序组中的 web 应用 （应用程序服务器） 的重定向 URI  
  
**授权请求的响应：**  
如果用户未登录之前，系统会提示用户输入凭据。  
AD FS 响应使用 HTTP 200 和窗体包含以下作为隐藏元素：  
* 代码： 授权代码  
* id_token: JWT 令牌中包含描述用户身份验证的声明  
2.  在窗体自动发布到 web 应用，将代码和 id_token 发送到 web 应用的 redirect_uri。  
  
3.  Web 应用，让收到代码，此时启动到的 AD FS 令牌终结点，发送以下请求  
  
**令牌请求：**  
发布 https://fs.contoso.com/adfs/oauth2/token
  
  
  
参数|ReplTest1  
---------|---------  
grant_type|"authorization_code"  
code|从上面的授权代码  
资源|RP ID （标识符） 的应用程序组中的 Web API  
client_id|应用程序组中的 web 应用程序 （服务器应用程序） 的客户端 Id  
redirect_uri|应用程序组中的 web 应用 （应用程序服务器） 的重定向 URI  
client_secret|应用程序组中的 web 应用 （应用程序服务器） 机密  
  
**令牌请求的响应：**  
AD FS 使用 access_token，refresh_token，并在正文中的 id_token 使用 HTTP 200 响应。  
  
4.  Web 应用程序，则可以使用 access_token 一部分 （在 web 应用本身在其中托管资源的情况），上述响应或以其他方式将其作为发送 HTTP 请求中的 Authorization 标头到 web API。  
  
#### <a name="single-sign-on-behavior"></a>在行为上的单一登录  
在行为上的单一登录与上面的 Oauth 2.0 机密客户端流相同。  
  
### <a name="on-behalf-of"></a>代表  
在此方案中，web 应用使用来自用户的原始访问令牌来请求和获取另一个 Web api，web 应用程序然后将最终用户访问另一个访问令牌。  这称为"代表的"流。  
  
![协议流的说明](media/ADFS_DEV_6.png)  
  
步骤 1 和 2 一样步骤 3 和 4 上一个流中的工作。  
在步骤 3 中的关键要求是 client_id 参数，2，该 Web 应用的客户端 ID 必须匹配 RP ID 的 Web API a。 换而言之，为新令牌交换访问令牌的受众必须匹配请求新令牌的实体的客户端 ID。  

## <a name="related-content"></a>相关内容  
请参阅[AD FS 开发](../AD-FS-Development.md)有关演练文章的完整列表，其中提供了分步说明有关使用相关的流。 
