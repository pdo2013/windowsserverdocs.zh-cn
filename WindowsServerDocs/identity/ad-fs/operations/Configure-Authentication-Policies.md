---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: "配置认证策略"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7faffb7ccbb4b0ea3c65329d18f915d7dafcd46f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="configure-authentication-policies"></a>配置认证策略

>适用于： Windows Server 2012 R2

在广告 FS，在 Windows Server 2012 R2，同时访问控制和验证机制增强具有多个因素包括用户、 设备、 位置和身份验证数据。 这些增强功能，你的用户界面通过或通过 Windows PowerShell 管理授予访问权限的广告 FS\ 保护应用程序中通过 multi\ 因素访问控制和 multi\ 强双因素身份验证根据用户的身份或一组成员、 网络位置、 workplace\ 已加入的设备数据的风险，身份验证状态的时间 multi\ 强双因素身份 \(MFA\) 执行。  
  
有关在 Windows Server 2012 R2 的 Active Directory 联合身份验证服务 \(AD FS\) 中 MFA 和 multi\ 因素访问控制的详细信息，请参阅以下主题：  


-   [加入工作区从任何设备 SSO 和无缝第二个跨公司应用程序因素身份验证](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [管理具有条件访问控制的风险](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [管理敏感应用程序的其他多重身份验证的风险](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>配置广告 FS 管理 snap\ 中通过的身份验证策略  
在会员**管理员**，或等效，在本地计算机上最低要求才能完成这些步骤。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
在广告 FS，在 Windows Server 2012 R2，你可以指定身份验证策略适用于所有应用和服务进行保护的广告 FS 全局范围。 你还可以设置为特定应用程序和服务，以依赖于方信任受 AD FS 认证策略。 指定每依赖某个特定应用程序身份验证策略方信任不会覆盖策略全球身份验证。 如果全球或每依赖用户尝试此信赖的方信任验证身份时触发身份验证策略要求 MFA，MFA 方信任。 策略全球身份验证是一种回退为信赖的方信任的应用程序和没有配置的身份验证特定策略的服务。 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>若要在 Windows Server 2012 R2 全球配置主要身份验证 
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  在广告 FS snap\ 中，单击**认证策略**。  
  
3.  在**主要身份验证**部分中，单击**编辑**旁边**全球设置**。 你还可以 right\ 单击**认证策略**，并选择**编辑全球主要身份验证**，或下**操作**窗格中选择**编辑全球主要身份验证**。  
![auth 策略](media/Configure-Authentication-Policies/authpolicy1.png)
  
4.  在**编辑策略全球的身份验证**窗口，然后在**主要**选项卡上，你可以将以下设置配置为策略全球身份验证的一部分：  
  
    -   要用于主要身份验证的身份验证方法。 你可以选择提供身份验证方法下的**外联网**和**Intranet**。  
  
    -   通过设备身份验证**启用设备身份验证**复选框。 有关详细信息，请参阅[加入工作区从任何设备 SSO 和无缝第二个因素身份验证在公司应用程序](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。  
![auth 策略](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>若要配置每个依赖的主要身份验证方信任  
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  在广告 FS snap\ 中，单击**认证策略**\\**每依赖方信任**，然后单击你想要配置身份验证策略信赖方信任。  
  
3.  不论采取何 right\ 键单击依赖方信任要配置身份验证的策略，然后依次选择**编辑自定义主要验证**，或下**操作**窗格中选择**编辑自定义主要验证**。  
![auth 策略](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  在**编辑 < relying\_party\_trust\_name > 身份验证策略**窗口下**主要**选项卡上，可以将以下设置配置为的一部分**每依赖方信任**身份验证策略：  
  
    -   是否需要提供他们的凭据 sign\ 中在每次用户通过**用户将需要提供他们的凭据 sign\ 中在每次**复选框。  
![auth 策略](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>全球配置多重身份验证  
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  在广告 FS snap\ 中，单击**认证策略**。  
  
3.  在**Multi\ 强双因素身份**部分中，单击**编辑**旁边**全球设置**。 你还可以 right\ 单击**身份验证的策略**，并选择**全球编辑 Multi\ 强双因素身份**，或下**操作**窗格中，选择**全球编辑 Multi\ 强双因素身份**。  
![auth 策略](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  在**编辑策略全球的身份验证**窗口下**Multi\ 因素**选项卡上，你可以将以下设置配置为全球的 multi\ 双因素身份验证策略的一部分：  
  
    -   设置或通过下的可用选项 MFA 条件**Users\ 中的组**，**设备**，并**位置**部分。  
  
    -   若要使这些设置的任何 MFA，必须选择至少一个额外的身份验证方法。 **身份验证的证书**是默认的可用选项。 你还可以将其他自定义额外的身份验证方法，例如，Windows Azure Active 身份验证配置为。 有关详细信息，请参阅[演练指南： 管理敏感应用程序的其他多重身份验证的风险](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。  
  
> [!WARNING]  
> 你只能全球配置额外的身份验证方法。  
![auth 策略](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>若要配置每依赖 multi\ 双因素身份验证方信任  
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  在广告 FS snap\ 中，单击**认证策略**\\**每依赖方信任**，然后单击你希望配置 MFA 信赖方信任。  
  
3.  不论采取何 right\ 键单击依赖方信任要配置 MFA，然后依次选择**编辑自定义 Multi\ 强双因素身份**，或下**操作**窗格中选择**编辑自定义 Multi\ 强双因素身份**。  
  
4.  在**编辑 < relying\_party\_trust\_name > 身份验证策略**窗口下**Multi\ 因素**选项卡上，可以将以下设置配置为的一部分 per\ 依赖方信任身份验证策略：  
  
    -   设置或通过下的可用选项 MFA 条件**Users\ 中的组**，**设备**，并**位置**部分。  

## <a name="configure-authentication-policies-via-windows-powershell"></a>配置通过 Windows PowerShell 身份验证策略  
Windows PowerShell 使使用各种因素访问控制更大的灵活性和适用于 Windows Server 2012 R2 认证策略和授权配置中广告 FS 的身份验证机制规则的所需实现如此的条件访问您的广告 FS \-secured 资源。  
  
会员管理员，或在本地计算机上的等效并完成这些步骤最低要求。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)\ (http:///\/ go.microsoft.com\/fwlink\ /？LinkId\ = 83477\)。   
  
### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>若要配置通过 Windows PowerShell 额外的身份验证方法  
  
1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口，并运行以下命令。  
  

    `Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `

  
> [!WARNING]  
> 若要验证是否已成功运行此命令，你可以运行`Get-AdfsGlobalAuthenticationPolicy`命令。  
  
### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>若要配置 MFA per\ 依赖方信任基于用户的组成员数据  
  
1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口，并运行以下命令：  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
  
  
> [!WARNING]  
> 确保替换*< relying\_party\_trust >*你依赖的方信任的名称。  
  
2.  在同一 Windows PowerShell 命令窗口中，运行以下命令。  
  
 
    $MfaClaimRule ="c: [类型 = ="https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'"，值 = ~"^(?i) < group_SID >$"] = > 问题 (类型 ="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'"，值"https://schemas.microsoft.com/claims/multipleauthn'");" 
    
    Set-adfsrelyingpartytrust – TargetRelyingParty $rp – AdditionalAuthenticationRules $MfaClaimRule
  
  
> [!NOTE]  
> 确保替换 < group\_SID > 使用安全标识符值的 Active Directory \(AD\) 组 \(SID\)。  
  
### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>若要配置 MFA 全球基于用户的组成员数据  
  
1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口，并运行以下命令。  
  

    $MfaClaimRule ="c: [类型 = ="https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'"，值 = ="group_SID'"]  
     = > 问题 (键入 ="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'"，值 ="https://schemas.microsoft.com/claims/multipleauthn'");"  
      
    设置 AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> 确保替换*< group\_SID >*与 SID 为广告组的价值。  
  
### <a name="to-configure-mfa-globally-based-on-users-location"></a>若要配置 MFA 全球基于用户的位置  
  
1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口，并运行以下命令。  
  
 
    $MfaClaimRule ="c: [类型 = ="https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'"，值 = ="true_or_false'"]  
     = > 问题 (键入 ="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'"，值 ="https://schemas.microsoft.com/claims/multipleauthn'");"  
  
    设置 AdfsAdditionalAuthenticationRule $MfaClaimRule  
  

  
> [!NOTE]  
> 确保替换*< true\_or\_false >*有`true`或`false`。 值取决于所采用的访问权限请求是否来自联网或 intranet 你特定规则条件。  
  
### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>若要配置 MFA 全球基于用户的设备数据  
  
1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口，并运行以下命令。  
  

    $MfaClaimRule ="c: [类型 = ="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'"，值 ="true_or_false"=]  
     = > 问题 (键入 ="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'"，值 ="https://schemas.microsoft.com/claims/multipleauthn'");"  
  
    设置 AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> 确保替换*< true\_or\_false >*有`true`或`false`。 值取决于你根据该设备是否 workplace\ 加入的特定规则条件。  
  
### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>若要配置 MFA 是全球访问请求涉及从联网和 non\ workplace\ 加入的设备  
  
1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口，并运行以下命令。  
  
 
    `Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
 
  
> [!NOTE]  
> 确保要更换的两个实例*< true\_or\_false >*有`true`或`false`，这取决于你的特定规则条件。 规则条件根据该设备是否 workplace\ 加入和是否访问请求来自联网或 intranet。  
  
### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>若要访问来自所属的某些组外部网络用户全球配置 MFA  
  
1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口，并运行以下命令。  
  

    Set-AdfsAdditionalAuthenticationRule"c: [类型 = = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`"，值 = = `"group_SID`"] & & c2: [类型 = = `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`"，值 = = `"true_or_false`"] = > 问题 (类型 = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`"，值 ="https://schemas.microsoft.com/claims/
      
> [!NOTE]  
> 确保替换*< group\_SID >*与 SID 组的值和*< true\_or\_false >*有`true`或`false`，这取决于你基于是否访问请求来自联网的特定规则条件或 intranet。  
  
### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>若要授予访问基于 Windows PowerShell 通过用户数据的应用程序  
  
1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口，并运行以下命令。  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> 确保替换*< relying\_party\_trust >*与你依赖的方信任的价值。  
  
2.  在同一 Windows PowerShell 命令窗口中，运行以下命令。  
  
    ```  
  
      $GroupAuthzRule = "@RuleTemplate = `“Authorization`” @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
    ```  
  
> [!NOTE]  
> > 确保替换*< group\_SID >*与 SID 为广告组的价值。  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>授予访问权限的应用程序受广告 FS 才该用户的身份验证的 MFA 与  
  
1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口，并运行以下命令。  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> 确保替换*< relying\_party\_trust >*与你依赖的方信任的价值。  
  
2.  在同一 Windows PowerShell 命令窗口中，运行以下命令。  
  
    ```  
    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessWithMFA`"  
    c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = ‘“PermitUsersWithClaim’");"  
  
    ```  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>若要授予访问权限的应用程序受仅广告 FS 如果访问请求来自 workplace\ 加入注册的设备的用户  
  
1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口，并运行以下命令。  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> 确保替换*< relying\_party\_trust >*与你依赖的方信任的价值。  
  
2.  在同一 Windows PowerShell 命令窗口中，运行以下命令。  
  

    $GroupAuthzRule ="@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
    c: [类型 = = `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`"，值 = ~ `"^(?i)true$`"] = > 问题 (类型 = `"https://schemas.microsoft.com/authorization/claims/permit`"，值 = `"PermitUsersWithClaim`");  
  

  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>若要授予访问权限的应用程序受仅广告 FS 如果访问请求来自 workplace\ 加入注册的设备已与 MFA 验证其身份一个用户到  
  
1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口，并运行以下命令。  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> 确保替换*< relying\_party\_trust >*与你依赖的方信任的价值。  
  
2.  在同一 Windows PowerShell 命令窗口中，运行以下命令。  
  
    ```  
    $GroupAuthzRule = ‘@RuleTemplate = “Authorization”  
    @RuleName = “RequireMFAOnRegisteredWorkplaceJoinedDevice”  
    c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
    c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$”] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
  
    ```  
  
### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>若要授予向应用程序受 AD FS 才访问请求来自已与 MFA 验证其身份一个用户外部网络的访问权限  
  
1.  在你联盟服务器上，打开 Windows PowerShell 命令窗口，并运行以下命令。  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  

  
> [!NOTE]  
> 确保替换*< relying\_party\_trust >*与你依赖的方信任的价值。  
  
2.  在同一 Windows PowerShell 命令窗口中，运行以下命令。  
  

    $GroupAuthzRule ="@RuleTemplate = `"Authorization`"  
    @RuleName = `"RequireMFAForExtranetAccess`"  
    c1: [类型 = = `"https://schemas.microsoft.com/claims/authnmethodsreferences`"，值 = ~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] & &  
    c2: [类型 = = `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`"，值 = ~ `"^(?i)false$`"] = > 问题 (类型 = `"https://schemas.microsoft.com/authorization/claims/permit`"，值 = `"PermitUsersWithClaim`");"  

## <a name="additional-references"></a>其他参考  

[广告 FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
