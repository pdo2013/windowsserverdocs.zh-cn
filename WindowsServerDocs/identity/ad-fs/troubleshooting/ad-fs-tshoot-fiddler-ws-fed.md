---
title: 故障排除-Fiddler-WS-联合身份验证的 AD FS
description: 本文演示了使用 AD FS 的 WS 联合身份验证交换的详细跟踪
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: be1c9f466ec13272d10f0fb9ca31cf326a1ec29a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846898"
---
# <a name="ad-fs-troubleshooting---fiddler---ws-federation"></a>故障排除-Fiddler-WS-联合身份验证的 AD FS
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler9.png)

## <a name="step-1-and-2"></a>步骤 1 和 2
这是我们跟踪的开始。  在此范围内将看到以下信息： ![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler1.png)

请求：

- HTTP GET 为我们信赖方 （ http://sql1.contoso.com/SampApp)

响应：

- 响应是 HTTP 302 （重定向）。  响应标头中的传输数据显示为 （重定向的位置 https://sts.contoso.com/adfs/ls)
- 重定向 URL 包含 wa = wsignin 1.0 我们信赖方应用程序已为我们生成 WS 联合身份验证登录请求和发送到 AD FS /adfs/ls/终结点这告诉我们。  此即为绑定重定向。
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler2.png)

## <a name="step-3-and-4"></a>步骤 3 和 4

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler3.png)

请求：

- HTTP GET 到我们的 AD FS server(sts.contoso.com)

响应：

- 响应是提示输入凭据。  这表示，我们将使用窗体的身份验证机制
- 通过单击响应的 web 视图上可以看到提示的凭据。
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler6.png)

## <a name="step-5-and-6"></a>步骤 5 和 6

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler4.png)

请求：

- 使用我们的用户名和密码的 HTTP POST。  
- 我们将介绍我们的凭据。  通过查看在请求中的原始数据，我们可以查看这些凭据

响应：

- 响应是找到和 MSIAuth 创建并返回已加密的 cookie。  这用于验证由我们的客户端生成的 SAML 断言。  这是也称为"身份验证 cookie"，才会显示 AD FS 时 Idp。


## <a name="step-7-and-8"></a>步骤 7 和 8
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler5.png)

请求：

- 现在，我们已通过身份验证我们在 AD FS 服务器的另一个 HTTP GET 并提供我们身份验证令牌

响应：

- 响应是 HTTP 确定，这意味着 AD FS 的内容具有基于提供的凭据对用户进行身份验证
- 此外，我们设置了 3 条 cookie 返回给客户端
    - MSISAuthenticated 包含客户端进行身份验证时的 base64 编码的时间戳值。
    - MSISLoopDetectionCookie 可供具有无限重定向循环到联合身份验证服务器中的最后的停止客户端的 AD FS 无限循环检测机制。 Cookie 数据是时间戳，采用 base64 编码。
    - MSISSignout 用于跟踪的 IdP，所有 RPs 都访问了 SSO 会话。 调用 WS-联合注销时，利用此 cookie。 可以看到使用 base64 解码器此 cookie 的内容。
    
## <a name="step-9-and-10"></a>步骤 9 和 10
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler7.png) 请求：

- HTTP POST

响应：

- 响应是找到

## <a name="step-11-and-12"></a>步骤 11 和 12
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler8.png) 请求：

- HTTP GET

响应：

- 响应是确定

## <a name="next-steps"></a>后续步骤

- [AD FS 进行故障排除](ad-fs-tshoot-overview.md)