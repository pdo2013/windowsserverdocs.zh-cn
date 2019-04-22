---
title: AD FS 故障排除-集成 Windows 身份验证
description: 本文档介绍如何解决集成的 windows 身份验证
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 91f252f5b0eca0f4c44e0b1a4564037298bf023c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814058"
---
# <a name="ad-fs-troubleshooting---integrated-windows-authentication"></a>AD FS 故障排除-集成 Windows 身份验证
集成的 Windows 身份验证使用户能够使用其 Windows 凭据和体验单一登录 (SSO)，使用 Kerberos 或 NTLM 进行登录。

## <a name="reason-integrated-windows-authentication-fails"></a>原因集成的 windows 身份验证失败
有三个主要原因为何集成的 windows 身份验证将失败。 它们分别是：
    - 服务主体名称 （spn） 配置不正确
    - 通道绑定令牌
    - Internet Explorer 配置

## <a name="spn-misonfiguration"></a>SPN misonfiguration
服务主体名称 (SPN) 是服务实例的唯一标识符。 Kerberos 身份验证使用 Spn 来将服务实例与服务登录帐户相关联。 这允许客户端应用程序请求的服务帐户进行身份验证即使客户端没有帐户名称。

如何的示例使用的 SPN 与 AD FS 是按如下所示：
1. Web 浏览器会查询 Active Directory，以确定哪个服务帐户运行 sts.contoso.com
2. Active Directory 通知浏览器，它是 AD FS 服务帐户。
3. 在浏览器将获取 AD FS 服务帐户的 Kerberos 票证。

如果 AD FS 服务帐户配置不正确或不正确的 SPN，这可能导致问题。  查看网络跟踪时，可能会看到如 KRB 错误的错误：KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN.

使用网络跟踪 （如 Wireshark) 可以确定哪些浏览器尝试解析的 SPN，然后使用命令行工具，setspn-Q <spn>，可以执行该 SPN 上进行的查找。  它可能找不到或可能为它分配到另一个帐户，而不是 AD FS 服务帐户。

![集成](media/ad-fs-tshoot-iwa/iwa3.png)

可以通过查看 AD FS 服务帐户的属性来验证 SPN。

![集成](media/ad-fs-tshoot-iwa/iwa1.png)

## <a name="channel-binding-token"></a>通道绑定令牌
目前，当客户端应用程序自行向身份验证使用 Kerberos、 Digest 或 NTLM 使用 HTTPS 的服务器，首先建立一个传输层安全 (TLS) 通道，进行身份验证，使用此通道。 

通道绑定令牌是 TLS 保护的外部通道，一个属性，用于通过客户端身份验证的内部通道将外部通道绑定到会话。

如果没有"--拦截"攻击发生，它们是 de 加密和重新加密的 SSL 流量，则该密钥将不匹配。  AD FS 将确定有其它东西在 web 浏览 r 与其自身之间中间。  这将导致 Kerberos 身份验证失败，并且将一个 401 对话框，而不是 SSO 体验提示用户。

这可能是通过的原因：
 - 坐在浏览器和 AD FS 之间的任何内容
 - Fiddler
 - 反向代理执行 SSL 桥接

默认情况下，AD FS 具有该选项设置为"允许"。  您可以更改此设置使用 PowerShell 脚本 `Set-ADFSProperties -ExtendProtectionTokenCheck`

为此，请参阅的详细信息[安全规划和部署的 AD FS 的最佳实践](../../ad-fs/design/best-practices-for-secure-planning-and-deployment-of-ad-fs.md)。

## <a name="internet-explorer-configuration"></a>Internet Explorer 配置
默认情况下，Internet 资源管理器将具有以下方法：

1. Internet 资源管理器将以单词协商标头中的 AD FS 中收到 401 响应。
2. 这将告知 web 浏览器以获取 Kerberos 或 NTLM 票证将发送回 AD FS。
3. 默认情况下 IE 将尝试执行此 (SPNEGO) 操作无需用户交互，如果单词为标头中的协商。  它仅将适用于 intranet 站点。

有 2 个可以禁止此行为从 happeing 的主要事项。
   - 启用集成的 Windows 身份验证未签入 IE 的属性。  这位于 Internet 选项-> 高级-> 安全。
![integrated](media/ad-fs-tshoot-iwa/iwa4.png)
   
   - 安全区域配置不正确
       - Fqdn 不在 intranet 区域
       - AD FS URL 不是在 intranet 区域中。

![集成](media/ad-fs-tshoot-iwa/iwa5.png)
## <a name="next-steps"></a>后续步骤

- [AD FS 进行故障排除](ad-fs-tshoot-overview.md)