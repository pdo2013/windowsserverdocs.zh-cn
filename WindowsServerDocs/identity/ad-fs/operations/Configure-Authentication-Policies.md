---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: 配置身份验证策略
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9345f995af2f256dddcbcbd7d05c4bf6170b563e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189856"
---
# <a name="configure-authentication-policies"></a>配置身份验证策略

在 AD FS 中，在 Windows Server 2012 R2 中，同时访问控制和身份验证机制已得到增强，包括用户、 设备、 位置和身份验证数据的多个因素。 这些增强功能，可以，通过用户界面或通过 Windows PowerShell，若要管理的访问权限授予 AD FS 的风险\-保护的应用程序中的通过多\-因素访问控制和多\-基于用户标识或组成员身份、 网络位置、 工作区的设备数据的身份验证\-加入和身份验证状态时多\-身份验证\(MFA\)执行。  
  
详细了解 MFA 和多重\-因素中 Active Directory 联合身份验证服务的访问控制\(AD FS\)在 Windows Server 2012 R2 中，请参阅以下主题：  


-   [工作区从任何设备加入实现 SSO 和无缝第二重身份验证跨公司应用程序](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [使用条件访问控制管理风险](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [使用针对敏感应用程序的附加多重身份验证管理风险](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>配置身份验证策略通过 AD FS 管理管理单元\-中  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
在 AD FS 中，在 Windows Server 2012 R2 中，可以指定适用于所有应用程序和服务的受 AD FS 保护的全局范围内的身份验证策略。 此外可以设置特定的应用程序和服务的依赖方信任和受 AD FS 保护的身份验证策略。 指定身份验证策略为特定应用程序按照信赖方信任不会覆盖全局身份验证策略。 如果全局或按信赖方信任身份验证策略需要 MFA，MFA 用户尝试向此信赖方信任进行身份验证时触发。 全局身份验证策略是一种回退的应用程序和服务不具有特定配置的身份验证策略的信赖方信任。 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>若要在 Windows Server 2012 R2 中全局配置主要身份验证 
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在 AD FS 管理单元\-中，单击**身份验证策略**。  
  
3.  在中**主要身份验证**部分中，单击**编辑**旁边**全局设置**。 此外可以右键\-单击**身份验证策略**，然后选择**编辑全局主要身份验证**，也可以在**操作**窗格中，选择**编辑全局主要身份验证**。  
![auth policies](media/Configure-Authentication-Policies/authpolicy1.png)
  
4.  在中**编辑全局身份验证策略**窗口，然后在**主**选项卡上，可以将以下设置配置为全局身份验证策略的一部分：  
  
    -   要用于主要身份验证的身份验证方法。 可以选择下可用的身份验证方法**Extranet**并**Intranet**。  
  
    -   通过设备身份验证**启用设备身份验证**复选框。 有关详细信息，请参阅 [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。  
![auth policies](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>若要配置主要身份验证按信赖方信任  
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在 AD FS 管理单元\-中，单击**身份验证策略**\\**按信赖方信任**，然后单击你想要配置身份验证的信赖方信任策略。  
  
3.  可以立即\-单击你想要配置身份验证策略，然后选择信赖方信任**编辑自定义主验证**，也可以在**操作**窗格中，选择**编辑自定义主验证**。  
![auth policies](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  在中**编辑身份验证的策略 < 信赖\_方\_信任\_名称 >** 窗口下**主**选项卡上，可以配置以下设置作为的一部分**每个信赖方信任**身份验证策略：  
  
    -   是否要求用户提供其凭据登录在每次\-中通过**要求用户提供其凭据登录在每次\-中**复选框。  
![auth policies](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>若要全局配置多重身份验证  
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在 AD FS 管理单元\-中，单击**身份验证策略**。  
  
3.  在中**多\-重身份验证**部分中，单击**编辑**旁边**全局设置**。 此外可以右键\-单击**身份验证策略**，然后选择**编辑全局多重\-重身份验证**，也可以在**操作**窗格中，选择**编辑全局多重\-因素身份验证**。  
![auth policies](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  在中**编辑全局身份验证策略**窗口下**多\-身份**选项卡上，可以将以下设置配置为属于全局多\-身份身份验证策略：  
  
    -   设置或通过在可用选项的 MFA 条件**用户\/组**，**设备**，并且**位置**部分。  
  
    -   若要启用的任何这些设置 MFA，必须选择至少一个额外的身份验证方法。 **证书身份验证**是默认可用的选项。 此外可以配置其他自定义的附加身份验证方法，例如，Windows Azure Active 身份验证。 有关详细信息，请参阅[操作实例指南：使用针对敏感应用程序的附加多重身份验证管理风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。  
  
> [!WARNING]  
> 仅可以全局配置其他身份验证方法。  
![auth policies](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>若要配置多\-身份验证按信赖方信任  
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在 AD FS 管理单元\-中，单击**身份验证策略**\\**按信赖方信任**，然后单击你想要配置 MFA 的信赖方信任。  
  
3.  可以立即\-单击你想要配置 MFA，，然后选择信赖方信任**编辑自定义多\-重身份验证**，也可以在**操作**窗格中，选择**编辑自定义多\-重身份验证**。  
  
4.  在中**编辑身份验证的策略 < 信赖\_方\_信任\_名称 >** 窗口下**多\-身份**选项卡上，你可以将以下设置配置的一部分为每个\-信赖方信任身份验证策略：  
  
    -   设置或通过在可用选项的 MFA 条件**用户\/组**，**设备**，并且**位置**部分。  

## <a name="configure-authentication-policies-via-windows-powershell"></a>配置身份验证策略通过 Windows PowerShell  
Windows PowerShell 允许更灵活地使用访问控制的不同因素和所需的身份验证机制来配置身份验证策略和授权的 Windows Server 2012 R2 中的 AD FS 中的可用规则为 AD FS 实现，则返回 true 的条件性访问\-保护的资源。  
  
在管理员或等效项，在本地计算机上的成员身份是完成这些过程的最低要求。  查看详细了解如何使用适当帐户和组成员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   
  
### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>若要配置通过 Windows PowerShell 的其他身份验证方法  
  
1.  联合身份验证服务器，打开 Windows PowerShell 命令窗口并运行以下命令。  
  

    `Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `

  
> [!WARNING]  
> 若要验证是否已成功运行此命令，可以运行 `Get-AdfsGlobalAuthenticationPolicy` 命令。  
  
### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>若要配置每个 MFA\-基于用户的组成员身份数据的信赖方信任  
  
1.  在联合服务器上，打开 Windows PowerShell 命令窗口并运行以下命令：  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
  
  
> [!WARNING]  
> 确保替换 *< 信赖\_方\_信任 >* 与你的信赖方信任的名称。  
  
2.  在同一个 Windows PowerShell 命令窗口中，运行以下命令。  
  
 
    $MfaClaimRule = “c:[Type == ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid’”, Value =~ ‘“^(?i) <group_SID>$’”] => issue(Type = ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod’”, Value ‘“https://schemas.microsoft.com/claims/multipleauthn’”);” 
    
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules $MfaClaimRule
  
  
> [!NOTE]  
> 确保替换 < 组\_SID > 安全标识符的值与\(SID\)你的 Active Directory \(AD\)组。  
  
### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>若要配置全局基于用户的组成员身份数据的 MFA  
  
1.  联合身份验证服务器，打开 Windows PowerShell 命令窗口并运行以下命令。  
  

    $MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid’", Value == ‘"group_SID’"]  
     => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod’", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn’");”  
      
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> 确保替换 *< 组\_SID >* 的 AD 组的 SID 的值。  
  
### <a name="to-configure-mfa-globally-based-on-users-location"></a>若要配置 MFA 全局基于用户的位置  
  
1.  联合身份验证服务器，打开 Windows PowerShell 命令窗口并运行以下命令。  
  
 
    $MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork’", Value == ‘"true_or_false’"]  
     => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod’", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn’");”  
  
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
  

  
> [!NOTE]  
> 确保替换 *< true\_或\_false >* 与`true`或`false`。 值取决于您是否访问请求来自 extranet 或 intranet 基于的特定规则条件。  
  
### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>若要配置 MFA 全局基于用户的设备数据  
  
1.  联合身份验证服务器，打开 Windows PowerShell 命令窗口并运行以下命令。  
  

    $MfaClaimRule = "c:[Type == ‘" https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser’", Value == ‘"true_or_false"']  
     => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod’", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn’");"  
  
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> 确保替换 *< true\_或\_false >* 与`true`或`false`。 值取决于您根据设备是否为工作区的特定规则条件\-或未联接。  
  
### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>如果访问请求从 extranet 和非全局范围内配置 MFA\-工作区\-已加入的设备  
  
1.  联合身份验证服务器，打开 Windows PowerShell 命令窗口并运行以下命令。  
  
 
    `Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
 
  
> [!NOTE]  
> 确保替换的两个实例 *< true\_或\_false >* 与`true`或`false`，这取决于特定的规则条件。 规则条件基于设备是否工作区\-加入或不以及访问请求来自 extranet 或 intranet。  
  
### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>如果访问来自属于特定组的 extranet 用户全局范围内配置 MFA  
  
1.  联合身份验证服务器，打开 Windows PowerShell 命令窗口并运行以下命令。  
  

    Set-AdfsAdditionalAuthenticationRule "c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value == `"group_SID`"] && c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value== `"true_or_false`"] => issue(Type = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`", Value =`"https://schemas.microsoft.com/claims/
      
> [!NOTE]  
> 确保替换 *< 组\_SID >* 与组 SID 的值并 *< true\_或\_false >* 使用`true`或`false`，这取决于您的特定规则条件基于是否访问请求来自 extranet 或 intranet。  
  
### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>若要授予访问权限基于通过 Windows PowerShell 的用户数据的应用程序  
  
1.  联合身份验证服务器，打开 Windows PowerShell 命令窗口并运行以下命令。  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> 确保替换 *< 信赖\_方\_信任 >* 值为你的信赖方信任。  
  
2.  在同一个 Windows PowerShell 命令窗口中，运行以下命令。  
  
    ```  
  
      $GroupAuthzRule = "@RuleTemplate = `“Authorization`” @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
    ```  
  
> [!NOTE]  
> > 确保替换 *< 组\_SID >* 的 AD 组的 SID 的值。  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>若要授予访问受 AD FS 仅当此用户的标识的应用程序已使用 MFA 验证  
  
1.  联合身份验证服务器，打开 Windows PowerShell 命令窗口并运行以下命令。  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> 确保替换 *< 信赖\_方\_信任 >* 值为你的信赖方信任。  
  
2.  在同一个 Windows PowerShell 命令窗口中，运行以下命令。  
  
    ```  
    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessWithMFA`"  
    c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = ‘“PermitUsersWithClaim’");"  
  
    ```  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>若要授予访问权限的应用程序的受 AD FS 仅当访问请求来自工作区\-已加入到用户注册的设备  
  
1.  联合身份验证服务器，打开 Windows PowerShell 命令窗口并运行以下命令。  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> 确保替换 *< 信赖\_方\_信任 >* 值为你的信赖方信任。  
  
2.  在同一个 Windows PowerShell 命令窗口中，运行以下命令。  
  

    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
    c:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");  
  

  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>若要授予访问权限的应用程序的受 AD FS 仅当访问请求来自工作区\-已加入到其标识已使用 MFA 进行验证的用户注册的设备  
  
1.  联合身份验证服务器，打开 Windows PowerShell 命令窗口并运行以下命令。  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> 确保替换 *< 信赖\_方\_信任 >* 值为你的信赖方信任。  
  
2.  在同一个 Windows PowerShell 命令窗口中，运行以下命令。  
  
    ```  
    $GroupAuthzRule = ‘@RuleTemplate = “Authorization”  
    @RuleName = “RequireMFAOnRegisteredWorkplaceJoinedDevice”  
    c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
    c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$”] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
  
    ```  
  
### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>若要授予对受 AD FS，仅当访问请求来自其标识已使用 MFA 进行验证的用户的应用程序的 extranet 访问权限  
  
1.  联合身份验证服务器，打开 Windows PowerShell 命令窗口并运行以下命令。  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  

  
> [!NOTE]  
> 确保替换 *< 信赖\_方\_信任 >* 值为你的信赖方信任。  
  
2.  在同一个 Windows PowerShell 命令窗口中，运行以下命令。  
  

    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"RequireMFAForExtranetAccess`"  
    c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
    c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value =~ `"^(?i)false$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  

## <a name="additional-references"></a>其他参考  

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
