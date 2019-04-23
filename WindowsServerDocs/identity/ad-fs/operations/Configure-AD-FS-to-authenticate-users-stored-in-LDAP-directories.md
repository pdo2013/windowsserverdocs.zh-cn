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
ms.openlocfilehash: 05f8b8991e664a84c3f2b3200de4068af8d1476a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846608"
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories"></a>配置 AD FS 对存储在 LDAP 目录中的用户进行身份验证

>适用于：Windows Server 2016

以下主题介绍启用 AD FS 基础结构，其标识存储在轻型目录访问协议 (LDAP) v3 兼容目录中的用户进行身份验证所需的配置。

在许多组织中，标识管理解决方案包含 Active Directory、 AD LDS 中或第三方 LDAP 目录的组合。 添加了对存储在 v3 符合 LDAP 的目录中的用户进行身份验证的 AD FS 支持，可受益从整个企业 AD FS 功能集而不考虑用户标识的存储位置。 AD FS 支持任何 LDAP v3 兼容的目录。

> [!NOTE]
> 一些 AD FS 功能包括上单一登录 (SSO)，设备身份验证、 灵活的条件性访问策略、 支持的工作-从的任意位置通过集成 Web 应用程序代理，且无缝地使用它在将 Azure AD 的联合身份验证使您和您的用户能够利用云，包括 Office 365 和其他 SaaS 应用程序。  有关详细信息，请参阅[Active Directory 联合身份验证服务概述](../../ad-fs/AD-FS-2016-Overview.md)。

为了使 AD FS 进行身份验证 LDAP 目录中的用户，你必须连接此 LDAP 目录到 AD FS 场通过创建**本地声明提供方信任**。  本地声明提供方信任是一个表示 AD FS 场中的 LDAP 目录的信任对象。 本地声明提供方信任对象包含各种标识符、 名称和标识此 LDAP 目录更改为本地联合身份验证服务的规则。

可以支持多个 LDAP 目录，每个都有其自己的配置，通过添加多个同一个 AD FS 场内**本地声明提供方信任**。 此外，还可以作为本地声明提供方信任建模 AD FS 所在的林不受信任的 AD DS 林。 可以使用 Windows PowerShell 来创建本地声明提供方信任。

LDAP 目录 （本地声明提供方信任） 可同时与 AD 目录 （声明提供方信任） 存在相同的 AD FS 场中的相同的 AD FS 服务器上，因此，AD FS 的单个实例，则能够进行身份验证和授权的用户的访问这两个 AD 中存储和非 AD 目录。

从 LDAP 目录的用户进行身份验证支持仅基于窗体的身份验证。 LDAP 目录中的用户进行身份验证不支持基于证书的和集成 Windows 身份验证。

所有被动授权协议的支持的 AD FS 中，包括 SAML，WS 联合身份验证和 LDAP 目录中存储的标识还支持 OAuth。

Ws-trust active 授权协议还支持 LDAP 目录中存储的标识。

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>配置 AD FS 进行身份验证 LDAP 目录中存储的用户
若要配置 AD FS 场从 LDAP 目录的用户进行身份验证，可以完成以下步骤：

1.  首先，配置与 LDAP 目录使用的连接**新建 AdfsLdapServerConnection** cmdlet:

    ```
    $DirectoryCred = Get-Credential
    $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
    ```

    > [!NOTE]
    > 建议创建一个新的连接对象想要连接到每个 LDAP 服务器。 AD FS 可以连接到多个副本 LDAP 服务器，并在特定的 LDAP 服务器已关闭的情况下，自动故障转移。 对于这种情况下，您可以为每个这些副本 LDAP 服务器创建一个 AdfsLdapServerConnection 以及如何将使用连接对象的数组-**LdapServerConnection**参数的**添加 AdfsLocalClaimsProviderTrust** cmdlet。

    **注意：** 你尝试使用 Get-credential，并键入 DN 和密码以用于绑定到 LDAP 实例可能会导致失败，因为的特定输入格式，例如，域 \ 用户名的用户界面要求或user@domain.tld。 而是可以使用 Convertto-securestring cmdlet，如下所示 (下面的示例假定 uid = admin，ou = 系统作为要用于绑定到 LDAP 实例的凭据的 DN):

    ```
    $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
    $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
    ```

    然后输入的密码 uid = admin 并完成余下的步骤。

2.  接下来，可以执行的 LDAP 属性映射到使用的现有 AD FS 声明的可选步骤**新建 AdfsLdapAttributeToClaimMapping** cmdlet。 在下面的示例中，您将映射 givenName、 姓氏和 CommonName LDAP 属性到 AD FS 声明：

    ```
    #Map given name claim
    $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
    # Map surname claim
    $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
    # Map common name claim
    $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
    ```

    做此映射是为了使从 LDAP 存储区的属性可作为在 AD FS 以便在 AD FS 中创建条件性访问控制规则的声明。 它还使 AD FS，可以轻松地将 LDAP 属性映射到声明，从而使用 LDAP 存储中的自定义架构。

3.  最后，您必须注册的 LDAP 存储 AD FS，因为本地声明提供程序信任使用**添加 AdfsLocalClaimsProviderTrust** cmdlet:

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

    在上述示例中，将创建名为"供应商"本地声明提供方信任。 指定此本地声明提供方信任表示通过将分配 AD FS 以连接到 LDAP 目录的连接信息`$vendorDirectory`到`-LdapServerConnection`参数。 请注意，在第一步，你已分配`$vendorDirectory`用于连接到特定的 LDAP 目录时要使用的连接字符串。 最后，您在指定`$GivenName`， `$Surname`，和`$CommonName`LDAP 属性 （这与您映射到 AD FS 声明） 是用于进行条件性访问控制，包括多重身份验证策略和颁发授权规则，以及对于通过 AD FS 颁发安全令牌中声明的颁发。 若要与 AD FS 配合使用 Ws 信任之类的活动协议，必须指定 OrganizationalAccountSuffix 参数，以使 AD FS 以区分本地声明提供方信任时活动的授权请求提供服务。

## <a name="see-also"></a>请参阅
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)


