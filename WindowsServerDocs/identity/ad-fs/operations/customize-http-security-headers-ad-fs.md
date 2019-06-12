---
title: 自定义 AD FS 的 HTTP 安全响应标头
description: 此文档 descirbes 如何自定义安全标头，以避免出现安全漏洞。
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: akgoel23
ms.date: 02/19/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 231c8783032f51f607565922d90ea7f7eb877cfd
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444697"
---
# <a name="customize-http-security-response-headers-with-ad-fs-2019"></a>自定义 AD FS 2019 HTTP 安全响应标头 
 
若要避免出现常见安全漏洞并为管理员提供能够充分利用最新改进的基于浏览器的保护机制中，AD FS 2019 添加自定义 HTTP 安全响应标头的功能由 AD FS 发送。 这通过引入了两个新 cmdlet 来实现：`Get-AdfsResponseHeaders`和`Set-AdfsResponseHeaders`。  
 
通常，我们将讨论本文档中使用安全响应标头来演示如何自定义 ad FS 2019 发送的标头。   
 
>[!NOTE]
>文档假设已安装 AD FS 2019。  

 
我们将讨论标头之前，让我们看几个方案创建了管理员可以自定义安全标头 
 
## <a name="scenarios"></a>方案 
1. 管理员已启用[ **HTTP 严格传输安全性 (HSTS)** ](#http-strict-transport-security-hsts) （通过 HTTPS 加密将强制所有连接） 来保护可能会访问使用 HTTP 从公共 wifi 访问的 web 应用的用户可能会受到黑客攻击的点。 她想要通过为子域启用 HSTS 来进一步加强安全性。  
2. 管理员已配置[ **X 框架选项**](#x-frame-options)响应标头 （会阻止呈现在 iFrame 中的任何 web 页面） 以防止网页正在 clickjacked。 但是，她需要自定义新的业务要求，以显示数据 （在 iFrame) 的标头值从具有不同的源 （域） 的应用程序。
3. 管理员已启用[ **X XSS 保护**](#x-xss-protection) （防止跨脚本攻击） 清理，并阻止对该页的如果浏览器检测到跨脚本攻击。 但是，她需要自定义标头，使页面的加载一次净化。  
4. 管理员需要启用[**跨域资源共享 (CORS)** ](#cross-origin-resource-sharing-cors-headers)和 AD FS 以允许在单个页面应用程序访问 web API 与另一个域上设置原点 （域）。  
5. 管理员已启用[**内容安全策略 (CSP)** ](#content-security-policy-csp)标头以防止跨站点脚本和数据注入攻击： 不允许任何跨域请求。 但是，由于新的业务要求她需要自定义标头，使 web 页后，可以从任何源加载图像并限制对受信任的提供程序的媒体。  

 
## <a name="http-security-response-headers"></a>HTTP 安全响应标头 
响应标头包含在 AD fs 发送到 web 浏览器的传出 HTTP 响应。 可以使用列出的标头`Get-AdfsResponseHeaders`cmdlet，如下所示。  

![标头响应](media/customize-http-security-headers-ad-fs/header1.png)

`ResponseHeaders`在上面的屏幕截图中的属性标识将包括在每个 HTTP 响应中的 AD fs 的安全标头。 仅当将发送的响应标头`ResponseHeadersEnabled`设置为`True`（默认值）。 值可以设置为`False`以防止任何安全标头包括在 HTTP 响应中的 AD FS。 但是不将此建议。  为此使用：

```PowerShell
Set-AdfsResponseHeaders -EnableResponseHeaders $false
```
 
### <a name="http-strict-transport-security-hsts"></a>HTTP 严格的传输的安全性 (HSTS) 
HSTS 是 web 安全策略机制，这有助于缓解协议降级攻击和具有 HTTP 和 HTTPS 终结点的服务的 cookie 劫持。 它允许 web 服务器来声明，web 浏览器 （或其他符合要求的用户代理） 应仅交互使用它使用 HTTPS 并永远不会通过 HTTP 协议。  
 
以独占方式通过 HTTPS，则打开所有 AD FS 终结点的 web 身份验证流量。 因此，AD FS 有效地减轻 HTTP 严格传输安全性策略机制提供的威胁 （默认情况下不存在任何降级到 HTTP 由于在 HTTP 中没有任何侦听器）。 可以通过设置以下参数自定义标头 
 
- **最大期限 =&lt;过期时间&gt;** – 的到期时间 （以秒为单位） 指定多长时间应该只能访问站点使用 HTTPS。 默认与建议的值为 31536000 秒 （1 年）。  
- **includeSubDomains** – 这是一个可选参数。 如果指定，HSTS 规则适用于以及所有子域。  
 
#### <a name="hsts-customization"></a>HSTS 自定义项 
默认情况下，启用该标头和`max-age`设置为 1 年; 但是，管理员可以修改`max-age`（降低最大期限值不推荐） 或为子域通过启用 HSTS `Set-AdfsResponseHeaders` cmdlet。  
 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=<seconds>; includeSubDomains" 
``` 

例如： 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=31536000; includeSubDomains" 
 ```

默认情况下，该标头包含在`ResponseHeaders`属性; 但是，管理员可以删除通过标头`Set-AdfsResponseHeaders`cmdlet。  
 
```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "Strict-Transport-Security" 
```

### <a name="x-frame-options"></a>X-Frame-Options 
默认情况下 AD FS 不允许外部应用程序执行交互式登录名时使用 iFrames。 这样做是为了防止网络钓鱼攻击的某些样式。 请注意，可以通过 iFrame 由于已建立的以前会话级别安全执行非交互式登录名。  
 
但是，在某些极少数情况下可能会信任需要 iFrame 支持交互式 AD FS 登录页的特定应用程序。 X 帧选项标头用于此目的。  
 
此 HTTP 安全响应标头用于进行通信到浏览器是否能够呈现中的页&lt;帧&gt;/&lt;iframe&gt;。 可以将标头设置为以下值之一： 
 
- **拒绝**– 将不会显示在帧中的页。 这是默认值，建议的设置。  
- **sameorigin** – 页面将仅显示在帧来源是否与网页的源相同。 选项不是很有用的除非所有上级也都具有相同的原点。  
- **允许从<specified origin>**  -页面将仅显示在帧如果原点 (例如， https://www。"。com) 匹配的标头中特定的源。 

#### <a name="x-frame-options-customization"></a>X 框架选项自定义项  
默认情况下，将设置标头来拒绝;但是，管理员可以修改通过值`Set-AdfsResponseHeaders`cmdlet。  
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "<deny/sameorigin/allow-from<specified origin>>" 
 ```

例如： 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "allow-from https://www.example.com" 
 ```

默认情况下，该标头包含在`ResponseHeaders`属性; 但是，管理员可以删除通过标头`Set-AdfsResponseHeaders`cmdlet。  

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-Frame-Options" 
```

### <a name="x-xss-protection"></a>X-XSS-Protection 
此 HTTP 安全响应标头用于停止从浏览器检测到跨站点脚本 (XSS) 攻击时加载的网页。 这称为 XSS 筛选。 可以将标头设置为以下值之一 
 
- **0** – 禁用 XSS 筛选。 不建议这样做。  
- **1** – 使 XSS 筛选。 如果检测到 XSS 攻击，浏览器将净化页面。   
- **1;模式 = 块**– 使 XSS 筛选。 如果检测到 XSS 攻击，浏览器将阻止页面的呈现。 这是默认值，建议的设置。  

#### <a name="x-xss-protection-customization"></a>X XSS 保护自定义项 
默认情况下，标头将设置为 1;模式 = 块;但是，管理员可以修改通过值`Set-AdfsResponseHeaders`cmdlet。  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "<0/1/1; mode=block/1; report=<reporting-uri>>" 
``` 

例如： 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "1" 
 ```

默认情况下，该标头包含在`ResponseHeaders`属性; 但是，管理员可以删除通过标头`Set-AdfsResponseHeaders`cmdlet。 

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-XSS-Protection" 
```

### <a name="cross-origin-resource-sharing-cors-headers"></a>跨域资源共享 (CORS) 标头 
Web 浏览器安全性将阻止网页上建立从启动脚本中的跨域请求。 但是，有时你可能想要访问其他来源 （域） 中的资源。 CORS 是一项 W3C 标准，可让服务器放宽同域策略。 使用 CORS，服务器可以在显式允许某些跨域请求时拒绝其他跨域请求。  
 
若要更好地了解 CORS 请求，让我们演练的方案，其中的单个页面应用程序 (SPA) 需要调用 web API 与不同的域。 此外，让我们考虑 SPA 和 API 配置 ADFS 2019 和 AD FS 已启用 CORS 即 AD FS 可以识别在 HTTP 请求中的 CORS 标头、 验证标头值，并在响应中包括适当的 CORS 标头 (如何启用详细信息和CORS 上配置 AD FS 2019 CORS 自定义部分下面）。 示例流： 

1. 用户通过客户端浏览器访问 SPA，并将重定向到 AD FS 身份验证终结点进行身份验证。 SPA 配置为隐式授权流，因为请求身份验证成功后返回访问 + ID 标记向浏览器。  
2. 用户身份验证后，包含在 SPA 前端 JavaScript 发出请求以访问 web API。 请求重定向到 AD FS 与以下标头
    - 选项 – 描述目标资源的通信选项 
    - 源-包括 web API 的来源
    - 访问控制的请求-方法-标识的 HTTP 方法 （例如，删除） 发出实际请求时要使用 
    - 访问的控件的请求的标头-标识发出实际请求时要使用的 HTTP 标头 
    
   >[!NOTE]
   >CORS 请求类似于标准的 HTTP 请求，但是，如果存在 origin 标头发出信号传入请求是 CORS 相关。 
3. AD FS 验证 web API origin 标头中包含所示配置 AD FS （如何修改下面的 CORS 自定义部分中的受信任的来源的详细信息） 中的受信任来源。 AD FS 然后使用以下标头进行响应。  
    - 访问控制的允许的源-如下所示 Origin 标头值相同 
    - 访问控制的允许的方法-如下所示的访问控制请求方法标头值相同 
    - 如下所示的访问控制的请求标头标头的访问-控制的允许的标头的值相同 
4. 浏览器发送实际请求包括以下标头 
    - HTTP 方法 （例如，删除） 
    - 源-包括 web API 的来源 
    - 访问-控制的允许的标头响应标头中包含的所有标头 
5. 验证后，AD FS 访问控制的允许的源响应标头中包含的 web API 域 （源） 的批准请求。  
6. 访问控制的允许的域标头包含将允许浏览器以继续进行调用请求的 API。

#### <a name="cors-customization"></a>CORS 自定义项 
默认情况下，不会启用 CORS 功能;但是，管理员可以启用通过集 AdfsResponseHeaders cmdlet 的功能。  

```PowerShell 
Set-AdfsResponseHeaders -EnableCORS $true 
 ```

一个启用了，管理员可以枚举的受信任来源使用相同的 cmdlet 的列表。 例如，以下命令将允许从这些来源的 CORS 请求**https&#58;//example1.com**并**https&#58;//example1.com**。 
 
```PowerShell
Set-AdfsResponseHeaders -CORSTrustedOrigins https://example1.com,https://example2.com 
 ```

> [!NOTE]
> 管理员可以允许任何来源的 CORS 请求通过包括"*"在列表中的受信任的来源，不过，由于安全漏洞和一条警告消息，不建议此方法提供了如果他们选择。 

### <a name="content-security-policy-csp"></a>内容安全策略 (CSP) 
此 HTTP 安全响应标头用于通过防止无意中执行恶意内容的浏览器来防止跨站点脚本、 点击劫持的侵害和其他数据注入式攻击。 不支持 CSP 的浏览器只需将忽略 CSP 响应标头。  
 
#### <a name="csp-customization"></a>CSP 自定义项 
CSP 标头的自定义涉及修改定义的资源浏览器的安全策略允许加载 web 页。 默认安全策略  
 
`Content-Security-Policy: default-src ‘self’ ‘unsafe-inline’ ‘’unsafe-eval’; img-src ‘self’ data:;` 
 
**默认 src**指令用于修改[-src 指令](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src)而无需显式列出每个指令。 例如，在以下策略示例 1 与相同策略 2。  

策略 1 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src 'self'" 
```
 
策略 2
```PowerShell 
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "script-src ‘self’; img-src ‘self’; font-src 'self';  
frame-src 'self'; manifest-src 'self'; media-src 'self';" 
```

如果显式列出一个指令，指定的值将覆盖指定的默认 src 值。在以下示例中，i m g src 将采用的值为 * （允许从任何源加载的映像） 而其他的 src 指令将采用的值为自助 （限制为同一来源 web 页面）。  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src ‘self’; img-src *" 
```
以下源可以为默认 src 策略定义 
 
- 'self'-指定此限制要加载到网页的源的内容的来源 
- 不安全的内联 – 在策略中指定这允许使用的内联 JavaScript 和 CSS 
- 不安全 eval – 在策略中指定这允许使用文本到等机制，JavaScript 的 eval 
- none-指定此将内容限制要加载任何来源 
- 数据:-指定数据：Uri 允许内容创建者可以在文档中嵌入小型文件内联。 不建议这样做的使用情况。  
 
>[!NOTE]
>AD FS 身份验证过程中使用 JavaScript，因此使 JavaScript 通过包括不安全内联和不安全 eval 源在默认策略。  

### <a name="custom-headers"></a>自定义标头 
除了以上列出安全响应标头 (HSTS，CSP，X 框架选项、 X XSS 保护和 CORS)，AD FS 2019 提供设置新的标头的功能。  
 
例如：若要将新的标头"TestHeader"值设置为"TestHeaderValue" 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "TestHeader" -SetHeaderValue "TestHeaderValue" 
 ```

完成设置后，将 AD FS 响应 （下面 fiddler 代码段） 中发送新的标头。  
 
![Fiddler](media/customize-http-security-headers-ad-fs/header2.png)

## <a name="web-browswer-compatibility"></a>Web 浏览器兼容性问题
使用下面的表和链接来确定哪些 web 浏览器兼容与每个安全响应标头。

|HTTP 安全响应标头|浏览器兼容性|
|-----|-----|
|HTTP 严格的传输的安全性 (HSTS)|[HSTS 浏览器兼容性](https://developer.mozilla.org/docs/Web/HTTP/Headers/Strict-Transport-Security#Browser_compatibility)|
|X-Frame-Options|[X 框架选项浏览器兼容性](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)| 
|X-XSS-Protection|[X XSS 保护浏览器兼容性](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection#Browser_compatibility)| 
|跨域资源共享 (CORS)|[CORS 浏览器兼容性](https://developer.mozilla.org/docs/Web/HTTP/CORS#Browser_compatibility) 
|内容安全策略 (CSP)|[CSP 浏览器兼容性](https://developer.mozilla.org/docs/Web/HTTP/CSP#Browser_compatibility) 

## <a name="next"></a>Next

- [使用 AD FS 帮助 troublehshooting 指南](https://aka.ms/adfshelp/troubleshooting )
- [AD FS 疑难解答](../../ad-fs/troubleshooting/ad-fs-tshoot-overview.md)
