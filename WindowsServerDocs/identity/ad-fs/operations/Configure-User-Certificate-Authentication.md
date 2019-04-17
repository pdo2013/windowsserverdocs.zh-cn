---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: "配置广告 FS 用户证书身份验证的支持"
description: 
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.service: active-directory
ms.technology: identity-adfs
ms.openlocfilehash: 9941d4dd997e857874aceddc920ec7f9d8944a81
ms.sourcegitcommit: d351cdbb0bf2533d6db35626ebbc4924b3834309
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2018
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>配置广告 FS 用户证书身份验证

>适用于：Windows Server 2016，Windows Server 2012 R2

可以 x509 中所述的使用模式之一用户证书身份验证配置广告 FS[本文](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)。 可以使用此功能[使用 Azure Active Directory](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/)或自行，以使客户端和设备预配到访问广告 FS 用户证书与从 intranet 或外部的资源。

## <a name="prerequisites"></a>先决条件
- 确保你的用户证书所有广告金融服务和 WAP 服务器通过受信任的
- 确保信任针对你的用户证书链的根证书处于 Active Directory 的 NTAuth 官方商城
- 如果使用型备用证书身份验证的广告 FS，确保你的广告金融服务和 WAP 服务器具有 SSL 证书包含"certauth"，例如"certauth.fs.contoso.com"，以前缀广告 FS 主机和到此主机该交通允许通过防火墙
- 如果使用从外部网站的证书身份验证，确保在至少有一个 AIA 和你证书中指定的列表中至少一个 CDP 或 OCSP 位置是可通过 internet 访问。
- 如果你在配置为 Azure AD 证书身份验证的广告 FS，确保你已经将配置[Azure AD 设置](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities)和[广告 FS 声称需规则](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements)证书颁发和序列号
- 此外为 Azure AD 证书身份验证，对于 Exchange ActiveSync 客户端的客户端证书必须具有用户可路由电子邮件地址 Exchange 联机主要名称或的值 RFC822 名称中备用名称主题字段。 （azure Active Directory 地图 RFC822 值的目录中的代理服务器地址属性到。）

## <a name="configure-ad-fs-for-user-certificate-authentication"></a>配置广告 FS 用户证书身份验证  
- 启用 intranet 或中广告 FS，使用广告 FS 管理控制台或 PowerShell cmdlet 组 AdfsGlobalAuthenticationPolicy 外部身份验证方法为用户证书身份验证
- 请确保每广告金融服务和 WAP 服务器上安装了整个信任，包括任何中间的证书链。 在本地计算机中间证书颁发机构官方商城上所有广告金融服务和 WAP 服务器应安装中间证书。
- 如果你想要使用基于证书字段和除了 EKU（索赔键入 https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku）扩展的索赔上 Active Directory 索赔提供商信任, 配置其他索赔直通规则。  有关可用的证书索赔的完整列表，请见下文。  
- [可选]配置中使用"受信任的客户端身份验证的发行商管理"下的指南客户端证书颁发的允许证书颁发机构[本文](https://technet.microsoft.com/en-us/library/dn786429(v=ws.11).aspx)。

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>在 Windows 桌面上 Chrome 浏览器配置无缝证书身份验证
（如 Wi-fi 证书）的多个用户证书存在满足客户端身份验证目标计算机上，当 Chrome 浏览器的 Windows 台式计算机上将会提示用户选择正确的证书。 这可能是最终用户对感到很困惑。 优化此体验，你可以设置 Chrome 若要自动选择正确的证书，为用户提供更好的用户体验的策略。 此策略可以手动设置通过对注册表进行更改，或配置自动通过 GPO（若要设置的注册表项）。 此身份验证广告 FS 能够通过其他用例明显颁发需要你的用户客户证书。 

有关此配置为 Chrome 的详细信息，请参阅此[链接](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls)。  


## <a name="troubleshooting"></a>故障排除
- 如果证书身份验证请求失败并 HTTP 204"不从 https://certauth.fs.contoso.com 内容"响应，验证根和任何中间 CA 证书已安装，分别、 到 CA 和中间加利福尼亚信任根证书所有联合身份验证的服务器上存储。
- 如果证书身份验证请求失败未知原因，导出到一个.cer 文件的客户端证书和运行该命令 

`certutil -f -urlfetch -verify certificatefilename.cer`

确保所有 CRL 和 CRL 位置解决增量。  请注意，增量 CRL 位置发现基于基本 CRL 的内容。

## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>类型和示例值的完整列表的用户证书声称参考：

|声称类型|示例值
|-----|-----
|https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version | 3
|https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm | sha256RSA
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer | CN = entca，直流 = 域，DC = contoso，直流 = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername | CN = entca，直流 = 域，DC = contoso，直流 = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore | 12/05/2016 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter | 12/05/2017 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subject | E =user@contoso.com，CN = 用户，CN = 用户，直流 = 域，DC = contoso，直流 = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname | E =user@contoso.com，CN = 用户，CN = 用户，直流 = 域，DC = contoso，直流 = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata | {Base64 编码数字证书数据}
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | DigitalSignature
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | KeyEncipherment
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier | 9D11941EC06FACCCCB1B116B56AA97F3987D620A
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier | 密钥 id 为 = d6 13 e3 6b 公元前 e5 d8 15 52 0a fd 36 6a d5 0b 51 f3 0b 25 7f
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename | 用户
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/san | 其他名称： 主体名称 =user@contoso.com，RFC822 名称 =user@contoso.com
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku | 1.3.6.1.4.1.311.10.3.4


