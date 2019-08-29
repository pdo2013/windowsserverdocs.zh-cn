---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: 为用户证书身份验证配置 AD FS 支持
description: ''
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 058433f98d986c0daa720dd19f283135763cfe30
ms.sourcegitcommit: c307886e96622e9595700c94128103b84f5722ce
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70108757"
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>为用户证书身份验证配置 AD FS

用户证书身份验证主要在两个用例中使用
* 用户使用智能卡针对其 AD FS 系统进行登录
* 用户使用的是预配到移动设备的证书


## <a name="prerequisites"></a>先决条件
1) 使用[本文](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)中所述的一种模式确定要启用 AD FS 用户证书身份验证的模式
2) 确保你的用户证书信任链安装 & 受所有 AD FS 和 WAP 服务器信任, 包括任何中间证书颁发机构。 通常, 这是通过 AD FS/WAP 服务器上的 GPO 完成的
3)  确保用户证书的信任链的根证书位于 NTAuth 存储区中 Active Directory
4) 如果在备用证书身份验证模式中使用 AD FS, 请确保 AD FS 和 WAP 服务器的 SSL 证书包含前缀为 "certauth" 的 AD FS 主机名 (例如 "certauth.fs.contoso.com"), 并允许到此主机名的流量通过防火墙
5) 如果使用 extranet 的证书身份验证, 请确保从 internet 访问证书中指定的列表中至少有一个 AIA 和至少一个 CDP 或 OCSP 位置。
6) 此外, 对于 Exchange ActiveSync 客户端, Azure AD 对于 Exchange ActiveSync 客户端, 客户端证书必须在 "使用者可选名称" 字段的主体名称或 RFC822 名称值中具有用户可路由电子邮件地址。 (Azure Active Directory 将 RFC822 值映射到目录中的 "代理地址" 属性。)


## <a name="configure-ad-fs-for-user-certificate-authentication"></a>配置 AD FS 执行用户证书身份验证  

使用 AD FS 管理控制台或 PowerShell cmdlet `Set-AdfsGlobalAuthenticationPolicy`, 在 AD FS 中使用 intranet 或 extranet 身份验证方法启用用户证书身份验证。

如果要为 Azure AD 证书身份验证配置 AD FS, 请确保已配置了证书颁发者和序列号所需的[Azure AD 设置](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities)和[AD FS 声明规则](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements)

此外, 还有一些可选的方面。
- 如果要使用基于证书字段和扩展的声明以及 EKU (声明类型 https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku) ), 请在 Active Directory 声明提供方信任上配置其他声明传递规则。  有关可用证书声明的完整列表, 请参阅下文。  
- 如果需要基于证书类型限制访问, 则可以在应用程序 AD FS 颁发授权规则中使用证书的其他属性。 常见的情况是 "仅允许 MDM 提供程序预配的证书" 或 "仅允许智能卡证书"
- 在[本文](https://technet.microsoft.com/library/dn786429(v=ws.11).aspx)中, 使用 "客户端身份验证的受信任颁发者的管理" 下的指导配置允许的客户端证书证书颁发机构。
- 你可能想要在执行证书身份验证时, 根据最终用户的需求来修改登录页。 常见的情况是将 "使用 X509 证书登录" 更改为最终用户友好的内容

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>在 Windows 桌面上为 Chrome 浏览器配置无缝证书身份验证
如果计算机上存在多个用户证书 (如 Wi-fi 证书), 则满足客户端身份验证的目的时, Windows 桌面上的 Chrome 浏览器将提示用户选择正确的证书。 这可能会给最终用户造成混淆。 若要优化此体验, 你可以将 "Chrome" 策略设置为自动选择正确的证书, 以获得更好的用户体验。 此策略可以通过更改注册表或通过 GPO 自动配置 (设置注册表项) 进行手动设置。 这需要你的用户客户端证书进行身份验证, 以使不同的颁发者不同于其他用例的颁发者 AD FS。 

有关为 Chrome 配置此配置的详细信息, 请参阅此[链接](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls)。  


## <a name="troubleshoot-certificate-authentication"></a>证书身份验证疑难解答
本文档重点介绍在为用户配置证书身份验证 AD FS 时遇到的常见问题。 

### <a name="check-if-certificate-trusted-issuers-is-configured-properly-in-all-the-ad-fswap-servers"></a>检查是否在所有 AD FS/WAP 服务器中正确配置了证书信任的颁发者
*常见症状:HTTP 204 "没有内容 https://certuath.adfs.contoso.com "*

AD FS 使用基础 windows 操作系统来证明用户证书的所有权, 并通过执行证书信任链验证来确保它与受信任的颁发者匹配。 若要匹配受信任的颁发者, 你将需要确保所有根和中间颁发机构都配置为本地计算机证书颁发机构存储中的受信任颁发者。 若要自动验证此情况, 请使用[AD FS 诊断分析器工具](https://adfshelp.microsoft.com/DiagnosticsAnalyzer/Analyze)。 该工具将查询所有服务器, 并确保正确配置正确的证书。 
1)  按照上面链接中提供的说明下载并运行该工具
2)  上传结果并检查是否存在任何故障

### <a name="check-if-certificate-authentication-is-enabled-in-the-ad-fs-authentication-policy"></a>检查是否已在 AD FS Authentication 策略中启用证书身份验证
默认情况下, AD FS 不启用证书身份验证。 有关如何启用证书身份验证的说明, 请参阅本文档的开头。 

### <a name="check-if-certificate-authentication-is-enabled-in-the-ad-fs-authentication-policy"></a>检查是否已在 AD FS Authentication 策略中启用证书身份验证
默认情况下, AD FS 在端口49443上使用与 AD FS 相同的主机名进行用户证书身份`adfs.contoso.com`验证 (例如)。 你还可以将 AD FS 配置为使用备用 SSL 绑定的端口 443 (默认 HTTPS 端口)。 但是, 此配置中使用的 URL 为`certauth.<adfs-farm-name>` ( `certauth.contoso.com`例如)。 有关详细信息, 请参阅[此链接](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)。 网络连接最常见的情况是防火墙配置不正确, 阻止或干扰用户证书身份验证流量。 通常情况下, 出现此问题时, 你将看到一个空白屏幕或500服务器错误。 
1)  请注意你在中配置的主机名和端口 AD FS
2)  确保将 AD FS 或 Web 应用程序代理 (WAP) 前面的任何防火墙配置为允许 AD FS 场`hostname:port`的组合。 需要参考网络工程师才能执行此步骤。 

### <a name="check-certificate-revocation-list-connectivity"></a>检查证书吊销列表连接
证书吊销列表 (CRL) 是编码到用户证书中以执行运行时吊销检查的终结点。 例如, 如果包含证书的设备被盗, 管理员可以将该证书添加到 "吊销的证书" 列表中。 之前接受此证书的任何终结点将无法进行身份验证。

每个 AD FS 和 WAP 服务器都需要到达 CRL 终结点, 以验证提供给它的证书是否仍然有效且未被吊销。 可以通过 HTTPS、HTTP、LDAP 或通过 OCSP (联机证书状态协议) 进行 CRL 验证。 如果 AD FS/WAP 服务器无法连接到终结点, 则身份验证将失败。 请按照以下步骤进行故障排除。 
1) 咨询 PKI 工程师, 确定用于从 PKI 系统吊销用户证书的 CRL 终结点。 
2)  在每个 AD FS/WAP 服务器上, 确保可通过使用的协议 (通常是 HTTPS 或 HTTP) 访问 CRL 终结点
3)  对于高级验证, 请在每个 AD FS/WAP 服务器上[启用 CAPI2 事件日志记录](https://blogs.msdn.microsoft.com/benjaminperkins/2013/09/30/enable-capi2-event-logging-to-troubleshoot-pki-and-ssl-certificate-issues/)
4) 检查 CAPI2 操作日志中的事件 ID 41 (验证吊销)
5) 检查`‘\<Result value="80092013"\>The revocation function was unable to check revocation because the revocation server was offline.\</Result\>’`

***提示***：可以通过将 DNS 解析 (Windows 上的主机文件) 配置为指向特定服务器来更轻松地针对单个 AD FS 或 WAP 服务器进行故障排除。 这允许你启用针对服务器的跟踪。 

### <a name="check-if-this-is-a-server-name-indication-sni-issue"></a>检查这是否是服务器名称指示 (SNI) 问题
AD FS 要求客户端设备 (或浏览器) 和负载均衡器支持 SNI。 某些客户端设备 (通常是较早版本的 Android) 可能不支持 SNI。 此外, 负载均衡器可能不支持 SNI 或尚未配置 SNI。 在这些情况中, 你可能会看到用户认证失败。 
1)  与网络工程师合作, 确保 AD FS/WAP 的负载均衡器支持 SNI
2)  如果无法支持 SNI, AD FS 会按照以下步骤进行操作:
    *   在主 AD FS 服务器上打开提升的命令提示符窗口
    *   键入```Netsh http show sslcert```
    *   复制联合身份验证服务的 "应用程序 GUID" 和 "证书哈希"
    *   键入`netsh http add sslcert ipport=0.0.0.0:{your_certauth_port} certhash={your_certhash} appid={your_applicaitonGUID}`

### <a name="check-if-the-client-device-has-been-provisioned-with-the-certificate-correctly"></a>检查是否已通过证书正确设置了客户端设备
你可能会注意到某些设备工作正常, 但其他设备却不能正常工作。 在这种情况下, 这通常是由于未在客户端设备上正确设置用户证书而导致的。 请按照以下步骤操作。 
1)  如果此问题特定于 Android 设备, 最常见的问题是 Android 设备上不完全信任证书链。  请参阅 MDM 供应商, 以确保已正确设置证书, 并且整个证书链在 Android 设备上完全受信任。 
2)  如果此问题特定于 Windows 设备, 请通过检查已登录用户 (而非系统/计算机) 的 Windows 证书存储来检查是否正确设置了证书。
3)  将客户端用户证书导出到 .cer 文件, 并运行命令 "certutil-urlfetch-verify certificatefilename"


### <a name="check-if-the-tls-version-is-compatible-between-ad-fswap-servers-and-the-client-device"></a>检查 TLS 版本是否与 AD FS/WAP 服务器和客户端设备兼容
在极少数情况下, 客户端设备 (通常为移动设备) 会更新为仅支持更高的 TLS 版本 (例如 1.3), 或者您可能会遇到相反的问题, 即 AD FS/WAP 服务器更新为仅使用较高的 TLS 版本且客户端设备不支持它。 可以使用联机 SSL 工具检查 AD FS/WAP 服务器, 并查看其是否与设备兼容。 有关如何控制 TLS 版本的详细信息, 请参阅[此链接](manage-ssl-protocols-in-ad-fs.md)。

### <a name="check-if-azure-ad-promptloginbehavior-is-configured-correctly-on-your-federated-domain-settings"></a>检查是否在联合域设置上正确配置 Azure AD PromptLoginBehavior
许多 Office 365 应用程序发送 prompt = 登录到 Azure AD。 默认情况下, Azure AD 会将其转换为新的密码登录名以 AD FS。 因此, 即使已在 AD FS 中配置证书身份验证, 最终用户也只会看到密码 "登录"。 
1)  使用 "Set-msoldomainfederationsettings" 命令获取联合域设置
2)  确保将 PromptLoginBehavior 参数设置为 "Disabled" 或 "NativeSupport" 中的一个

有关详细信息, 请参阅[此链接](ad-fs-prompt-login.md)。 

### <a name="additional-troubleshooting"></a>其他疑难解答
这种情况极少出现
1)  如果 CRL 列表很长, 则尝试下载时可能会超时。 在这种情况下, 需要根据每个 https://support.microsoft.com/en-us/help/820129/http-sys-registry-settings-for-windows




## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>参考：用户证书声明类型和示例值的完整列表

|                                         声明类型                                         |                              示例值                               |
|--------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version         |                                    3                                     |
|     https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm      |                                sha256RSA                                 |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer            |                 CN = entca, DC = domain, DC = contoso, DC = com                  |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername          |                 CN = entca, DC = domain, DC = contoso, DC = com                  |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore          |                           12/05/2016 20:50:18                            |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter           |                           12/05/2017 20:50:18                            |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/subject           |   E =user@contoso.com, cn = user, cn = Users, dc = domain, DC = contoso, dc = com   |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname         |   E =user@contoso.com, cn = user, cn = Users, dc = domain, DC = contoso, dc = com   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata           |                {Base64 编码的数字证书数据}                 |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             DigitalSignature                             |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             KeyEncipherment                              |
|  https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier   |                 9D11941EC06FACCCCB1B116B56AA97F3987D620A                 |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier  |    KeyID = d6 13 e3 6b bc e5 d8 15 52 0a fd 36 6a d5 0b 51 f3 0b 25 7f     |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename |                                   “用户”                                   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/san           | 其他名称: Principal name =user@contoso.com, RFC822 name =user@contoso.com |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku           |                          1.3.6.1.4.1.311.10.3.4                          |

## <a name="related-links"></a>相关链接
* [为 AD FS 证书身份验证配置备用主机名绑定](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [在 Azure AD 中配置证书颁发机构](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities)
