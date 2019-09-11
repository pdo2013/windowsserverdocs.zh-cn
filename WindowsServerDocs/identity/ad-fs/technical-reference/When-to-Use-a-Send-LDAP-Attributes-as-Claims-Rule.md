---
ms.assetid: 606f4196-b579-4806-a462-3abd4d93e87c
title: 何时使用“以声明方式发送 LDAP 属性”规则
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a1210918067bbb71ea26dff4db11561bbeb09dbd
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869253"
---
# <a name="when-to-use-a-send-ldap-attributes-as-claims-rule"></a>何时使用“以声明方式发送 LDAP 属性”规则
如果要颁发包含实际轻型目录\(访问\)协议\(LDAP\)属性值的传出声明，则可以在 Active Directory 联合身份验证服务 AD FS 中使用此规则。属性存储，然后将声明类型与每个 LDAP 属性相关联。 有关属性存储的详细信息，请参阅[属性存储的角色](The-Role-of-Attribute-Stores.md)。  
  
使用此规则时，会为与规则逻辑匹配的每个指定的 LDAP 属性发出声明，如下表中所述。  
  
|规则选项|规则逻辑|  
|---------------|--------------|  
|LDAP 属性到传出声明类型的映射|如果属性存储等于“指定属性存储”，而 LDAP 属性等于“指定值”，则将 LDAP 属性值映射到“指定传出声明”类型并发出声明。|  
  
以下部分提供声明规则的基本简介。 它们还提供有关何时将发送 LDAP 属性用作声明规则的详细信息。  
  
## <a name="about-claim-rules"></a>关于声明规则  
声明规则表示将接受传入声明的业务逻辑实例，如果 x 之后为 y \(\) ，则对其应用条件，并基于条件参数生成传出声明。 下面的列表概述了在进一步阅读本主题中的内容之前应了解的有关声明规则的重要提示：  
  
-   在 AD FS 管理 "管理\-单元中，只能使用声明规则模板创建声明规则  
  
-   声明规则直接从声明提供程序\(（例如 Active Directory 或另一个联合身份验证服务\) ）或在声明提供方信任的接受转换规则的输出中处理传入声明。  
  
-   声明规则由声明颁发引擎按给定规则集内的时间顺序处理。 通过为规则设置优先级，可以进一步优化或筛选由给定规则集内以前的规则生成的声明。  
  
-   声明规则模板始终要求你指定传入声明类型。 但是，你可以使用单个规则处理声明类型相同的多个声明值。  
  
有关声明规则和声明规则集的更多详细信息，请参阅[声明规则的角色](The-Role-of-Claim-Rules.md)。 有关如何处理规则的详细信息，请参阅[声明引擎的角色](The-Role-of-the-Claims-Engine.md)。 有关如何处理声明规则集的详细信息，请参阅[声明管道的角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="mapping-of-ldap-attributes-to-outgoing-claim-types"></a>LDAP 属性到传出声明类型的映射  
使用 "以声明方式发送 LDAP 属性" 规则模板时，可以从 LDAP 属性存储中选择属性，例如 Active Directory 或 Active Directory 域服务\(AD DS\)将其值作为声明发送给信赖方. 这实质上是将特定 LDAP 属性从定义的属性存储映射到一组可用于授权的传出声明。  
  
通过使用此模板，可以从一个规则添加多个属性（将作为多个声明发送）。 例如，可以使用此规则模板创建一个规则，该规则会从 **company** 和 **department** Active Directory 属性查找经过身份验证的用户的属性值，然后将这些值作为两个不同的传出声明发送。  
  
你还可以使用此规则发送所有用户的组成员身份。 如果要仅发送单个组成员身份，请使用“以声明方式发送组成员身份”规则模板。 有关详细信息，请参阅[何时使用发送组成员身份作为声明规则](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)。  
  
## <a name="how-to-create-this-rule"></a>如何创建此规则  
您可以使用声明规则语言或使用 "AD FS 管理" 管理单元\-中的 "以声明方式发送 LDAP 属性" 规则模板来创建此规则。 此规则模板提供以下配置选项：  
  
-   指定声明规则名称  
  
-   选择要从中提取 LDAP 属性的属性存储  
  
-   LDAP 属性到传出声明类型的映射  
  
有关如何创建此规则的详细信息，请参阅[创建将 LDAP 属性作为声明发送的规则](https://technet.microsoft.com/library/dd807115.aspx)。  
  
## <a name="using-the-claim-rule-language"></a>使用声明规则语言  
如果对 Active Directory、AD DS 或 Active Directory 轻型目录服务\(AD LDS\)的查询必须与**samAccountname**之外的 LDAP 属性进行比较，则必须改用自定义规则。 如果输入集中没有 Windows 帐户名称声明，则还必须使用自定义规则指定要用于查询 AD DS 或 AD LDS 的声明。  
  
以下示例旨在帮助你了解一些可以用于使用声明规则语言构造自定义规则以便查询和提取属性存储中的数据的各种方法。  
  
### <a name="example-how-to-query-an-adlds-attribute-store-and-return-a-specified-value"></a>例如：如何查询 AD LDS 属性存储并返回指定值  
参数必须用分号分隔。 第一个参数是 LDAP 筛选器。 后续参数是要对任何匹配对象返回的属性。  
  
下面的示例演示如何按**sAMAccountName**属性查找用户，并使用用户的 mail 属性值\-发出电子邮件地址声明：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"));  
  
```  
  
下面的示例演示如何通过**mail**属性查找用户，并使用用户的**title**和**Displayname**属性的值颁发标题和显示名称声明：  
  
```  
c:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress ", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "mail={0};title;displayname", param = c.Value);  
```  
  
下面的示例演示如何按邮件和标题查找用户，然后使用用户的**displayname**属性发出显示名称声明：  
  
```  
c1:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"] && c2:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "(&(mail={0})(title={1}));displayname", param = c1.Value, param = c2.Value);  
```  
  
### <a name="example-how-to-query-an-activedirectory-attribute-store-and-return-a-specified-value"></a>例如：如何查询 Active Directory 属性存储并返回指定值  
Active Directory 查询必须包含\(\)域名作为最后一个参数，以便 Active Directory 属性存储可以查询正确的域。 另外，支持相同的语法。  
  
以下示例演示如何按 **sAMAccountName** 属性在其域中查找用户，然后返回 **mail** 属性：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
### <a name="example-how-to-query-an-activedirectory-attribute-store-based-on-the-value-of-an-incoming-claim"></a>例如：如何基于传入声明的值查询 Active Directory 属性存储  
  
```  
c:[Type == "http://test/name"]  
  
   => issue(store = "Enterprise AD Attribute Store",  
  
         types = ("http://test/email"),  
  
         query = ";mail;{0}",  
  
         param = c.Value)  
  
```  
  
上面的查询由以下三个部分组成：  
  
-   LDAP 筛选器 — 可指定查询的此部分以检索要对其查询属性的对象。 有关有效 LDAP 查询的常规信息，请参阅 RFC 2254。 当你在查询 Active Directory 属性存储，但未指定 LDAP 筛选器时，假定为 samAccountName\= {0}查询，而 Active Directory 属性存储需要一个可将值{0}馈送到的参数. 否则，查询会导致错误。 对于 Active Directory 之外的 LDAP 属性存储，不能省略查询的 LDAP 筛选器部分，否则查询会导致错误。  
  
-   属性规范—在查询的此第二部分中，如果您使用\(的多个\-属性值\)都需要使用筛选对象，则可以指定以逗号分隔的属性。 指定的属性数必须与在查询中定义的声明类型数匹配。  
  
-   Active Directory 域 — 仅当属性存储为 Active Directory 时才指定查询的最后一部分。 \(查询其他属性存储时不需要此方法。\)查询的此部分用来用域名\\称指定用户帐户。 Active Directory 属性存储使用域部分来确定要连接到的相应域控制器并运行查询和请求属性。  
  
### <a name="example-how-to-use-two-custom-rules-to-extract-the-manager-e-mail-from-an-attribute-in-activedirectory"></a>例如：如何使用两个自定义规则从 Active Directory 中的\-属性提取经理电子邮件  
以下两个自定义规则在按如下所示的顺序一起使用时，查询 Active Directory用户帐户\(规则 1\)的 manager 属性，然后使用该属性查询管理器的用户帐户**mail**特性\(规则 2\)。 最后，**mail** 属性作为“ManagerEmail”声明发出。 总之，规则1查询 Active Directory 并将查询的结果传递给规则2，后者随后提取经理电子\-邮件值。  
  
例如，当这些规则完成运行时，将发出一个声明，其中包含 corp.fabrikam.com 域中\-的用户的经理电子邮件地址。  
  
**规则1**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
=> add(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"), query = "sAMAccountName=  
{0};mail,userPrincipalName,extensionAttribute5,manager,department,extensionAttribute2,cn;{1}", param = regexreplace(c.Value, "(?  
<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
**规则2**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
&& c1:[Type == "http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerEmail"), query = "distinguishedName={0};mail;{1}", param = c1.Value,   
param = regexreplace(c1.Value, ".*DC=(?<domain>.+),DC=corp,DC=fabrikam,DC=com", "${domain}\username"));  
```  
  
> [!NOTE]  
> 仅当用户的经理与此示例\(\)中的用户在同一个域中 corp.fabrikam.com 时，这些规则才有效。  
  
## <a name="additional-references"></a>其他参考  
[创建规则以将 LDAP 属性作为声明发送](https://technet.microsoft.com/library/dd807115.aspx)  
  

