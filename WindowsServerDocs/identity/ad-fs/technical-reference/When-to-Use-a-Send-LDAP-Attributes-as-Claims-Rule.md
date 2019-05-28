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
ms.openlocfilehash: 5af00db05c572a45811eea49b832a054a9e0e492
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188248"
---
# <a name="when-to-use-a-send-ldap-attributes-as-claims-rule"></a>何时使用“以声明方式发送 LDAP 属性”规则
可以在 Active Directory 联合身份验证服务中使用此规则\(AD FS\)当你想要发出包含实际轻型目录访问协议的传出声明\(LDAP\)中存在的属性值属性存储，并且将与每个 LDAP 属性的声明类型。 有关属性存储的详细信息，请参阅[The Role of Attribute Stores](The-Role-of-Attribute-Stores.md)。  
  
使用此规则时，会为与规则逻辑匹配的每个指定的 LDAP 属性发出声明，如下表中所述。  
  
|规则选项|规则逻辑|  
|---------------|--------------|  
|LDAP 属性到传出声明类型的映射|如果属性存储等于“指定属性存储”  ，而 LDAP 属性等于“指定值”  ，则将 LDAP 属性值映射到“指定传出声明”  类型并发出声明。|  
  
以下部分提供声明规则的基本简介。 它们还提供有关何时将发送 LDAP 属性用作声明规则的详细信息。  
  
## <a name="about-claim-rules"></a>关于声明规则  
声明规则表示业务逻辑，使用传入声明、 向它应用条件的实例\(if x，then y\)和生成传出声明基于条件参数。 下面的列表概述了在进一步阅读本主题中的内容之前应了解的有关声明规则的重要提示：  
  
-   在 AD FS 管理管理单元\-中，声明只可以使用声明规则模板创建规则  
  
-   声明规则过程传入声明既可以直接从声明提供程序\(Active Directory 或另一个联合身份验证服务等\)或输出中的接受转换规则声明提供方信任上的。  
  
-   声明规则由声明颁发引擎按给定规则集内的时间顺序处理。 通过为规则设置优先级，可以进一步优化或筛选由给定规则集内以前的规则生成的声明。  
  
-   声明规则模板始终要求你指定传入声明类型。 但是，你可以使用单个规则处理声明类型相同的多个声明值。  
  
有关详细信息有关声明规则和声明规则集的详细信息，请参阅[规则的角色声明](The-Role-of-Claim-Rules.md)。 有关如何处理规则的详细信息，请参阅[The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。 有关如何处理声明规则集的详细信息，请参阅[声明管道的角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="mapping-of-ldap-attributes-to-outgoing-claim-types"></a>LDAP 属性到传出声明类型的映射  
当您使用发送 LDAP 属性作为声明规则模板时，可以选择从 LDAP 属性存储，如 Active Directory 或 Active Directory 域服务的属性\(AD DS\)以向信赖方的声明的形式发送它们的值. 这实质上是将特定 LDAP 属性从定义的属性存储映射到一组可用于授权的传出声明。  
  
通过使用此模板，可以从一个规则添加多个属性（将作为多个声明发送）。 例如，可以使用此规则模板创建一个规则，该规则会从 **company** 和 **department** Active Directory 属性查找经过身份验证的用户的属性值，然后将这些值作为两个不同的传出声明发送。  
  
还可以使用此规则发送所有用户的组成员身份。 如果要仅发送单个组成员身份，请使用“以声明方式发送组成员身份”规则模板。 有关详细信息，请参阅 [When to Use a Send Group Membership as a Claim Rule](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)。  
  
## <a name="how-to-create-this-rule"></a>如何创建此规则  
可以通过使用声明规则语言创建此规则，也可以通过使用发送 LDAP 属性作为声明规则模板位于 AD FS 管理管理单元\-中。 此规则模板提供以下配置选项：  
  
-   指定声明规则名称  
  
-   选择要从中提取 LDAP 属性的属性存储  
  
-   LDAP 属性到传出声明类型的映射  
  
有关如何创建此规则的详细信息，请参阅[创建规则以声明方式发送 LDAP 属性](https://technet.microsoft.com/library/dd807115.aspx)。  
  
## <a name="using-the-claim-rule-language"></a>使用声明规则语言  
如果将查询 Active Directory、 AD DS 或 Active Directory 轻型目录服务与\(AD LDS\)必须进行比较的 LDAP 属性以外**samAccountname**，必须改为使用自定义规则。 如果输入集中没有 Windows 帐户名称声明，则还必须使用自定义规则指定要用于查询 AD DS 或 AD LDS 的声明。  
  
以下示例旨在帮助你了解一些可以用于使用声明规则语言构造自定义规则以便查询和提取属性存储中的数据的各种方法。  
  
### <a name="example-how-to-query-an-adlds-attribute-store-and-return-a-specified-value"></a>例如：如何查询 AD LDS 属性存储并返回指定值  
参数必须用分号分隔。 第一个参数是 LDAP 筛选器。 后续参数是要对任何匹配对象返回的属性。  
  
下面的示例演示如何查找按用户**sAMAccountName**属性，并发出电子\-邮件地址声明，使用用户的邮件属性的值：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"));  
  
```  
  
以下示例演示如何按 **mail** 属性查找用户，并使用用户的 **title** 和 **displayname** 属性值发出职务和显示名称声明：  
  
```  
c:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress ", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "mail={0};title;displayname", param = c.Value);  
```  
  
以下示例演示如何按 mail 和 title 查找用户，并使用用户的 **displayname** 属性值发出显示名称声明：  
  
```  
c1:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"] && c2:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "(&(mail={0})(title={1}));displayname", param = c1.Value, param = c2.Value);  
```  
  
### <a name="example-how-to-query-an-activedirectory-attribute-store-and-return-a-specified-value"></a>例如：如何查询 Active Directory 属性存储并返回指定值  
Active Directory 查询必须包括用户的名称\(的域名\)为最后一个参数，以便 Active Directory 属性存储可以会查询正确的域。 另外，支持相同的语法。  
  
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
  
-   LDAP 筛选器 — 可指定查询的此部分以检索要对其查询属性的对象。 有关有效 LDAP 查询的常规信息，请参阅 RFC 2254。 当查询 Active Directory 属性存储并且未指定 LDAP 筛选器，samAccountName\= {0}假定查询和 Active Directory 属性存储需要一个参数，它可以将的值为{0}. 否则，查询会导致错误。 对于 Active Directory 之外的 LDAP 属性存储，不能省略查询的 LDAP 筛选器部分，否则查询会导致错误。  
  
-   属性规范 — 在此第二个查询的一部分，您指定的属性\(是逗号\-分隔如果使用多个属性值\)希望从已筛选的对象。 指定的属性数必须与在查询中定义的声明类型数匹配。  
  
-   Active Directory 域 — 仅当属性存储为 Active Directory 时才指定查询的最后一部分。 \(它不需要查询其他属性存储时。\)此查询的一部分用于在窗体域中指定的用户帐户\\名称。 Active Directory 属性存储使用域部分来确定要连接到的相应域控制器并运行查询和请求属性。  
  
### <a name="example-how-to-use-two-custom-rules-to-extract-the-manager-e-mail-from-an-attribute-in-activedirectory"></a>例如：如何使用两个自定义规则来提取经理电子\-邮件从 Active Directory 中的属性  
以下两个自定义规则，如下所示顺序一起使用时查询适用于 Active Directory**管理器**的用户帐户属性\(规则 1\)然后使用该属性来查询用户用于管理器帐户**邮件**特性\(规则 2\)。 最后，**mail** 属性作为“ManagerEmail”声明发出。 总之，规则 1 查询 Active Directory，并将查询的结果传递给规则 2，后者随后提取经理电子\-邮件值。  
  
例如，在这些规则完成运行后，会发出一个声明，其中包含管理器的 e\-邮件 corp.fabrikam.com 域中的用户的地址。  
  
**规则 1**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
=> add(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"), query = "sAMAccountName=  
{0};mail,userPrincipalName,extensionAttribute5,manager,department,extensionAttribute2,cn;{1}", param = regexreplace(c.Value, "(?  
<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
**规则 2**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
&& c1:[Type == "http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerEmail"), query = "distinguishedName={0};mail;{1}", param = c1.Value,   
param = regexreplace(c1.Value, ".*DC=(?<domain>.+),DC=corp,DC=fabrikam,DC=com", "${domain}\username"));  
```  
  
> [!NOTE]  
> 这些规则，仅当用户的经理是在用户所在的域中工作\(在此示例中的 corp.fabrikam.com\)。  
  
## <a name="additional-references"></a>其他参考  
[创建规则以声明方式发送 LDAP 属性](https://technet.microsoft.com/library/dd807115.aspx)  
  

