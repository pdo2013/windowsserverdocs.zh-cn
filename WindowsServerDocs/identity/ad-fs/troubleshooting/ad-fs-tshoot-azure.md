---
title: AD FS 故障排除-Azure AD
description: 本文档介绍如何排查 AD FS 和 Azure AD 的各个方面
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 293618b3fe2a24caff8fd6b52c5528cc699f93de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407289"
---
# <a name="ad-fs-troubleshooting---azure-ad"></a>AD FS 故障排除-Azure AD
随着云的增长，许多企业都在使用 Azure AD 来实现其各种应用和服务。  与 Azure AD 联合已成为许多组织的标准做法。  本文档将介绍解决此联合身份验证问题的一些方面。  一般疑难解答文档中的几个主题仍适用于与 Azure 联合，因此本文档重点介绍 Azure AD 和 AD FS 交互的细节。

## <a name="redirection-to-ad-fs"></a>重定向到 AD FS
当你登录到某个应用程序（如 Office 365），并且你已 "重定向" 到组织 AD FS 服务器登录时，会发生重定向。

![](media/ad-fs-tshoot-azure/azure1.png)


### <a name="first-things-to-check"></a>要检查的第一项
如果没有进行重定向，则需要检查几项

   1. 通过登录到 Azure 门户并在 Azure AD Connect 下进行检查，确保 Azure AD 租户启用了联合身份验证。

![](media/ad-fs-tshoot-azure/azure2.png)

1. 请确保通过单击 Azure 门户中 "联合身份验证" 旁边的域来验证自定义域。
   ![](media/ad-fs-tshoot-azure/azure3.png)

2. 最后，您需要检查[DNS](ad-fs-tshoot-dns.md) ，并确保您的 AD FS 服务器或 WAP 服务器正在从 internet 进行解析。  验证此方法是否可解决，以及您能否导航到它。
3. 还可以使用 PowerShell cmdlt `Get-AzureADDomain` 获取此信息。

![](media/ad-fs-tshoot-azure/azure6.png)

### <a name="you-are-receiving-an-unknown-auth-method-error"></a>您收到未知的身份验证方法错误
在从 Azure 重定向时，可能会出现 "未知的身份验证方法" 错误，指出在 AD FS 或 STS 级别不支持 AuthnContext。 

当使用强制身份验证方法的参数 Azure AD 重定向到 AD FS 或 STS 时，这是最常见的。 

若要强制执行身份验证方法，请使用以下方法之一：
- 对于 WS 联合身份验证，请使用 WAUTH 查询字符串来强制使用首选的身份验证方法。

- 对于 SAML 2.0，请使用以下内容：
  ```
  <saml:AuthnContext>
  <saml:AuthnContextClassRef>
  urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
  </saml:AuthnContextClassRef>
  </saml:AuthnContext>
  ```
  当使用不正确的值发送强制身份验证方法时，或者如果 AD FS 或 STS 上不支持该身份验证方法，则会在进行身份验证之前收到一条错误消息。

|需要进行身份验证的方法|wauth URI|
|-----|-----|
|用户名和密码身份验证|urn： oasis：名称： tc： SAML：1.0： am： password|
|SSL 客户端身份验证|urn:ietf:rfc:2246|
|Windows 集成的身份验证|urn：联合身份验证：身份验证： windows|

支持的 SAML 身份验证上下文类

|身份验证方法|身份验证上下文类 URI|
|-----|-----| 
|用户名和密码|urn： oasis：名称： tc： SAML：2.0： ac：类：密码|
|受密码保护的传输|urn： oasis：名称： tc： SAML：2.0： ac：类： PasswordProtectedTransport|
|传输层安全性（TLS）客户端|urn： oasis：名称： tc： SAML：2.0： ac：类： TLSClient
|X.509 证书|urn： oasis：名称： tc： SAML：2.0： ac：类： X509
|集成的 Windows 身份验证|urn：联合身份验证：身份验证： windows|
|Kerberos|urn： oasis：名称： tc： SAML：2.0： ac：类： Kerberos|

若要确保 AD FS 级别支持身份验证方法，请检查以下各项。

#### <a name="ad-fs-20"></a>AD FS 2。0 

在 " **/adfs/ls/web.config**" 下，确保 "身份验证类型" 条目存在。

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

在**AD FS 管理**"下，单击 AD FS 管理单元中的"**身份验证策略**"。

在 "**主要身份验证**" 部分中，单击 "全局设置" 旁边的 "编辑"。 你还可以右键单击 "身份验证策略"，然后选择 "编辑全局主要身份验证"。 或者，在 "操作" 窗格中，选择 "编辑全局主要身份验证"。

在 "编辑全局身份验证策略" 窗口中的 "主要" 选项卡上，你可以将设置配置为全局身份验证策略的一部分。 例如，对于主要身份验证，可以在 "Extranet 和 Intranet" 下选择可用的身份验证方法。

\* * 确保选中 "所需的身份验证方法" 复选框。 

#### <a name="ad-fs-2016"></a>AD FS 2016

在**AD FS 管理**"下，单击 AD FS 管理单元中的"**服务**和**身份验证方法**"。

在 "**主要身份验证**" 部分中，单击 "编辑"。

在 "**编辑身份验证方法**" 窗口的 "主要" 选项卡上，你可以将设置配置为身份验证策略的一部分。

![](media/ad-fs-tshoot-azure/azure4.png)

## <a name="tokens-issued-by-ad-fs"></a>AD FS 颁发的令牌

### <a name="azure-ad-throws-error-after-token-issuance"></a>令牌颁发后 Azure AD 引发错误
AD FS 颁发令牌后，Azure AD 可能会引发错误。 在这种情况下，请检查是否存在以下问题：
- 令牌中 AD FS 颁发的声明应与 Azure AD 中用户的相应属性匹配。
- Azure AD 的令牌应包含以下必需的声明：
    - WSFED 
        - UPN此声明的值应与 Azure AD 中的用户的 UPN 匹配。
        - ImmutableID此声明的值应匹配 Azure AD 中用户的 sourceAnchor 或 ImmutableID。

若要获取 Azure AD 中的用户属性值，请运行以下命令行： `Get-AzureADUser –UserPrincipalName <UPN>`

![](media/ad-fs-tshoot-azure/azure5.png)

   - SAML 2.0：
       - IDPEmail此声明的值应与 Azure AD 中的用户的用户主体名称相匹配。
       - NAMEID此声明的值应匹配 Azure AD 中用户的 sourceAnchor 或 ImmutableID。

有关详细信息，请参阅[使用 SAML 2.0 标识提供程序实现单一登录](https://technet.microsoft.com/library/dn641269.aspx)。

### <a name="token-signing-certificate-mismatch-between-ad-fs-and-azure-ad"></a>AD FS 和 Azure AD 之间的令牌签名证书不匹配。

AD FS 使用令牌签名证书对发送到用户或应用程序的令牌进行签名。 AD FS 与 Azure AD 之间的信任是基于此令牌签名证书的联合信任。

但是，如果由于自动证书滚动更新或某些干预而更改了 AD FS 端的令牌签名证书，则必须在联合域的 Azure AD 端更新新证书的详细信息。 当 AD FS 上的主要令牌签名证书不同于 Azure Ad 时，Azure AD 颁发的令牌不受 AD FS 信任。 因此，不允许联合用户登录。

若要解决此问题，可以使用[续订 Office 365 的联合身份验证证书和 Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)中的步骤大纲。

## <a name="other-common-things-to-check"></a>要检查的其他常见注意事项
下面是在 AD FS 和 Azure AD 交互问题时要检查的事项的快速列表。
- Windows 凭据管理器中已过时或已缓存的凭据
- 在 Office 365 的信赖方信任上配置的安全哈希算法设置为 SHA1

## <a name="next-steps"></a>后续步骤

- [AD FS 疑难解答](ad-fs-tshoot-overview.md)