---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: 配置 AD FS 对存储在 LDAP 目录中的用户进行身份验证
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 87ada412e6b4ab47aa18a62953b84d8a1369dffa
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544512"
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories"></a>配置 AD FS 对存储在 LDAP 目录中的用户进行身份验证

以下主题介绍了使 AD FS 基础结构能够对其标识存储在轻型目录访问协议 (LDAP) v3 兼容目录中的用户进行身份验证所需的配置。

在许多组织中, 身份管理解决方案由 Active Directory、AD LDS 或第三方 LDAP 目录的组合组成。 添加了 AD FS 对对存储在符合 LDAP v3 的目录中的用户进行身份验证的支持, 您可以从整个企业级 AD FS 功能集中获益, 无论用户身份存储在何处。 AD FS 支持任何符合 LDAP v3 的目录。

> [!NOTE]
> 某些 AD FS 功能包括单一登录 (SSO)、设备身份验证、灵活的条件性访问策略、通过与 Web 应用程序代理集成的任何位置支持工作, 以及与 Azure AD 的无缝联合使你和你的用户能够利用云, 包括 Office 365 和其他 SaaS 应用程序。  有关详细信息, 请参阅[Active Directory 联合身份验证服务概述](../../ad-fs/AD-FS-2016-Overview.md)。

为了使 AD FS 从 LDAP 目录对用户进行身份验证, 必须通过创建**本地声明提供程序信任**将此 LDAP 目录连接到 AD FS 场。  本地声明提供程序信任是表示 AD FS 场中的 LDAP 目录的信任对象。 本地声明提供程序信任对象包含各种标识符、名称和用于将此 LDAP 目录标识到本地联合身份验证服务的规则。

你可以通过添加多个**本地声明提供程序信任**, 在同一 AD FS 场中支持多个具有自己的配置的 LDAP 目录。 此外, 也可以将不受 AD FS 所在的林信任的 AD DS 林建模为本地声明提供程序信任。 可以使用 Windows PowerShell 创建本地声明提供程序信任。

LDAP 目录 (本地声明提供程序信任) 可以与 AD 目录 (声明提供程序信任) 共存于同一 AD FS 场中的同一 AD FS 服务器上, 因此, AD FS 的单个实例可以对存储在 AD 和非 AD 目录中。

仅支持基于窗体的身份验证从 LDAP 目录对用户进行身份验证。 在 LDAP 目录中对用户进行身份验证时不支持基于证书和集成的 Windows 身份验证。

LDAP 目录中存储的标识还支持 AD FS 支持的所有被动授权协议 (包括 SAML、WS-FEDERATION 和 OAuth)。

对于存储在 LDAP 目录中的标识, 也支持 WS 信任 active authorization 协议。

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>配置 AD FS 对 LDAP 目录中存储的用户进行身份验证
若要将 AD FS 场配置为通过 LDAP 目录对用户进行身份验证, 可以完成以下步骤:

1. 首先, 使用**AdfsLdapServerConnection** cmdlet 配置与 LDAP 目录的连接:

   ```
   $DirectoryCred = Get-Credential
   $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
   ```

   > [!NOTE]
   > 建议为要连接到的每个 LDAP 服务器创建一个新的连接对象。 AD FS 可以连接到多个副本 LDAP 服务器, 并在特定 LDAP 服务器关闭时自动进行故障转移。 对于这种情况, 可以为每个副本 LDAP 服务器创建一个 AdfsLdapServerConnection, 然后使用**AdfsLocalClaimsProviderTrust** cmdlet 的-**LdapServerConnection**参数添加连接对象的数组。

   **注意：** 尝试使用 Get 凭据并键入 DN 和密码来绑定到 LDAP 实例可能会导致失败, 因为用户界面要求使用特定输入格式, 例如, 域 \ 用户名或user@domain.tld。 可以改为按如下所示使用 Convertto-html-SecureString cmdlet (下面的示例假定 uid = admin, ou = system 作为要用于绑定到 LDAP 实例的凭据的 DN):

   ```
   $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
   $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
   ```

   然后输入 uid = admin 的密码并完成其余步骤。

2. 接下来, 可以使用**AdfsLdapAttributeToClaimMapping** cmdlet 执行将 LDAP 属性映射到现有 AD FS 声明的可选步骤。 在下面的示例中, 将 givenName、姓和 CommonName LDAP 属性映射到 AD FS 声明:

   ```
   #Map given name claim
   $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
   # Map surname claim
   $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
   # Map common name claim
   $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
   ```

   此映射的目的是为了使 LDAP 存储中的属性可用作 AD FS 中的声明, 以便在 AD FS 中创建条件性访问控制规则。 它还通过提供一种简单的方法将 LDAP 属性映射到声明, 使 AD FS 可以在 LDAP 存储中使用自定义架构。

3. 最后, 必须使用**AdfsLocalClaimsProviderTrust** CMDLET 将 LDAP 存储与 AD FS 作为本地声明提供方信任注册:

   ```
   Add-AdfsLocalClaimsProviderTrust -Name "Vendors" -Identifier "urn:vendors" -Type Ldap

   # Connection info
   -LdapServerConnection $vendorDirectory 

   # How to locate user objects in directory
   -UserObjectClass inetOrgPerson -UserContainer "CN=VendorsContainer,CN=VendorsPartition" -LdapAuthenticationMethod Basic 

   # Claims for authenticated users
   -AnchorClaimLdapAttribute mail -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -LdapAttributeToClaimMapping @($GivenName, $Surname, $CommonName) 

   # General claims provider properties
   -AcceptanceTransformRules "c:[Type != ''] => issue(claim=c);" -Enabled $true 

   # Optional - supply user name suffix if you want to use Ws-Trust
   -OrganizationalAccountSuffix "vendors.contoso.com"
   ```

   在上面的示例中, 你将创建名为 "供应商" 的本地声明提供程序信任。 你正在指定 AD FS 连接到本地声明提供程序信任表示的 LDAP 目录的连接信息, 方法是`$vendorDirectory`将其`-LdapServerConnection`分配给参数。 请注意, 在第一步中, `$vendorDirectory`已分配了连接到特定 LDAP 目录时要使用的连接字符串。 最后, 您指定`$GivenName`将、 `$Surname`和`$CommonName` LDAP 属性 (映射到 AD FS 声明) 用于条件性访问控制, 包括多重身份验证策略和颁发授权规则, 以及通过 AD FS 颁发的安全令牌中的声明进行颁发。 若要将 Ws 信任之类的活动协议与 AD FS 一起使用, 则必须指定 OrganizationalAccountSuffix 参数, 该参数允许 AD FS 在为 active authorization 请求提供服务时区分本地声明提供程序信任。

## <a name="see-also"></a>请参阅
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)


