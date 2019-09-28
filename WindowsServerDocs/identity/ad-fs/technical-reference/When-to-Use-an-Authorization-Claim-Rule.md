---
ms.assetid: b734cbcb-342c-4a28-8ab5-b9cd990bb1c2
title: 何时使用授权声明规则
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 49246d9df294b966f0ba38b1d3c1f361ce5f1d5f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407269"
---
# <a name="when-to-use-an-authorization-claim-rule"></a>何时使用授权声明规则
如果需要采用传入声明类型， \(则\)可以在 Active Directory 联合身份验证服务 AD FS 中使用此规则，然后应用一项操作，该操作将根据你在规则中指定。 使用此规则时，将根据你在此规则中配置的任一选项传递或转换与以下规则逻辑匹配的声明：  
  
|规则选项|规则逻辑|  
|---------------|--------------|  
|允许所有用户|如果传入声明类型等于 *任何声明类型* 并且值等于 *任何值*，则发出值等于 *“允许”* 的声明|  
|允许具有此传入声明的用户访问|如果传入声明类型等于*指定的声明类型*并且值等于*指定的声明值*，则发出值等于 *“允许”* 的声明|  
|拒绝具有此传入声明的用户访问|如果传入声明类型等于 *指定的声明类型* 并且值等于 *指定的声明值*，则发出值等于 *“拒绝”* 的声明|  
  
以下部分提供了声明规则的基本介绍，并提供了有关何时使用此规则的更多详细信息。  
  
## <a name="about-claim-rules"></a>关于声明规则  
声明规则表示将接受传入声明的业务逻辑实例，如果 x 之后为 y \(\) ，则对其应用条件，并基于条件参数生成传出声明。 下面的列表概述了在进一步阅读本主题中的内容之前应了解的有关声明规则的重要提示：  
  
-   在 AD FS 管理 "管理\-单元中，只能使用声明规则模板创建声明规则  
  
-   声明规则直接从声明提供程序\(（例如 Active Directory 或另一个联合身份验证服务\) ）或在声明提供方信任的接受转换规则的输出中处理传入声明。  
  
-   声明规则由声明颁发引擎按给定规则集内的时间顺序处理。 通过为规则设置优先级，可以进一步优化或筛选由给定规则集内以前的规则生成的声明。  
  
-   声明规则模板始终要求你指定传入声明类型。 但是，你可以使用单个规则处理声明类型相同的多个声明值。  
  
有关声明规则和声明规则集的更多详细信息，请参阅[声明规则的角色](The-Role-of-Claim-Rules.md)。 有关如何处理规则的详细信息，请参阅[声明引擎的角色](The-Role-of-the-Claims-Engine.md)。 有关如何处理声明规则集的详细信息，请参阅[声明管道的角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="permit-all-users"></a>允许所有用户  
使用“允许所有用户”规则模板时，所有用户都将有权访问信赖方。 但是，你可以使用其他授权规则进一步限制访问。 如果一项规则允许用户访问信赖方，而另一项规则拒绝用户访问信赖方，那么，拒绝结果将覆盖允许结果，用户将被拒绝访问。  
  
有权从联合身份验证服务访问信赖方的用户可能仍会被信赖方拒绝服务。  
  
## <a name="permit-access-to-users-with-this-incoming-claim"></a>允许具有此传入声明的用户访问  
使用 "根据传入声明允许或拒绝用户" 规则模板来创建规则并设置允许的条件时，可以根据传入声明的类型和值允许特定用户访问信赖方。 例如，可以使用此规则模板创建一项规则，该规则仅允许那些具有值为 Domain Admins 的组声明的用户。 如果一项规则允许用户访问信赖方，而另一项规则拒绝用户访问信赖方，那么，拒绝结果将覆盖允许结果，用户将被拒绝访问。  
  
有权从联合身份验证服务访问信赖方的用户可能仍会被信赖方拒绝服务。 如果希望允许所有用户访问信赖方，则使用“允许所有用户”规则模板。  
  
## <a name="deny-access-to-users-with-this-incoming-claim"></a>拒绝具有此传入声明的用户访问  
当你使用 "根据传入声明允许或拒绝用户" 规则模板来创建规则并将条件设置为 "拒绝" 时，可以根据传入声明的类型和值拒绝用户访问信赖方。 例如，可以使用此规则模板创建一项规则，该规则将拒绝所有具有值为 Domain Users 的组声明的用户。  
  
如果想要使用拒绝条件，但同时允许特定用户访问信赖方，那么，稍后必须显式添加具有允许条件的授权规则，以便允许这些用户访问信赖方。  
  
如果在声明发出引擎处理规则集时拒绝用户访问，则将关闭进一步的规则处理，并 AD FS 向用户的请求返回 "拒绝访问" 错误。  
  
## <a name="authorizing-users"></a>向用户授权  
在 AD FS 中，授权规则用于发出允许或拒绝声明，以确定是否允许某个用户或用户\(组根据所使用\)的声明类型在给定的信赖中访问基于 Web\-的资源参与方。 只能在信赖方信任上设置授权规则。  
  
### <a name="authorization-rule-sets"></a>授权规则集  
根据你需要配置的允许或拒绝操作的类型，存在不同的授权规则集。 这些规则集包括：  
  
-   **发出授权规则**：这些规则确定用户是否可以接收有关信赖方的声明，从而访问该信赖方。  
  
-   **委派授权规则**：这些规则确定用户在面向信赖方时是否可以充当另一位用户。 当用户充当另一位用户时，有关发出请求的用户的声明仍位于令牌中。  
  
-   **模拟授权规则**：这些规则确定用户在面向信赖方时是否可以完全模拟另一位用户。 模拟另一位用户是一项非常强大的功能，因为信赖方不知道被模拟的用户是谁。  
  
有关授权规则过程如何适应声明发出管道的更多详细信息，请参阅“声明发出引擎的角色”。  
  
### <a name="supported-claim-types"></a>支持的声明类型  
AD FS 定义了两个用于确定是允许还是拒绝用户的声明类型。 这些声明类型统一资源标识符\(uri\)如下所示：  
  
1.  **允许**： http：\/\/schemas.microsoft.com\/授权\/声明允许\/  
  
2.  **拒绝**： http：\/\/schemas.microsoft.com\/authorization\/声明Deny\/  
  
## <a name="how-to-create-this-rule"></a>如何创建此规则  
您可以使用声明规则语言或使用 "AD FS 管理" 管理单元\-中的 "**允许所有用户**" 规则模板或 "**根据传入声明允许或拒绝用户**" 规则模板来创建这两种授权规则。 “允许所有用户”规则模板不提供任何配置选项。 但是，“根据传入声明允许或拒绝用户”规则模板提供下列配置选项：  
  
-   指定声明规则名称  
  
-   指定传入声明类型  
  
-   键入传入声明值  
  
-   允许具有此传入声明的用户访问  
  
-   拒绝具有此传入声明的用户访问  
  
有关如何创建此模板的详细说明，请参阅[创建规则以允许所有用户](https://technet.microsoft.com/library/ee913577.aspx)，或[创建一个规则，以便根据 AD FS 部署指南中的传入声明允许或拒绝用户](https://technet.microsoft.com/library/ee913594.aspx)。  
  
## <a name="using-the-claim-rule-language"></a>使用声明规则语言  
如果仅在声明值与自定义模式匹配时才发送声明，则必须使用自定义规则。 有关详细信息，请参阅[何时使用自定义声明规则](When-to-Use-a-Custom-Claim-Rule.md)。  
  
### <a name="example-of-how-to-create-an-authorization-rule-based-on-multiple-claims"></a>有关如何基于多个声明创建授权规则的示例  
使用声明规则语言语法来授权声明时，还可以基于用户原始声明中存在的多个声明来发出声明。 下面的规则仅当用户是组编辑器的成员且使用 Windows 身份验证通过了身份验证时才发出授权声明：  
  
```  
[type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod",   
value == "urn:federation:authentication:windows" ]  
&& [type == "http://schemas.xmlsoap.org/claims/Group ", value == “editors”]   
=> issue(type = "http://schemas.xmlsoap.org/claims/authZ", value = "Granted");  
```  
  
### <a name="example-of-how-to-create-authorization-rules-that-will-delegate-who-can-create-or-remove-federation-server-proxy-trusts"></a>有关如何创建授权规则以委派可创建或删除联合服务器代理信任的人员的示例  
必须先在联合身份验证服务和联合服务器代理计算机之间建立信任，联合身份验证服务才能使用联合服务器代理重定向客户端请求。 默认情况下，在 AD FS 联合服务器代理配置向导中成功提供以下凭据之一时，将建立代理信任：  
  
-   联合身份验证服务所用的、由代理提供保护的服务帐户  
  
-   作为联合服务器场中所有联合服务器上的本地管理员组成员的 Active Directory 域帐户  
  
如果想要指定可以为给定联合身份验证服务创建代理信任的用户，可以使用以下任一委派方法。 此方法列表按优先级顺序排列，具体取决于 AD FS 产品团队对最安全且问题最少的委派方法的建议。 只能根据组织需求使用这些方法中的其中一种：  
  
1.  在 Active Directory \(中创建域安全组例如 FSProxyTrustCreators\)，将此组添加到场中每台联合服务器上的本地管理员组，然后仅添加所需的用户帐户将此权限委托给新组。 这是首选方法。  
  
2.  将用户的域帐户添加到场中每台联合服务器上的 "管理员" 组。  
  
3.  如果出于某种原因而无法使用上述任一方法，还可以为此创建授权规则。 虽然不建议这样做（因为如果未正确编写此规则，可能会出现并发症），但你可以使用自定义授权规则委派哪些 Active Directory 域用户帐户还可以创建甚至删除所有与给定联合身份验证服务关联的联合服务器代理之间的信任。  
  
    如果选择方法3，则可以使用以下规则语法颁发授权声明，该声明允许指定用户\(在这种情况下，contoso\\frankm\)为一个或多个联合服务器代理创建信任。联合身份验证服务。 必须使用 Windows PowerShell 命令 **\-Set set-adfsproperties AddProxyAuthorizationRules**应用此规则。  
  
    ```  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", issuer=~"^AD AUTHORITY$" value == "contoso\frankm" ] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true")  
  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
    之后，如果想要删除用户，使用户再也不能创建代理信任，你可以还原到默认代理信任授权规则，以删除用户为联合身份验证服务创建代理信任的权限。 还必须使用 Windows PowerShell 命令 **\-Set set-adfsproperties AddProxyAuthorizationRules**应用此规则。  
  
    ```  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
有关如何使用声明规则语言的详细信息，请参阅[声明规则语言的角色](The-Role-of-the-Claim-Rule-Language.md)。  
  

