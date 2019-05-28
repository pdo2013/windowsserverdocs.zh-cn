---
ms.assetid: b734cbcb-342c-4a28-8ab5-b9cd990bb1c2
title: 何时使用授权声明规则
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6b852a580bdc0ea02643d478dc51b5cbcd2eac4b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188307"
---
# <a name="when-to-use-an-authorization-claim-rule"></a>何时使用授权声明规则
可以在 Active Directory 联合身份验证服务中使用此规则\(AD FS\)何时需要采取传入声明类型，然后应用一项操作，以确定是否将允许或拒绝访问用户基于值的在规则中指定。 使用此规则时，将根据你在此规则中配置的任一选项传递或转换与以下规则逻辑匹配的声明：  
  
|规则选项|规则逻辑|  
|---------------|--------------|  
|允许所有用户|如果传入声明类型等于 *任何声明类型* 并且值等于 *任何值*，则发出值等于 *“允许”* 的声明|  
|允许具有此传入声明的用户访问|如果传入声明类型等于*指定的声明类型*并且值等于*指定的声明值*，则发出值等于 *“允许”* 的声明|  
|拒绝具有此传入声明的用户访问|如果传入声明类型等于 *指定的声明类型* 并且值等于 *指定的声明值*，则发出值等于 *“拒绝”* 的声明|  
  
以下部分提供了声明规则的基本介绍，并提供了有关何时使用此规则的更多详细信息。  
  
## <a name="about-claim-rules"></a>关于声明规则  
声明规则表示业务逻辑，使用传入声明、 向它应用条件的实例\(if x，then y\)和生成传出声明基于条件参数。 下面的列表概述了在进一步阅读本主题中的内容之前应了解的有关声明规则的重要提示：  
  
-   在 AD FS 管理管理单元\-中，声明只可以使用声明规则模板创建规则  
  
-   声明规则过程传入声明既可以直接从声明提供程序\(Active Directory 或另一个联合身份验证服务等\)或输出中的接受转换规则声明提供方信任上的。  
  
-   声明规则由声明颁发引擎按给定规则集内的时间顺序处理。 通过为规则设置优先级，可以进一步优化或筛选由给定规则集内以前的规则生成的声明。  
  
-   声明规则模板始终要求你指定传入声明类型。 但是，你可以使用单个规则处理声明类型相同的多个声明值。  
  
有关详细信息有关声明规则和声明规则集的详细信息，请参阅[规则的角色声明](The-Role-of-Claim-Rules.md)。 有关如何处理规则的详细信息，请参阅[The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。 有关如何处理声明规则集的详细信息，请参阅[声明管道的角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="permit-all-users"></a>允许所有用户  
使用“允许所有用户”规则模板时，所有用户都将有权访问信赖方。 但是，你可以使用其他授权规则进一步限制访问。 如果一项规则允许用户访问信赖方，而另一项规则拒绝用户访问信赖方，那么，拒绝结果将覆盖允许结果，用户将被拒绝访问。  
  
有权从联合身份验证服务访问信赖方的用户可能仍会被信赖方拒绝服务。  
  
## <a name="permit-access-to-users-with-this-incoming-claim"></a>允许具有此传入声明的用户访问  
使用“根据传入声明允许或拒绝用户”规则模板来创建规则并设置允许的条件时，可以根据传入声明的类型和值允许特定用户访问信赖方。 例如，可以使用此规则模板创建一项规则，该规则仅允许那些具有值为 Domain Admins 的组声明的用户。 如果一项规则允许用户访问信赖方，而另一项规则拒绝用户访问信赖方，那么，拒绝结果将覆盖允许结果，用户将被拒绝访问。  
  
有权从联合身份验证服务访问信赖方的用户可能仍会被信赖方拒绝服务。 如果希望允许所有用户访问信赖方，则使用“允许所有用户”规则模板。  
  
## <a name="deny-access-to-users-with-this-incoming-claim"></a>拒绝具有此传入声明的用户访问  
使用“根据传入声明允许或拒绝用户”规则模板来创建规则并设置拒绝的条件时，可以根据传入声明的类型和值拒绝用户访问信赖方。 例如，可以使用此规则模板创建一项规则，该规则将拒绝所有具有值为 Domain Users 的组声明的用户。  
  
如果想要使用拒绝条件，但同时允许特定用户访问信赖方，那么，稍后必须显式添加具有允许条件的授权规则，以便允许这些用户访问信赖方。  
  
如果用户被拒绝访问时声明发出引擎处理规则集，更多的规则处理关闭和 AD FS 对用户的请求将返回"拒绝访问"错误。  
  
## <a name="authorizing-users"></a>向用户授权  
在 AD FS 中，授权规则用于发出允许或拒绝声明，以确定用户或用户组是否\(具体取决于所使用的声明类型\)有权访问 Web\-基于中给定的依赖的资源或不参与方。 只能在信赖方信任上设置授权规则。  
  
### <a name="authorization-rule-sets"></a>授权规则集  
根据你需要配置的允许或拒绝操作的类型，存在不同的授权规则集。 这些规则集包括：  
  
-   **发出授权规则**：这些规则确定用户是否可以接收有关信赖方的声明，从而访问该信赖方。  
  
-   **委派授权规则**：这些规则确定用户在面向信赖方时是否可以充当另一位用户。 当用户充当另一位用户时，有关发出请求的用户的声明仍位于令牌中。  
  
-   **模拟授权规则**：这些规则确定用户在面向信赖方时是否可以完全模拟另一位用户。 模拟另一位用户是一项非常强大的功能，因为信赖方不知道被模拟的用户是谁。  
  
有关授权规则过程如何适应声明发出管道的更多详细信息，请参阅“声明发出引擎的角色”。  
  
### <a name="supported-claim-types"></a>支持的声明类型  
AD FS 定义两个用于确定是允许还是拒绝用户的声明类型。 这些声明类型的统一资源标识符\(Uri\)如下所示：  
  
1.  **允许**: http:\/\/schemas.microsoft.com\/授权\/声明\/允许  
  
2.  **拒绝**: http:\/\/schemas.microsoft.com\/授权\/声明\/拒绝  
  
## <a name="how-to-create-this-rule"></a>如何创建此规则  
你可以创建使用声明规则语言或使用这两个授权规则**允许所有用户**规则模板或**允许或拒绝用户根据传入声明**的 AD FS 中的规则模板管理管理单元\-中。 “允许所有用户”规则模板不提供任何配置选项。 但是，“根据传入声明允许或拒绝用户”规则模板提供下列配置选项：  
  
-   指定声明规则名称  
  
-   指定传入声明类型  
  
-   键入传入声明值  
  
-   允许具有此传入声明的用户访问  
  
-   拒绝具有此传入声明的用户访问  
  
有关如何创建此模板的详细说明，请参阅[创建规则，以便允许所有用户](https://technet.microsoft.com/library/ee913577.aspx)或[上传入声明创建规则，以便允许或拒绝用户基于](https://technet.microsoft.com/library/ee913594.aspx)AD FS 部署指南中。  
  
## <a name="using-the-claim-rule-language"></a>使用声明规则语言  
如果仅在声明值与自定义模式匹配时才发送声明，则必须使用自定义规则。 有关详细信息，请参阅 [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md)。  
  
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
  
如果想要指定可以为给定联合身份验证服务创建代理信任的用户，可以使用以下任一委派方法。 根据 AD FS 产品团队对最安全且问题最少的委派方法的建议，此列表中的方法按优先级排序。 只能根据组织需求使用这些方法中的其中一种：  
  
1.  在 Active Directory 中创建域安全组\(例如，FSProxyTrustCreators\)，将此组添加到每个场中的联合身份验证服务器上本地管理员组，然后添加到所需的用户帐户若要委派此权限授予新的组。 这是首选方法。  
  
2.  将用户的域帐户添加到场中每台联合服务器上的管理员组中。  
  
3.  如果出于某种原因而无法使用上述任一方法，还可以为此创建授权规则。 虽然不建议这样做（因为如果未正确编写此规则，可能会出现并发症），但你可以使用自定义授权规则委派哪些 Active Directory 域用户帐户还可以创建甚至删除所有与给定联合身份验证服务关联的联合服务器代理之间的信任。  
  
    如果你选择方法 3，可以使用以下规则语法发出授权声明，将允许指定的用户\(在此情况下，contoso\\frankm\)创建信任的一个或多个联合服务器代理到联合身份验证服务。 必须将应用此规则使用 Windows PowerShell 命令**设置\-ADFSProperties AddProxyAuthorizationRules**。  
  
    ```  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", issuer=~"^AD AUTHORITY$" value == "contoso\frankm" ] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true")  
  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
    之后，如果想要删除用户，使用户再也不能创建代理信任，你可以还原到默认代理信任授权规则，以删除用户为联合身份验证服务创建代理信任的权限。 您还必须应用此规则使用 Windows PowerShell 命令**设置\-ADFSProperties AddProxyAuthorizationRules**。  
  
    ```  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
有关如何使用声明规则语言的详细信息，请参阅[的声明规则语言角色](The-Role-of-the-Claim-Rule-Language.md)。  
  

