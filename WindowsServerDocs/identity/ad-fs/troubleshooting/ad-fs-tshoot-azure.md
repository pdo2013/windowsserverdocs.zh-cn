---
title: AD FS 故障排除-Azure AD
description: 本文档介绍如何对 AD FS 和 Azure AD 的各个方面进行故障排除
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 228ef34ab25276c1cf98f9b2b64e997390023c87
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444008"
---
# <a name="ad-fs-troubleshooting---azure-ad"></a>AD FS 故障排除-Azure AD
与云的发展，许多公司都转而使用为其各种应用和服务使用 Azure AD。  与 Azure AD 联合已成为许多组织的标准实践。  本文档将介绍的一些故障排除此联合身份验证发生的问题的方面。  多个常规故障排除文档中的主题仍适用于本文档将重点介绍与 Azure AD 的只是具体信息以便与 Azure 联合和 AD FS 交互。

## <a name="redirection-to-ad-fs"></a>重定向到 AD FS
重定向时发生在登录到 Office 365 之类的应用程序并将"重定向"组织的 AD FS 服务器登录。

![](media/ad-fs-tshoot-azure/azure1.png)


### <a name="first-things-to-check"></a>第一个要检查的事项
如果没有进行重定向有几个你想要检查

   1. 请确保你的 Azure AD 租户的联合身份验证启用通过登录到 Azure 门户并检查在 Azure AD Connect。

![](media/ad-fs-tshoot-azure/azure2.png)

1. 请确保单击旁边联合身份验证的域在 Azure 门户中验证自定义域。
   ![](media/ad-fs-tshoot-azure/azure3.png)

2. 最后，您想要检查[DNS](ad-fs-tshoot-dns.md) ，并确保你的 AD FS 服务器或 WAP 服务器从 internet 解析。  验证此方法解决并且您将能够导航到它。
3. 你还可以使用 PowerShell cmdlt`Get-AzureADDomain`还获取此信息。

![](media/ad-fs-tshoot-azure/azure6.png)

### <a name="you-are-receiving-an-unknown-auth-method-error"></a>收到未知身份验证方法错误
您可能会遇到"未知身份验证方法"错误消息指出，AuthnContext 时，不支持在 AD FS 或 STS 级别将从 Azure 被重定向。 

在 Azure AD 使用重定向到 AD FS 或 STS 强制执行身份验证方法的参数时，这是最常见。 

若要强制实施身份验证方法，请使用以下方法之一：
- 对于 WS 联合身份验证使用的 WAUTH 查询字符串来强制使用首选的身份验证方法。

- 为 SAML2.0，使用以下方法：
  ```
  <saml:AuthnContext>
  <saml:AuthnContextClassRef>
  urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
  </saml:AuthnContextClassRef>
  </saml:AuthnContext>
  ```
  使用错误的值，发送的强制执行身份验证方法时或在 AD FS 或 STS 上不支持该身份验证方法，您收到一条错误消息之前进行身份验证。

|想要的身份验证方法|wauth URI|
|-----|-----|
|用户名和密码身份验证|urn:oasis:names:tc:SAML:1.0:am:password|
|SSL 客户端身份验证|urn:ietf:rfc:2246|
|Windows 集成的身份验证|urn:federation:authentication:windows|

受支持的 SAML 身份验证上下文类

|身份验证方法|身份验证上下文类 URI|
|-----|-----| 
|用户名和密码|urn:oasis:names:tc:SAML:2.0:ac:classes:Password|
|受密码保护传输|urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport|
|传输层安全性 (TLS) 客户端|urn:oasis:names:tc:SAML:2.0:ac:classes:TLSClient
|X.509 证书|urn:oasis:names:tc:SAML:2.0:ac:classes:X509
|集成的 Windows 身份验证|urn:federation:authentication:windows|
|Kerberos|urn:oasis:names:tc:SAML:2.0:ac:classes:Kerberos|

为了确保 AD FS 级别支持的身份验证方法，请检查以下各项。

#### <a name="ad-fs-20"></a>AD FS 2.0 

下 **/adfs/ls/web.config**，请确保身份验证类型的项已存在。

```
<microsoft.identityServer.web>
<localAuthenticationTypes>
<add name="Forms" page="FormsSignIn.aspx" />
<add name="Integrated" page="auth/integrated/" />
<add name="TlsClient" page="auth/sslclient/" />
<add name="Basic" page="auth/basic/" />
</localAuthenticationTypes>
```

#### <a name="ad-fs-2012-r2"></a>AD FS 2012 R2

下**AD FS 管理**，单击**身份验证策略**在 AD FS 管理单元中。

在中**主要身份验证**部分中，单击编辑旁边的全局设置。 您可以右键单击身份验证策略，然后选择编辑全局主要身份验证。 或者，在操作窗格中，选择编辑全局主要身份验证。

在编辑全局身份验证策略窗口中，在主选项卡上，你可以配置设置作为全局身份验证策略的一部分。 例如，对主要身份验证，可以选择在 Extranet 和 Intranet 的可用身份验证方法。

* * 请确保选择了所需的身份验证方法复选框。 

#### <a name="ad-fs-2016"></a>AD FS 2016

下**AD FS 管理**，单击**服务**并**身份验证方法**在 AD FS 管理单元中。

在中**主要身份验证**部分中，单击编辑。

在中**编辑身份验证方法**窗口中，在主选项卡上，可以设置配置为身份验证策略的一部分。

![](media/ad-fs-tshoot-azure/azure4.png)

## <a name="tokens-issued-by-ad-fs"></a>由 AD FS 颁发的令牌

### <a name="azure-ad-throws-error-after-token-issuance"></a>Azure AD 令牌颁发后将引发错误
AD FS 颁发的令牌后，Azure AD 可能会引发错误。 在此情况下，检查以下问题：
- AD fs 令牌中颁发的声明应在 Azure AD 中匹配的用户的相应属性。
- Azure AD 的令牌应包含以下必需的声明：
    - WSFED: 
        - UPN:此声明的值应与用户的 UPN 匹配在 Azure AD 中。
        - ImmutableID:在 Azure AD 中，此声明的值应与匹配的 sourceAnchor 或用户的 ImmutableID。

若要在 Azure AD 中获取用户属性值，请运行以下命令行： `Get-AzureADUser –UserPrincipalName <UPN>`

![](media/ad-fs-tshoot-azure/azure5.png)

   - SAML 2.0:
       - IDPEmail:此声明的值应与在 Azure AD 中匹配用户的用户主体的名称。
       - NAMEID:在 Azure AD 中，此声明的值应与匹配的 sourceAnchor 或用户的 ImmutableID。

有关详细信息，请参阅[使用 SAML 2.0 标识提供者来实现单一登录](https://technet.microsoft.com/library/dn641269.aspx)。

### <a name="token-signing-certificate-mismatch-between-ad-fs-and-azure-ad"></a>令牌签名证书不匹配 AD FS 与 Azure AD 之间。

AD FS 使用的令牌签名证书以签署发送到用户或应用程序的令牌。 AD FS 与 Azure AD 之间的信任是已基于此令牌签名证书的联合的信任。

但是，如果由于自动证书滚动更新或通过一些干预更改上的 AD FS 端的令牌签名证书，则必须联合域在 Azure AD 端上更新新证书的详细信息。 不同于 Azure Ad 上的 AD FS 的主令牌签名证书时，由 Azure AD 是不受信任的 AD fs 颁发的令牌。 因此，联合的用户不允许登录。

若要修复此，可以使用的步骤概述了在[续订 Office 365 和 Azure Active Directory 联合身份验证证书](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)。

## <a name="other-common-things-to-check"></a>其他要检查的常见事项
下面是要检查遇到问题的 AD FS 和 Azure AD 交互的事项一览表。
- Windows 凭据管理器中的过时或缓存凭据
- 适用于 Office 365 的信赖方信任配置的安全哈希算法设置为 SHA1

## <a name="next-steps"></a>后续步骤

- [AD FS 疑难解答](ad-fs-tshoot-overview.md)