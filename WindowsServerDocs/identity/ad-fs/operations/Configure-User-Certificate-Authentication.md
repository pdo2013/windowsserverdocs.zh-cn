---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: 配置 AD FS 支持用户证书身份验证
description: ''
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d5c2d84c263517a4c81622ca02538796ccd9da71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817498"
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>为用户证书身份验证配置 AD FS

>适用于：Windows Server 2016, Windows Server 2012 R2

可以为 x509 用户证书身份验证使用一种模式中所述配置 AD FS[这篇文章](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)。 可以使用此功能[与 Azure Active Directory](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/)或本身要使客户端和设备预配用户证书访问 AD FS 与从 intranet 或 extranet 资源。

## <a name="prerequisites"></a>先决条件
- 确保用户证书受信任的所有 AD FS 和 WAP 服务器
- 请确保对用户证书的信任链的根证书处于 NTAuth 存储在 Active Directory 中
- 如果使用备用证书身份验证模式中的 AD FS，确保你的 AD FS 和 WAP 服务器具有包含"certauth"，例如"certauth.fs.contoso.com"，带有前缀的 AD FS 主机名的 SSL 证书并允许该流量与此主机名通过防火墙
- 如果使用证书身份验证从 extranet，请确保至少一个 AIA 和至少一个 CDP 或 OCSP 的位置，从你的证书中指定的列表是从 internet 访问。
- 如果要配置 Azure AD 证书身份验证的 AD FS，请确保你已配置[Azure AD 设置](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities)并[AD FS 声明规则所需](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements)证书颁发者和序列号
- 此外为 Azure AD 证书身份验证，对于 Exchange ActiveSync 客户端，客户端证书必须用户可路由电子邮件地址在 Exchange online 中的主体名称或 RFC822 名称值的使用者备用名称字段。 （azure Active Directory RFC822 会将值映射到目录中的代理地址属性。）

## <a name="configure-ad-fs-for-user-certificate-authentication"></a>配置 AD FS 执行用户证书身份验证  
- 启用用户证书身份验证为 intranet 或 extranet 身份验证方法，在 AD FS 中，使用 AD FS 管理控制台或 PowerShell cmdlet 集 AdfsGlobalAuthenticationPolicy
- 请确保每个 AD FS 和 WAP 服务器上安装了信任，包括任何中间证书的整个链。 中间证书应安装在所有 AD FS 和 WAP 服务器上的本地计算机中间证书颁发机构存储中。
- 如果你想要使用为基础的证书字段和除了 EKU 扩展声明 (声明类型 https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku)，在 Active Directory 声明提供方信任配置规则的其他声明传递。  有关可用的证书声明的完整列表，请参阅下文。  
- [可选]在中配置为使用"管理的客户端身份验证的受信任颁发者"下的指南的客户端证书允许颁发的证书颁发机构[这篇文章](https://technet.microsoft.com/library/dn786429(v=ws.11).aspx)。

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>为 Windows 台式计算机上的 Chrome 浏览器配置无缝证书身份验证
满足的客户端身份验证目的计算机上存在多个用户证书 （如 Wi-fi 证书） 时，在 Windows 桌面上的 Chrome 浏览器将提示用户选择正确的证书。 这可能会给最终用户造成混淆。 若要优化此体验，可以设置适用于 Chrome 来自动选择正确的证书，以更好的用户体验的策略。 可以手动进行的注册表更改设置或通过 GPO （若要设置的注册表项） 自动配置此策略。 这需要针对 AD FS 能够从其他用例的不同颁发者进行身份验证的用户客户端证书。 

有关此配置对于 Chrome 的详细信息，请参阅此[链接](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls)。  


## <a name="troubleshooting"></a>疑难解答
- 如果证书身份验证请求因 HTTP 204"从无内容 https://certauth.fs.contoso.com"响应，验证，根和任何中间 CA 证书是否已安装，分别为受信任的根 CA 证书和中间 CA 证书对所有存储联合身份验证服务器。
- 如果证书身份验证请求失败，原因未知，将客户端证书导出到.cer 文件，并运行命令 

`certutil -f -urlfetch -verify certificatefilename.cer`

请确保任何 CRL 和增量 CRL 位置解析。  请注意，基于内容的基本 CRL 找到增量 CRL 的位置。

## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>参考：用户证书的完整列表声明类型和示例值

|声明类型|示例值
|-----|-----
|https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version | 3
|https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm | sha256RSA
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer | CN=entca, DC=domain, DC=contoso, DC=com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername | CN=entca, DC=domain, DC=contoso, DC=com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore | 12/05/2016 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter | 12/05/2017 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subject | E=user@contoso.com, CN=user, CN=Users, DC=domain, DC=contoso, DC=com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname | E=user@contoso.com, CN=user, CN=Users, DC=domain, DC=contoso, DC=com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata | {Base64 编码的数字证书数据}
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | DigitalSignature
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | KeyEncipherment
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier | 9D11941EC06FACCCCB1B116B56AA97F3987D620A
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier | KeyID = d6 13 e3 6b bc e5 d8 15 52 0a fd 36 6a d5 0b 51 f3 0b 25 7f
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename | “用户”
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/san | 其他主体名称： 名称 =user@contoso.com，RFC822 名称 =user@contoso.com
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku | 1.3.6.1.4.1.311.10.3.4


