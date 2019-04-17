---
ms.assetid: b734cbcb-342c-4a28-8ab5-b9cd990bb1c2
title: "何时使用授权索赔规则"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 144e382692e8f2a6732f8c7c5b8a1dd6049192cb
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

# <a name="when-to-use-an-authorization-claim-rule"></a>何时使用授权索赔规则
在你需要采用传入的索赔类型，然后应用将确定是否允许用户拒绝访问根据您规则中指定的值操作时，你可以使用 Active Directory 联合身份验证服务 \(AD FS\) 在此规则。 当你使用此规则时，你将通过或转换索赔相匹配以下规则逻辑，根据你在规则配置的选项之一：  
  
|规则选项|规则逻辑|  
|---------------|--------------|  
|允许所有用户|如果接收的索赔类型等于*任何声称类型*和重视等*任何值*，然后问题声称与值等*允许*|  
|允许访问到用户与此传入的声明|如果接收的索赔类型等于*指定索赔类型*和重视等*指定声明值*，然后问题声称与值等*允许*|  
|拒绝访问到用户与此传入的声明|如果接收的索赔类型等于*指定索赔类型*和重视等*指定声明值*，然后问题声称与值等*拒绝*|  
  
以下部分提供了基本的介绍声称规则，并提供有关何时可以使用此规则的更多详细信息。  
  
## <a name="about-claim-rules"></a>有关索赔规则  
索赔规则表示企业逻辑，将需要传入的索赔，到该应用条件实例 \（如果然后 y\ x）和产生传出声明条件参数而异。 以下列表轮廓重要阅读进一步之前，你应该提前了解的提示声称规则本主题中：  
  
-   在 snap\ 中广告 FS 管理索赔规则只能创建使用索赔规则模板  
  
-   索赔规则进程传入索赔直接从索赔提供商 \（如 Active Directory 或其他联盟 Service\）或的输出中验收转换上索赔提供商信任规则。  
  
-   索赔规则处理给定的规则组内的时间顺序索赔颁发引擎。 通过在规则设置优先级，可以进一步优化或筛选将生成一给定的规则组中的以前规则的索赔。  
  
-   索赔规则模板将始终需要你指定传入的索赔类型。 但是，你可以使用相同的索赔类型使用单个规则处理多个索赔值。  
  
有关详细信息索赔规则和索赔规则集，请参阅[声称规则的角色](The-Role-of-Claim-Rules.md)。 有关如何处理规则的详细信息，请参阅[的索赔引擎角色](The-Role-of-the-Claims-Engine.md)。 如何处理声明规则集的详细信息，请参阅[的索赔管道角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="permit-all-users"></a>允许所有用户  
当你使用允许所有用户规则模板时，所有用户将都有权访问依赖方。 但是，你可以使用其他授权规则进一步限制访问。 如果一个规则允许用户访问依赖方，，另一个规则拒绝依赖方用户访问拒绝结果重写允许结果和拒绝该用户访问。  
  
允许访问到依赖方联合身份验证服务中的用户可能仍会拒绝服务依赖方。  
  
## <a name="permit-access-to-users-with-this-incoming-claim"></a>允许访问到用户与此传入的声明  
当你使用的允许或拒绝用户基于上传入声称规则模板创建规则并设置允许条件时，你可以到依赖方基于类型和传入的声明的值允许特定用户访问。 例如，可以使用此规则模板创建规则将允许那些声称值为域管理某个组的用户。 如果一个规则允许用户访问依赖方，，另一个规则拒绝依赖方用户访问拒绝结果重写允许结果和拒绝该用户访问。  
  
用户有权访问依赖方从联合身份验证服务可能仍会拒绝服务依赖方。 如果你想要允许所有用户访问依赖方，使用都允许所有用户规则模板。  
  
## <a name="deny-access-to-users-with-this-incoming-claim"></a>拒绝访问到用户与此传入的声明  
当你使用的允许或拒绝用户基于上传入声称规则模板创建一个规则并设置情况，若要拒绝时，你可以根据类型和传入的声明的值信赖方拒绝用户的访问权限。 例如，可以使用此规则模板创建将拒绝有一组中的所有用户都声称值为域用户规则。  
  
如果你想要使用拒绝条件，但也可以通过访问依赖方特定用户，你稍后明确必须添加授权规则与允许条件启用依赖方这些用户访问。  
  
如果用户被拒绝访问索赔颁发引擎处理规则集时，处理进一步规则关机并广告 FS 返回到用户请求"拒绝访问"错误。  
  
## <a name="authorizing-users"></a>在授权的用户  
在广告 FS 授权使用规则发出许可证或拒绝将确定是否用户或一组用户的索赔 \（具体取决于索赔类型 used\) 将允许访问 Web\ 基于资源给定信赖方或不。 授权规则只能信赖方信任上设置。  
  
### <a name="authorization-rule-sets"></a>授权规则集  
不同的授权规则集存在根据允许类型，或拒绝需要进行配置的操作。 这些规则集包括：  
  
-   **颁发授权规则**：这些规则确定是否用户可以接收依赖方提起的索赔，因此，访问依赖方。  
  
-   **委派授权规则**：这些世纪的用户可以充当信赖方到其他用户。 当用户充当其他用户时，索赔有关请求用户仍放置在标记。  
  
-   **模拟授权规则**：这些规则确定用户是否可以完全模拟依赖方到其他用户。 模拟另一个用户是一项功能强大功能，因为信赖方不知道所模拟用户。  
  
有关如何授权规则过程适应索赔颁发管道的更多详细信息，请参阅的索赔颁发引擎角色。  
  
### <a name="supported-claim-types"></a>受支持的索赔类型  
广告 FSdefines 两声称类型，用于确定用户是否允许使用或拒绝。 这些声明类型统一资源标识符 \(URIs\) 如下所示：  
  
1.  **允许**: http:///\/schemas.microsoft.com\/authorization\/claims\/permit  
  
2.  **拒绝**: http:///\/schemas.microsoft.com\/authorization\/claims\/deny  
  
## <a name="how-to-create-this-rule"></a>如何创建此规则  
你可以创建使用索赔规则语言，或者使用这两个授权规则**允许所有用户**规则模板或**允许拒绝用户根据或传入声称**规则 snap\ 中广告 FS 管理模板。 允许所有用户规则模板不提供任何配置选项。 但是，允许或用户拒绝根据传入的索赔规则模板提供以下配置选项：  
  
-   指定声明规则名称  
  
-   指定传入的索赔类型  
  
-   键入传入的索赔值  
  
-   允许访问到用户与此传入的声明  
  
-   拒绝访问到用户与此传入的声明  
  
有关如何创建此模板相关说明进行操作，请参阅[创建允许所有用户规则](https://technet.microsoft.com/library/ee913577.aspx)或[上传入的索赔创建规则应用于允许或拒绝用户基于](https://technet.microsoft.com/library/ee913594.aspx)广告 FS 部署指南中。  
  
## <a name="using-the-claim-rule-language"></a>使用索赔规则语言  
如果仅在声明值匹配自定义模式时，应该发送索赔，必须使用自定义规则。 有关详细信息，请参阅[何时使用自定义索赔规则](When-to-Use-a-Custom-Claim-Rule.md)。  
  
### <a name="example-of-how-to-create-an-authorization-rule-based-on-multiple-claims"></a>如何创建多个索赔根据授权规则的示例  
当使用索赔规则语言语法授权索赔，可以还根据的多个索赔用户的原始索赔中存在发出索赔。 用户是组编辑器的成员，并且已使用 Windows 身份验证身份验证时才会以下规则发出授权索赔：  
  
```  
[type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod",   
value == "urn:federation:authentication:windows" ]  
&& [type == "http://schemas.xmlsoap.org/claims/Group ", value == “editors”]   
=> issue(type = "http://schemas.xmlsoap.org/claims/authZ", value = "Granted");  
```  
  
### <a name="example-of-how-to-create-authorization-rules-that-will-delegate-who-can-create-or-remove-federation-server-proxy-trusts"></a>如何创建授权的示例规则，将委托可创建或删除联合身份验证的服务器代理信任的人员  
联合身份验证服务可以使用联合身份验证的服务器代理重定向的客户端请求之前，必须首先联合身份验证服务和联合身份验证的服务器代理计算机之间建立信任。 默认情况下，建立代理信任时以下凭据任一提供成功广告 FS 联合身份验证的代理配置向导中：  
  
-   所使用的代理将保护联合身份验证服务、的服务帐户  
  
-   组成员的本地管理员联盟服务器电场的日落中的所有联盟服务器上的 Active Directory 域帐户  
  
当你要指定的用户可以给定的联合身份验证服务用于创建代理信任时，你可以使用任何以下委派方法。 此列表的方法是委派的优先顺序，根据广告 FS 产品团队建议最安全的而且至少问题的方法。 则需要使用以下方法，具体取决于你的组织的需求之一：  
  
1.  在 Active Directory 中创建域安全组 \（例如，FSProxyTrustCreators\）添加到每个电场的日落，在联合身份验证的服务器上的本地管理员组此组，然后添加所需委派此右到新组的用户帐户。 这是首选的方法。  
  
2.  添加到每一场中联合身份验证的服务器上的管理员组的用户域帐户。  
  
3.  如果出于某些原因你无法使用以下方法之一，你还可以实现此目的创建授权规则。 虽然不推荐这样做，因为可能可能会发生，如果此规则书写不正确的问题，你可以使用自定义授权规则委派帐户可以还创建或甚至删除之间与给定的联合身份验证服务相关联的所有联合 server 代理信任的 Active Directory 域用户。  
  
    如果你选择方法 3，可以使用以下规则语法处理可指定的用户授权索赔 \（在本例中为 contoso\\frankm\）来创建一个或多个联盟信任服务器的代理联合身份验证服务。 你必须应用使用的 Windows PowerShell 命令此规则**Set\ ADFSProperties AddProxyAuthorizationRules**。  
  
    ```  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", issuer=~"^AD AUTHORITY$" value == "contoso\frankm" ] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true")  
  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
    更高版本，如果你想要删除的用户，以便用户可以不再创建代理信任，你可以还原默认代理信任授权规则要删除的用户联合身份验证服务用于创建代理信任右侧。 你必须将应用使用的 Windows PowerShell 命令此规则**Set\ ADFSProperties AddProxyAuthorizationRules**。  
  
    ```  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
有关如何使用索赔规则语言的详细信息，请参阅[声称规则语言的角色](The-Role-of-the-Claim-Rule-Language.md)。  
  

