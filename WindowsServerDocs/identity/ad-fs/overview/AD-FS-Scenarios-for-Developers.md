---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: "面向开发人员广告 FS 方案"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 753b2b235cb1d73ab47588f8f229410c1f81db40
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-scenarios-for-developers"></a>面向开发人员广告 FS 方案

>适用于：Windows Server 2016

在 Windows Server 2016 [广告 FS 2016] 广告 FS 可使您添加的行业标准 OpenID 连接和 OAuth 2.0 基于身份验证和授权给您开发的，应用程序和了用户对广告 FS 直接进行身份验证这些应用。    
  
广告 FS 2016 还支持 WS 联盟，WS 信任和 SAML 协议和配置文件我们在以前的版本中有支持。  如果你感兴趣这些协议的开发人员指南，请参阅下面的文章。  本文将侧重于如何使用和从较新的协议支持中获益。  
  
## <a name="why-modern-authentication"></a>为什么现代身份验证  
时，你可以继续使用广告 FS 标记为在使用 WS 联盟，WS 信任和你一样的 SAML 协议有之前，请使用较新的协议，你会收到以下优势：  
  
* **简单起见和一致性**  
    * 使用相同的一组 Api 和模式，以支持登录：   
        *   多种类型的应用程序 （服务器桌面、 移动、 浏览器）  
        *   多个平台 (android，iOS Windows)  
        *   在公司的网络中的应用程序或在云中托管  
    * 使用你已可以使用 Azure AD 防御的用户身份验证的库的同一组  
* **灵活性**  
    * 除了标准的用户身份验证，如启用更复杂的方案：      
        * ? 与腿毛较 3 平板电脑的登录的用户授予一个 web 应用程序或服务访问驻留与其他 web 的应用或服务的资源的版本流。    
        * ? 服务器的版本流中的端服务访问的后端 API  
        * ? JavaScript 基于单页面应用程序 （水疗）  
* **Industry 的支持**  
    * OAuth 2.0 和 OpenID 连接尽情享受适用宽利用率于行业，以便在知识这些模式将帮助你使身份验证和之外的 Active Directory 环境授权  
  
## <a name="how-it-works-the-basics"></a>它的工作原理： 基础知识  
你可以向你使用的相同的一套工具和库你已可以使用 Azure AD 防御的用户身份验证的应用程序添加广告 FS 现代身份验证。   
  
在广告 FS 方案当然，很广告 FS 和作为身份提供商和授权的不 Azure AD 服务器。  否则概念是完全相同的： 用户提供其凭据和获取标记，直接或通过中间资源的访问权。  
  
最基本方案组成的用户或"资源所有者"、 浏览器访问 web 应用程序与交互：  
  
![广告 FS 面向开发人员](media/ADFS_DEV_1.png)  
  
Web 应用程序称为"客户"，因为它启动授权服务器 (广告 FS) 请求访问符号的资源。  资源托管本身 web 应用或可能会访问与该网络或 internet 上的某个地方网站 API。   用户或"资源所有者"授权的客户端 web 应用来接收该访问标记通过提供对授权服务器的凭据。    
  
## <a name="how-it-works-components"></a>它的工作原理： 组件  
OAuth 2.0 和 OpenID 连接方案广告 FS 规格中使用的工具和您使用的身份提供 Azure AD 时库的同一组。  这些组件是：  
* Active Directory 身份验证库 (ADAL): 客户端库便于收集用户凭据、 创建提交令牌请求和找回密码结果标记。    
* （打开 Web 界面.NET） OWIN 中间： 时 OWIN 社区基于项目，Microsoft 已创建一组的服务器端库保护 web 应用程序和 web OpenID 连接和 OAuth 2.0 Api  
  
这些组件的角色下图所示：  
  
![广告 FS 面向开发人员](media/ADFS_DEV_2.png)  
  
## <a name="modeling-these-scenarios-in-ad-fs-2016"></a>建模广告 FS 2016 在这些方案  
  
### <a name="application-groups"></a>应用程序组  
代表广告 FS 策略在这些方案中，我们引入了一个名为应用分组的新概念。  应用程序组可能包含的任何数字和组合的应用程序的基本以下类型：  
  
  
  
应用程序组 / 应用程序类型  |描述  |角色    
---------|---------|---------  
本机应用程序     |  有时称为公共客户端，这被用于运行的电脑或设备上，并与用户进行交互的客户端应用。       | 请求标记从用户访问授权服务器 (广告 FS) 的资源。  发送到受保护的资源并使用这些标记为 HTTP 标题 HTTP 请求。        
服务器应用程序     |   在服务器上运行，并通过浏览器的用户通常可以访问 web 应用程序。  因为它是可维护其自己的客户端密码或凭据，有时称为机密的客户端。      | 请求标记从用户访问授权服务器 (广告 FS) 的资源。  发送到受保护的资源并使用这些标记为 HTTP 标题 HTTP 请求。               
Web API     |  结束资源用户访问。 这些"信赖方"的新表达的想法。| 消耗标记获得的客户端  
  
### <a name="differences-from-ad-fs-2012-r2"></a>从广告 FS 2012 R2 的差异  
应用程序组组合广告 FS 2012 R2 公开分别为信赖方，客户端应用程序的权限的信任和授权元素。  
  
下表比较的相应应用程序信任对象中创建广告 FS 2012 R2 与广告 FS 2016 方法：  
  
在 Windows Server 2012 R2 的广告 FS|在 PowerShell 中|广告 FS 管理  
---------|---------|---------  
添加本机客户端|添加 AdfsClient|纳帕  
作为客户端添加服务器应用程序|添加 AdfsClient|纳帕  
添加 Web API / 资源|添加 AdfsRelyingPartyTrust|创建信赖聚会信任  
  
广告 FS 2016|在 PowerShell 中|广告 FS 管理  
---------|---------|---------  
添加本机客户端|添加 AdfsNativeClientApplication|添加本机的应用组  
作为客户端添加服务器应用程序|添加 AdfsServerApplication|添加服务器的应用组  
添加 Web API / 资源|添加 AdfsWebApiApplication|添加 Web API 的应用组  
  
### <a name="application-permissions-and-consent"></a>应用权限和同意  
默认情况下，应用程序组中的客户端允许访问的同一组中的资源。  管理员没有配置特定应用程序的权限。  应用程序组还允许管理员在指定允许，如 openid 或 user_impersonation 范围。  下面的说明方案指定完全哪些范围是必需的方案。  
  
由于广告 FS 使用管理员同意的情况下的模型，用户将时没有提示同意的情况下访问资源。  通过配置应用程序组，管理员实际上提供代表所有应用程序用户的同意。  
  
## <a name="supported-scenarios"></a>受支持的方案  
以下部分介绍我们在更多详细信息中支持的方案。  
  
### <a name="tokens-used"></a>使用标记  
这些方案中使使用标记的三种类型：  
  
* **id_token:** JWT 令牌用于表示的用户的身份。 Id_token aud 或受众索赔匹配本机或服务器的应用程序的客户端 ID。  
* **access_token:** JWT 令牌用于在 Oauth 和 OpenID 连接方案和想要可消耗的资源。  此令牌 aud 或受众索赔必须匹配资源或 Web API 的标识符。  
* **refresh_token:**此令牌提交来替代收集用户凭据，以提供体验单一登录。  此令牌是于颁发和使用广告 FS 并不是通过客户端或资源较高的可读性。    
  
### <a name="native-client-to-web-api"></a>Web api 本机客户端  
这种情况下，本机客户端应用程序称呼广告 FS 2016 保护 Web API 的用户。  
* 本机客户端应用程序使用 ADAL 将发送授权和令牌到广告 FS，提示输入凭据从必要时，用户请求后将其发送作为 HTTP 标头在 Web API 请求的结果标记  
* [仅用于演示这就是]Web API 读取访问令牌客户端，发送的结果，并且将它们发送给客户的 ClaimsPrincipal 对象的索赔。  
  
![协议流量的说明](media/ADFS_DEV_3.png)  
  
1.  本机客户端应用程序启动通过调用 ADAL 库流量。  这会触发基于浏览器 HTTP 访问广告 FS 授权端点：  
  
**授权请求：**  
获取 https://fs.contoso.com/adfs/oauth2/authorize?  
  
参数|值  
---------|---------  
response_type|"代码"  
资源|在应用程序组中的 Web API RP ID （标识符）  
client_id|客户端应用程序组中的原始应用程序 Id  
redirect_uri|将重定向 URI 本机应用组中的应用程序  
  
**授权请求的响应：**  
如果用户之前已未登录时，用户将提示您输入凭据。    
广告 FS 响应通过返回作为"代码"参数 redirect_uri 查询组件中的验证码。  例如： HTTP 月 1.1 302 找到位置： **http://redirect_uri:80 /？ 代码 =&lt;代码&gt;。**  
  
2.  然后，本机客户端到广告 FS 令牌端点发送的代码，以及以下参数:  
  
**令牌请求：**  
https://fs.contoso.com/adfs/oauth2/token 博客文章  
  
参数|值  
---------|---------  
grant_type|"授权" 
代码|从 1 的验证码  
资源|在应用程序组中的 Web API RP ID （标识符）  
client_id|客户端应用程序组中的原始应用程序 Id  
redirect_uri|将重定向 URI 本机应用组中的应用程序  
  
**令牌请求的响应：**  
广告 FS 与 access_token、 refresh_token 和正文中的 id_token HTTP 200 进行响应。  
  
3.  然后本机应用发送的上述响应 access_token 部分 HTTP 请求中的授权标题为到 web API。  
  
### <a name="single-sign-on-behavior"></a>单一登录行为  
（默认） 的 1 小时 access_token 仍然有效在缓存，并且新请求不会触发广告 FS 到任何通信，后续客户在请求。  通过 ADAL 从缓存将自动被获取 access_token。  
  
访问标记过期后，ADAL 将自动刷新令牌基于的请求发送到广告 FS 令牌端点 （自动跳授权请求）。  
**刷新令牌请求：**  
https://fs.contoso.com/adfs/oauth2/token 博客文章
   

参数|值|
---------|---------
grant_type|"refresh_token"|
资源|在应用程序组中的 Web API RP ID （标识符）|
client_id|客户端应用程序组中的原始应用程序 Id
refresh_token|颁发的初始令牌请求的响应广告 FS 刷新标记

  
  
**刷新令牌请求的响应：**  
如果内 < SSO_period > 刷新标记，则此请求将导致一个新的访问权限标记。 用户未提示您输入凭据。  有关详细信息 SSO 设置查看[广告 FS 单一登录上设置](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)  
  
如果刷新令牌已过期，此请求将导致错误"invalid_grant"和"error_description"HTTP 401"MSIS9615： 已过期 refresh_token 参数中收到刷新令牌"。 在此情况下，ADAL 自动提交新的授权请求，如下所示 #1 上方。    
  
### <a name="web-browser-to-web-app"></a>Web 浏览器到 Web 的应用   
在此情况下，浏览器与用户需要访问的托管通过 web 应用程序的资源。    
有两种情况，实现这一点。  
  
#### <a name="oauth-confidential-client"></a>Oauth 机密客户端  
此项 scenario 等同于上述中没有授权请求后, 跟令牌更换费用的输入代码。  （建模作为服务器广告 FS 中的应用程序） 的 web 应用启动通过浏览器授权请求和交换标记代码 （通过直接连接到广告 FS）  
  
![协议流量的说明](media/ADFS_DEV_4.png)  
  
1.  通过浏览器，将发送到广告 FS HTTP 获取请求授权 Web 应用启动授权端点  
**授权请求**:  
获取 https://fs.contoso.com/adfs/oauth2/authorize?  
  
参数|值  
---------|---------  
response_type|"代码"  
资源|在应用程序组中的 Web API RP ID （标识符）  
client_id|客户端本机应用组中的应用 Id  
redirect_uri|将重定向 URI 的应用程序组中的 web 应用 （在服务器应用程序）  
  
授权请求的响应：  
如果用户之前已未登录时，用户将提示您输入凭据。  
广告 FS 响应通过例如返回作为"代码"参数 redirect_uri 的查询组件中的验证码： HTTP 月 1.1 302 找到位置： https://webapp.contoso.com/?code=&lt;代码&gt;。  
  
2.  由于上述 302，浏览器启动 HTTP 访问 web 应用中，例如： 获取 http://redirect_uri:80 /？ 代码 =&lt;代码&gt;。   
  
3.  Web 应用，有收到该代码，此时开始发送以下广告 FS 令牌端点请求  
**令牌请求：**  
https://fs.contoso.com/adfs/oauth2/token 博客文章  
  
参数|值  
---------|---------  
grant_type|"授权"  
代码|从上面 2 的验证码  
资源|在应用程序组中的 Web API RP ID （标识符）  
client_id|客户端应用程序组中的 web 应用 （在服务器应用程序） Id  
redirect_uri|将重定向 URI 的应用程序组中的 web 应用 （在服务器应用程序）  
client_secret|机密的应用程序组中的 web 应用 （在服务器应用程序）。 **注意： 客户端的凭据不必不会 client_secret。  广告 FS 支持的功能，以及使用证书或 Windows 的集成身份验证。**  
  
**令牌请求的响应：**  
广告 FS 与 access_token、 refresh_token 和正文中的 id_token HTTP 200 进行响应。  
索赔  
4.  Web 应用程序可能会消耗 access_token 部分上述响应 （在这种情况的 web 应用本身承载资源），然后或以其他方式将其发送的 HTTP 请求的授权标题为到 web API。  
  
#### <a name="single-sign-on-behavior"></a>单一登录行为  
尽管访问令牌仍将有效的 1 小时 （默认） 客户端的缓存中，您可能认为，第二个请求能否如下所示本机客户端方案上述-新请求不会触发广告 FS 到任何通信访问令牌将自动获取从缓存 ADAL 通过按。  但是，则可能 web 应用可以发送明显授权和令牌请求，、 前者通过不同的 URL 链接，如下所示我们示例。  
  
在此情况下，它是使广告 FS 处理新的验证码，而不会提示用户输入凭据的广告 FS 浏览器 SSO cookie。 然后将 web 应用调用到广告 FS 交换获取新的访问权限令牌新的验证码。  用户未提示您输入凭据。  
  
否则，如果 web 应用智能足够了解如果用户已进行身份验证，可以跳过授权请求和任一：  
* 缓存的访问标记，如果过期，是检索和使用，或者   
* 如下所述请求令牌基于的请求可以发送给广告 FS 令牌端点，  
  
**刷新令牌请求：**  
https://fs.contoso.com/adfs/oauth2/token 博客文章
   
参数|值  
---------|---------  
grant_type|"refresh_token"  
资源|在应用程序组中的 Web API RP ID （标识符）  
client_id|客户端应用程序组中的 web 应用 （在服务器应用程序） Id  
refresh_token|刷新颁发的初始令牌请求的响应广告 FS 标记  
client_secret|成功的应用程序组中的 web 应用 （在服务器应用程序）  
  
**刷新令牌请求的响应：**  
如果内 < SSO_period > 刷新标记，则此请求将导致一个新的访问权限标记。 用户未提示您输入凭据。 有关详细信息 SSO 设置查看[广告 FS 单一登录上设置](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)   
  
如果刷新令牌已过期，此请求将导致错误"invalid_grant"和"error_description"HTTP 401"MSIS9615： 已过期 refresh_token 参数中收到刷新令牌"。 在此情况下，ADAL 自动提交新的授权请求，如下所示 #1 上方。    
  
#### <a name="openid-connect-hybrid-flow"></a>OpenID 连接： 混合流量  
此项 scenario 有类似于上述中的 web 浏览器重定向，并从 web 应用与广告 FS 进行令牌交换的代码通过应用启动授权请求。  在此情况下之处在于广告 FS，作为请求最初授权响应的问题 id_token。  
  
![协议流量的说明](media/ADFS_DEV_5.png)  
  
1.  通过浏览器，将发送到广告 FS HTTP 获取请求授权 Web 应用启动授权端点  
  
**授权请求：**  
获取 https://fs.contoso.com/adfs/oauth2/authorize?  
  
参数|值  
---------|---------  
response_type|"代码 + id_token"  
response_mode|"form_post"  
资源|在应用程序组中的 Web API RP ID （标识符）  
client_id|客户端应用程序组中的 web 应用 （在服务器应用程序） Id  
redirect_uri|将重定向 URI 的应用程序组中的 web 应用 （在服务器应用程序）  
  
**授权请求的响应：**  
如果用户之前已未登录时，用户将提示您输入凭据。  
广告 FS 响应 HTTP 200 和表单包含以下作为隐藏元素：  
* 代码： 验证码  
* id_token： 包含描述用户身份验证的索赔 JWT 令牌  
2.  窗体自动到的 web 应用，向 web 应用发送的代码和 id_token redirect_uri 文章。  
  
3.  Web 应用，有收到该代码，此时开始发送以下广告 FS 令牌端点请求  
  
**令牌请求：**  
https://fs.contoso.com/adfs/oauth2/token 博客文章
  
  
  
参数|值  
---------|---------  
grant_type|"授权"  
代码|从上面的验证码  
资源|在应用程序组中的 Web API RP ID （标识符）  
client_id|客户端应用程序组中的 web 应用 （在服务器应用程序） Id  
redirect_uri|将重定向 URI 的应用程序组中的 web 应用 （在服务器应用程序）  
client_secret|成功的应用程序组中的 web 应用 （在服务器应用程序）  
  
**令牌请求的响应：**  
广告 FS 与 access_token、 refresh_token 和正文中的 id_token HTTP 200 进行响应。  
  
4.  Web 应用程序可能会消耗 access_token 部分上述响应 （在这种情况的 web 应用本身承载资源），然后或以其他方式将其发送的 HTTP 请求的授权标题为到 web API。  
  
#### <a name="single-sign-on-behavior"></a>单一登录行为  
行为单一登录并至于 Oauth 2.0 机密的客户端流上述相同。  
  
### <a name="on-behalf-of"></a>代表  
在此情况下，web 应用使用原始访问令牌用户请求和其他 Web API，然后为最终用户访问 web 应用，将获得另一个访问标记。  这称为"在代表的"流量。  
  
![协议流量的说明](media/ADFS_DEV_6.png)  
  
就像步骤 3 和 4 以前流中的步骤 1 和 2 工作。  
第 3 步中的关键要求是 client_id 参数的客户端 ID Web 应用 2，必须匹配 RP ID 的 Web API a。换言之，新令牌未兑换的访问标记受众必须匹配实体要求的新标记的客户端 ID。  

## <a name="related-content"></a>相关的内容  
请参阅[广告 FS 开发](../AD-FS-Development.md)有关浏览文章完整列表，其中提供的分步说明使用相关的版本流。 
