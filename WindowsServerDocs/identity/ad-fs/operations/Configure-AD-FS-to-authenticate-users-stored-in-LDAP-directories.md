---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: "配置广告 FS LDAP 目录中存储的用户进行身份验证"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05f8b8991e664a84c3f2b3200de4068af8d1476a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories"></a>配置广告 FS LDAP 目录中存储的用户进行身份验证

>适用于：Windows Server 2016

以下主题介绍了允许你广告 FS 基础结构进行身份验证其身份存储轻型目录访问协议 (LDAP) v3 兼容目录中的用户所需的配置。

在许多公司，标识管理解决方案包含的 Active Directory、广告 LDS 或第三方 LDAP 目录组合。 增加存储 LDAP v3 兼容目录中的用户身份验证的广告 FS 支持后，你可以受益从整个企业级广告 FS 功能集无论是否为你的用户身份的存储位置。 广告 FS 支持任何 LDAP v3 兼容的目录。

> [!NOTE]
> 某些广告 FS 功能包括单一登录 (SSO)、设备身份验证、灵活条件访问策略，对工作-从的任意位置通过 Web 应用程序代理，与集成和与 Azure AD 它在打开的无缝联盟允许您和您的用户可以利用云中进行构建，包括 Office 365 和其他 SaaS 应用程序的支持。  有关详细信息，请参阅[Active Directory 联合身份验证服务概述](../../ad-fs/AD-FS-2016-Overview.md)。

为了使广告 FS LDAP 目录中的用户进行身份验证，你必须此 LDAP 目录通过连接到广告 FS 场创建**本地索赔提供商信任**。  本地索赔提供商信任是代表广告 FS 场中的 LDAP 目录信任对象。 本地索赔对象包含各种标识符、名称和规则来标识该 LDAP 目录本地联合身份验证服务提供商信任。

您可以支持多个 LDAP 目录，每一个都有它自己的配置，通过添加多个相同的广告 FS 场中**本地索赔提供商信任**。 此外，还可以本地索赔提供商信任作为建模不受信任的广告 FS 存在于森林的广告 DS 森林。 你可以通过使用 Windows PowerShell 中创建本地索赔提供商信任。

LDAP 目录（本地索赔提供商信任）上可同时存在广告目录（索赔提供商信任）使用相同的广告 FS 服务器，在同一广告 FS 电场的日落，因此，广告 FS 的单个实例能够身份验证和授权的访问权限的用户，存储在两广告和无广告目录。

仅在基于形式的身份验证 LDAP 目录中的用户身份验证的支持。 不支持 LDAP 目录中的用户身份验证证书的和基于集成 Windows 身份验证。

所有被动授权协议 LDAP 目录中存储的身份还支持 OAuth 和广告 FS，包括 SAML，WS 联合身份验证的支持。

WS 信任活动授权协议还支持 LDAP 目录中存储的身份。

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>配置广告 FS LDAP 目录中存储的用户进行身份验证
若要配置你的广告 FS 场 LDAP 目录中的用户进行身份验证，你可以完成以下步骤：

1.  首先，将配置连接到你 LDAP 目录使用**新建 AdfsLdapServerConnection** cmdlet:

    ```
    $DirectoryCred = Get-Credential
    $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
    ```

    > [!NOTE]
    > 建议你创建新的连接对象为你想要连接到每个 LDAP 服务器。 广告 FS 可以连接到多个副本 LDAP 服务器，并自动特定 LDAP 服务器已关闭的情况下进行故障转移。 这种情况，你可以为每个副本 LDAP 服务器创建一个 AdfsLdapServerConnection，然后添加连接对象使用大量-**LdapServerConnection**参数**添加 AdfsLocalClaimsProviderTrust** cmdlet。

    **注意：**你尝试使用 Get-Credential，然后键入一个 DN 和密码，以用于绑定到 LDAP 实例可能导致失败，因为的特定输入格式，如域 \ 用户名用户界面要求或user@domain.tld。 如下所示改为使用 ConvertTo-SecureString cmdlet (下面的示例假定 uid = 管理员，ou = 作为 DN 的凭据，以用于绑定到 LDAP 实例的系统):

    ```
    $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
    $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
    ```

    Uid 输入的密码 = 管理员，并完成的其余步骤。

2.  接下来，你可以执行映射到使用现有的广告 FS 声明 LDAP 属性步骤可选**新建 AdfsLdapAttributeToClaimMapping** cmdlet。 在下面的示例中，映射 givenName、姓、，并为广告 FS 索赔属性 CommonName LDAP:

    ```
    #Map given name claim
    $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
    # Map surname claim
    $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
    # Map common name claim
    $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
    ```

    完成此映射以提供属性 LDAP 官方商城作为广告 FS 中以创建广告 FS 条件访问控制规则的索赔。 它还允许广告 FS 处理 LDAP 官方商城中的自定义架构通过提供简单的方式，映射的索赔均 LDAP 属性。

3.  最后，你必须注册 LDAP 官方商城与广告 FS 如本地索赔提供商信任使用**添加 AdfsLocalClaimsProviderTrust** cmdlet:

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

    在上方的示例中，您将创建本地索赔提供商信任称为"供应商"。 你所指定连接信息用于广告 FS 连接到 LDAP 目录此本地索赔提供商信任表示通过分配`$vendorDirectory`到`-LdapServerConnection`参数。 注意，在一个步骤中，您已分配`$vendorDirectory`连接字符串用于连接到你的特定 LDAP 目录。 最后，你要指定`$GivenName`，`$Surname`，并`$CommonName`LDAP 属性（其中你映射到广告 FS 索赔）将被用于于条件访问控制，包括多重身份验证的策略和颁发授权规则，以及通过在广告 FS 发出安全令牌索赔颁发。 使用与广告 FS Ws 信任等活动的协议，以便你必须指定 OrganizationalAccountSuffix 参数，可使广告 FS 服务活动授权请求时的本地索赔提供商信任之间的区分。

## <a name="see-also"></a>请参阅
[广告 FS 操作](../../ad-fs/AD-FS-2016-Operations.md)


