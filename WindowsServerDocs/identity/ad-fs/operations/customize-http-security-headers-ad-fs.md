---
title: 自定义 HTTP 安全响应标头和 AD FS
description: 本文档 descirbes 如何自定义安全标头以防范安全漏洞。
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: akgoel23
ms.date: 02/19/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3c497cbafb8f9313f988a1b892d2b8fef68115eb
ms.sourcegitcommit: f6503e503d8f08ba8000db9c5eda890551d4db37
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68523906"
---
# <a name="customize-http-security-response-headers-with-ad-fs-2019"></a>自定义 AD FS 2019 的 HTTP 安全响应标头 
 
为了防范常见的安全漏洞, 并使管理员能够利用基于浏览器的保护机制中的最新改进功能, AD FS 2019 添加了自定义 HTTP 安全响应标头的功能AD FS 发送。 这是通过引入两个新的 cmdlet 来完成`Get-AdfsResponseHeaders`的`Set-AdfsResponseHeaders`: 和。  

>[!NOTE]
>使用 cmdlet 自定义 HTTP 安全响应标头的功能 (CORS 标头除外) `Get-AdfsResponseHeaders` : `Set-AdfsResponseHeaders`和向后移植 2016 AD FS。 通过安装[KB4493473](https://support.microsoft.com/en-us/help/4493473/windows-10-update-kb4493473)和[KB4507459](https://support.microsoft.com/en-us/help/4507459/windows-10-update-kb4507459), 可以将功能添加到 AD FS 2016。 

在本文档中, 我们将讨论常见的安全响应标头, 以演示如何自定义 AD FS 2019 发送的标头。   
 
>[!NOTE]
>本文档假定已安装 AD FS 2019。  

 
在讨论标题之前, 让我们看看一些需要管理员自定义安全标头的方案 
 
## <a name="scenarios"></a>方案 
1. 管理员已启用[**Http 严格传输-安全 (HSTS)** ](#http-strict-transport-security-hsts) (强制所有通过 HTTPS 加密的连接), 以防止可能会受到攻击的公共 wifi 访问点使用 HTTP 访问 web 应用的用户。 她想要通过为子域启用 HSTS 来进一步增强安全性。  
2. 管理员已配置[**X 帧选项**](#x-frame-options)响应标头 (防止在 iFrame 中呈现任何网页), 以防止网页被 clickjacked。 不过, 她需要自定义标头值, 因为新业务要求使用不同的源 (域) 从应用程序中显示数据 (在 iFrame 中)。
3. 如果浏览器检测到跨脚本攻击, 管理员启用了[**X-XSS 保护**](#x-xss-protection)(阻止跨脚本攻击) 来净化和阻止页面。 不过, 她需要自定义标头, 以允许页面在净化后加载。  
4. 管理员需要启用[**跨域资源共享 (CORS)** ](#cross-origin-resource-sharing-cors-headers) , 并在 AD FS 上设置原点 (域), 以允许单个页面应用程序使用另一个域来访问 web API。  
5. 管理员已启用[**内容安全策略 (CSP)** ](#content-security-policy-csp)标头, 通过禁止任何跨域请求来阻止跨站点脚本和数据注入攻击。 但是, 由于新的业务要求, 她需要自定义标头, 以允许网页从任意来源加载图像, 并将媒体限制为受信任的提供程序。  

 
## <a name="http-security-response-headers"></a>HTTP 安全响应标头 
响应标头包含在 AD FS 发送到 web 浏览器的传出 HTTP 响应中。 标头可以使用`Get-AdfsResponseHeaders` cmdlet 列出, 如下所示。  

![标头响应](media/customize-http-security-headers-ad-fs/header1.png)

上面`ResponseHeaders`的屏幕截图中的属性标识每个 HTTP 响应中 AD FS 将包含的安全标头。 仅当`ResponseHeadersEnabled`设置为`True` (默认值) 时, 才会发送响应标头。 该值可以设置为`False` , 以防止 AD FS 包含 HTTP 响应中的任何安全标头。 但是, 不建议这样做。  为此, 请使用以下内容:

```PowerShell
Set-AdfsResponseHeaders -EnableResponseHeaders $false
```
 
### <a name="http-strict-transport-security-hsts"></a>HTTP 严格传输-安全性 (HSTS) 
HSTS 是一种 web 安全策略机制, 有助于缓解同时具有 HTTP 和 HTTPS 终结点的服务的协议降级攻击和 cookie 劫持。 它允许 web 服务器声明 web 浏览器 (或其他符合用户代理) 只应使用 HTTPS 与其交互, 从不通过 HTTP 协议进行交互。  
 
Web 身份验证流量的所有 AD FS 终结点都是通过 HTTPS 以独占方式打开的。 因此, AD FS 会有效地缓解 HTTP 严格传输安全策略机制提供的威胁 (默认情况下, 由于 HTTP 中没有侦听器, 因此不会降级到 HTTP)。 可以通过设置以下参数自定义标头 
 
- **最大期限 =&lt;过期时间&gt;**  –过期时间 (以秒为单位) 指定仅应使用 HTTPS 访问站点的时间长度。 默认值和推荐值为31536000秒 (1 年)。  
- **includeSubDomains** –这是一个可选参数。 如果指定, 则 HSTS 规则也适用于所有子域。  
 
#### <a name="hsts-customization"></a>自定义 HSTS 
默认情况下, 标头已启用`max-age`并设置为1年; 但是, 管理员可以修改`max-age` (建议不要使用 "最大期限" 值) 或通过`Set-AdfsResponseHeaders` cmdlet 为子域启用 HSTS。  
 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=<seconds>; includeSubDomains" 
``` 

例如： 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=31536000; includeSubDomains" 
 ```

默认情况下, 该标头包含在`ResponseHeaders`属性中; 但是, 管理员可以`Set-AdfsResponseHeaders`通过 cmdlet 删除标头。  
 
```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "Strict-Transport-Security" 
```

### <a name="x-frame-options"></a>X 框架-选项 
默认情况下, AD FS 不允许外部应用程序在执行交互式登录时使用 Iframe。 这样做是为了防止某些网络钓鱼攻击样式。 请注意, 由于之前已建立会话级别的安全性, 因此可通过 iFrame 执行非交互式登录。  
 
但是, 在某些极少数情况下, 你可能会信任需要支持 iFrame 的特定应用程序 AD FS 登录页。 "X 框架-选项" 标头用于实现此目的。  
 
此 HTTP 安全响应标头用于与浏览器通信, 无论它是否可以在&lt;框架&gt;/&lt;iframe&gt;中呈现页。 标头可设置为以下值之一: 
 
- **拒绝**–帧中的页面将不会显示。 这是默认设置和建议的设置。  
- **sameorigin** –如果原点与网页的原点相同, 则页面将仅在框架中显示。 此选项不十分有用, 除非所有上级也在同一源中。  
- **允许-从<specified origin>**  -页仅在源 (例如,) https://www 中显示在框架中。com) 匹配标头中的特定来源。 

#### <a name="x-frame-options-customization"></a>X 框架-选项自定义  
默认情况下, 标头将设置为 deny;但是, 管理员可以通过`Set-AdfsResponseHeaders` cmdlet 修改此值。  
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "<deny/sameorigin/allow-from<specified origin>>" 
 ```

例如： 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "allow-from https://www.example.com" 
 ```

默认情况下, 该标头包含在`ResponseHeaders`属性中; 但是, 管理员可以`Set-AdfsResponseHeaders`通过 cmdlet 删除标头。  

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-Frame-Options" 
```

### <a name="x-xss-protection"></a>X-XSS-Protection 
当浏览器检测到跨站点脚本 (XSS) 攻击时, 此 HTTP 安全响应标头用于阻止网页加载。 这称为 XSS 筛选。 标头可设置为以下值之一 
 
- **0** –禁用 XSS 筛选。 不建议使用。  
- **1** –启用 XSS 筛选。 如果检测到 XSS 攻击, 浏览器将净化页面。   
- **1; mode = block** –启用 XSS 筛选。 如果检测到 XSS 攻击, 浏览器将阻止页面的呈现。 这是默认设置和建议的设置。  

#### <a name="x-xss-protection-customization"></a>X-XSS-保护自定义 
默认情况下, 标头将设置为 1;mode = block;但是, 管理员可以通过`Set-AdfsResponseHeaders` cmdlet 修改此值。  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "<0/1/1; mode=block/1; report=<reporting-uri>>" 
``` 

例如： 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "1" 
 ```

默认情况下, 该标头包含在`ResponseHeaders`属性中; 但是, 管理员可以`Set-AdfsResponseHeaders`通过 cmdlet 删除标头。 

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-XSS-Protection" 
```

### <a name="cross-origin-resource-sharing-cors-headers"></a>跨域资源共享 (CORS) 标头 
Web 浏览器安全性可防止网页发出跨源请求, 这些请求是从脚本内启动的。 但是, 有时您可能想要访问其他来源 (域) 中的资源。 CORS 是一种 W3C 标准, 可让服务器放宽相同的源策略。 使用 CORS，服务器可以在显式允许某些跨域请求时拒绝其他跨域请求。  
 
为了更好地理解 CORS 请求, 我们将演练一个方案, 其中单页面应用程序 (SPA) 需要调用具有不同域的 web API。 接下来, 让我们考虑到在 ADFS 2019 上配置了 SPA 和 API, 并 AD FS 启用了 CORS, 即 AD FS 可以标识 HTTP 请求中的 CORS 标头, 验证标头值, 并在响应中包含相应的 CORS 标头 (有关如何启用和的详细信息,在下面的 "CORS 自定义" 一节中的 AD FS 2019 上配置 CORS。 示例流: 

1. 用户通过客户端浏览器访问 SPA, 并被重定向到 AD FS 身份验证终结点进行身份验证。 由于 SPA 是为隐式授权流配置的, 因此在身份验证成功后, request 会向浏览器返回访问 + ID 令牌。  
2. 进行用户身份验证后, SPA 中包含的前端 JavaScript 将请求访问 web API。 请求重定向到具有以下标头的 AD FS
    - 选项–描述目标资源的通信选项 
    - 源–包括 web API 的源
    - "访问控制-请求-方法" –标识发出实际请求时要使用的 HTTP 方法 (例如删除)。 
    - 访问控制-请求标头-标识发出实际请求时要使用的 HTTP 标头 
    
   >[!NOTE]
   >CORS 请求类似于标准 HTTP 请求, 但源标头信号存在传入请求与 CORS 相关。 
3. AD FS 验证标头中包含的 web API 来源是否列在 AD FS 中配置的受信任来源中 (有关如何修改 CORS 自定义中的受信任来源的详细信息部分)。 然后 AD FS 会用以下标头进行响应。  
    - 访问控制-允许源–与源标头中的值相同 
    - 访问控制-允许-方法-值与在访问控制请求方法头中的值相同 
    - 访问控制-允许-标头-值与访问控制-请求标头中的值相同 
4. Browser 发送包含以下标头的实际请求 
    - HTTP 方法 (如删除) 
    - 源–包括 web API 的源 
    - 访问控制-允许标头响应标头中包含的所有标头 
5. 验证后, AD FS 通过在访问控制-允许的响应标头中包含 web API 域 (源) 来批准请求。  
6. 包含访问控制-允许的标头将允许浏览器继续调用请求的 API。

#### <a name="cors-customization"></a>CORS 自定义 
默认情况下, CORS 功能不会启用;但是, 管理员可以通过 AdfsResponseHeaders cmdlet 启用此功能。  

```PowerShell 
Set-AdfsResponseHeaders -EnableCORS $true 
 ```

一个已启用的管理员将能够使用同一个 cmdlet 枚举受信任来源的列表。 例如, 以下命令将允许来自起源**https&#58;//example1.com**和 **&#58;https//example1.com**的 CORS 请求。 
 
```PowerShell
Set-AdfsResponseHeaders -CORSTrustedOrigins https://example1.com,https://example2.com 
 ```

> [!NOTE]
> 管理员可以通过在受信任来源列表中包括 "*" 来允许来自任何来源的 CORS 请求, 但由于安全漏洞, 不建议使用此方法, 如果用户选择, 则会提供一条警告消息。 

### <a name="content-security-policy-csp"></a>内容安全策略 (CSP) 
此 HTTP 安全响应标头用于阻止浏览器无意中执行恶意内容, 阻止跨站点脚本、点击劫持和其他数据注入攻击。 不支持 CSP 的浏览器只是忽略 CSP 响应标头。  
 
#### <a name="csp-customization"></a>CSP 自定义 
自定义 CSP 标头时, 需要修改安全策略, 该策略可定义允许为网页加载的资源浏览器。 默认安全策略是  
 
`Content-Security-Policy: default-src ‘self’ ‘unsafe-inline’ ‘’unsafe-eval’; img-src ‘self’ data:;` 
 
**默认的-src**指令用于修改[-src 指令](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src), 而无需显式列出每个指令。 例如, 在下面的示例中, 策略1与策略2相同。  

策略1 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src 'self'" 
```
 
策略2
```PowerShell 
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "script-src ‘self’; img-src ‘self’; font-src 'self';  
frame-src 'self'; manifest-src 'self'; media-src 'self';" 
```

如果显式列出指令, 则指定的值将覆盖为默认值 src 提供的值。在下面的示例中, img-src 将值作为 "*" (允许从任何源加载图像), 而其他-src 指令将值作为 "self" (限制为与网页相同的源)。  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src ‘self’; img-src *" 
```
可以为默认的-src 策略定义以下源 
 
- "self" –指定此限制将内容源加载到网页的源 
- "unsafe-inline" –在策略中指定此项可使用内联 JavaScript 和 CSS 
- "unsafe-eval" –在策略中指定此项可将文本用于 JavaScript 机制, 如 eval 
- "无" –指定此限制要加载的任何源中的内容 
- 数据:-指定数据:Url 允许内容创建者将小型文件嵌入文档中。 不建议使用。  
 
>[!NOTE]
>AD FS 在身份验证过程中使用 JavaScript, 因此通过在默认策略中包含 "unsafe 内联" 和 "unsafe-eval" 源来启用 JavaScript。  

### <a name="custom-headers"></a>自定义标头 
除了上面列出的安全响应标头 (HSTS、CSP、X 帧选项、X-XSS 保护和 CORS) 以外, AD FS 2019 提供设置新标头的功能。  
 
例如：将新标头 "TestHeader" 设置为 "TestHeaderValue" 值 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "TestHeader" -SetHeaderValue "TestHeaderValue" 
 ```

设置后, 将在 AD FS 响应中发送新的标头 (下面的 fiddler 代码片段)。  
 
![Fiddler](media/customize-http-security-headers-ad-fs/header2.png)

## <a name="web-browswer-compatibility"></a>Web 浏览器兼容性
使用下表和链接来确定哪些 web 浏览器与每个安全响应标头兼容。

|HTTP 安全响应标头|浏览器兼容性|
|-----|-----|
|HTTP 严格传输-安全性 (HSTS)|[HSTS 浏览器兼容性](https://developer.mozilla.org/docs/Web/HTTP/Headers/Strict-Transport-Security#Browser_compatibility)|
|X 框架-选项|[X 框架-选项浏览器兼容性](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)| 
|X-XSS-Protection|[X-XSS-保护浏览器兼容性](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection#Browser_compatibility)| 
|跨域资源共享 (CORS)|[CORS 浏览器兼容性](https://developer.mozilla.org/docs/Web/HTTP/CORS#Browser_compatibility) 
|内容安全策略 (CSP)|[CSP 浏览器兼容性](https://developer.mozilla.org/docs/Web/HTTP/CSP#Browser_compatibility) 

## <a name="next"></a>Next

- [使用 AD FS 帮助 troublehshooting 指南](https://aka.ms/adfshelp/troubleshooting )
- [AD FS 疑难解答](../../ad-fs/troubleshooting/ad-fs-tshoot-overview.md)
