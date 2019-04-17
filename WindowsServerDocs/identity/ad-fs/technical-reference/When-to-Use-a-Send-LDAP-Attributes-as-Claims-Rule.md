---
ms.assetid: 606f4196-b579-4806-a462-3abd4d93e87c
title: "何时使用作为索赔规则发送 LDAP 属性"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05a2dd88057b64675bbc3bd30724d1eda0880c44
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

# <a name="when-to-use-a-send-ldap-attributes-as-claims-rule"></a>何时使用作为索赔规则发送 LDAP 属性
当你想要发出传出包含实际轻型目录访问协议 \(LDAP\) 特性值，在属性应用商店中存在，然后将索赔类型关联的每个 LDAP 属性的索赔，可以使用 Active Directory 联合身份验证服务 \(AD FS\) 中的此规则。 有关特性官方商城的详细信息，请参阅[角色的特性存储](The-Role-of-Attribute-Stores.md)。  
  
当你使用此规则时，你发出有关每个 LDAP 属性索赔指定和相匹配的规则逻辑下, 表中所述。  
  
|规则选项|规则逻辑|  
|---------------|--------------|  
|映射到传出声明类型 LDAP 属性|如果特性官方商城等于*指定的特性官方商城*和 LDAP 特性等于*指定值*，然后将映射到 LDAP 特性值*指定传出声明*键入和发出索赔。|  
  
下面提供了基本简介声称规则。 它们还提供了有关何时发送 LDAP 属性用作索赔规则的详细信息。  
  
## <a name="about-claim-rules"></a>有关索赔规则  
索赔规则表示企业逻辑，将需要传入的索赔，到该应用条件实例 \（如果然后 y\ x）和产生传出声明条件参数而异。 以下列表轮廓重要阅读进一步之前，你应该提前了解的提示声称规则本主题中：  
  
-   在 snap\ 中广告 FS 管理索赔规则只能创建使用索赔规则模板  
  
-   索赔规则进程传入索赔直接从索赔提供商 \（如 Active Directory 或其他联盟 Service\）或的输出中验收转换上索赔提供商信任规则。  
  
-   索赔规则处理给定的规则组内的时间顺序索赔颁发引擎。 通过在规则设置优先级，可以进一步优化或筛选将生成一给定的规则组中的以前规则的索赔。  
  
-   索赔规则模板将始终需要你指定传入的索赔类型。 但是，你可以使用相同的索赔类型使用单个规则处理多个索赔值。  
  
有关详细信息索赔规则和索赔规则集，请参阅[声称规则的角色](The-Role-of-Claim-Rules.md)。 有关如何处理规则的详细信息，请参阅[的索赔引擎角色](The-Role-of-the-Claims-Engine.md)。 如何处理声明规则集的详细信息，请参阅[的索赔管道角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="mapping-of-ldap-attributes-to-outgoing-claim-types"></a>映射到传出声明类型 LDAP 属性  
当你使用发送 LDAP 属性为索赔规则模板时，你可以选择 LDAP 特性官方商城，如 Active Directory 或 Active Directory 域服务 \(AD DS\) 发送到依赖方索赔为其值属性。 它本质上是映射来自定义传出用于授权的索赔一组属性应用商店的特定 LDAP 属性。  
  
通过使用此模板，你可以添加多个属性，然后将其发送以多个索赔，从一个规则。 例如，你可以使用此规则模板创建属性值从授权的用户将查找规则**公司**和**部门**Active Directory 属性，然后将这些值发送作为两种不同的传出索赔。  
  
你还可以使用此规则向所有用户的组成员发送。 如果你想要发送仅个别的组成员资格，用作索赔规则模板发送组成员。 有关详细信息，请参阅[何时使用发送组成员资格作为索赔规则](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)。  
  
## <a name="how-to-create-this-rule"></a>如何创建此规则  
你可以使用索赔规则语言创建此规则，或按照索赔发送 LDAP 属性规则中 snap\ 中广告 FS 管理模板。 此规则模板提供以下配置选项：  
  
-   指定声明规则名称  
  
-   选择要从中解压缩 LDAP 属性属性应用商店  
  
-   映射到传出声明类型 LDAP 属性  
  
有关如何创建此规则的详细信息，请参阅[创建规则应用于作为索赔发送 LDAP 属性](https://technet.microsoft.com/library/dd807115.aspx)。  
  
## <a name="using-the-claim-rule-language"></a>使用索赔规则语言  
如果查询到的 Active Directory、广告 DS 或 Active Directory 轻型目录服务 \(AD LDS\) 必须以外的其他比较 LDAP 特性**samAccountname**，则必须改为使用自定义规则。 如果输入的集中没有 Windows 的帐户名称索赔，你必须使用自定义规则指定用于查询广告 DS 或广告 LDS 索赔。  
  
下面的示例包括可用于帮助您了解各种方式来构建自定义在属性应用商店中使用索赔规则语言设置为查询和提取数据规则一些。  
  
### <a name="example-how-to-query-an-ad-lds-attribute-store-and-return-a-specified-value"></a>示例：如何查询广告 LDS 特性存储和退还指定的值  
必须分号的分隔参数。 第一个参数是 LDAP 筛选器。 后续参数是属性返回任何匹配对象上。  
  
下面的示例显示了如何查找按用户**sAMAccountName**特性并发出 e\ 邮件地址的索赔，使用用户 mail 属性的价值：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"));  
  
```  
  
下面的示例显示了如何查找按用户**邮件**特性并发出标题和显示名称的索赔，使用的用户的值**标题**和**显示名称**属性：  
  
```  
c:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress ", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "mail={0};title;displayname", param = c.Value);  
```  
  
下面的示例显示了如何通过邮件和标题来查找用户，然后发出使用该用户的显示名称索赔**显示名称**特性：  
  
```  
c1:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"] && c2:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "(&(mail={0})(title={1}));displayname", param = c1.Value, param = c2.Value);  
```  
  
### <a name="example-how-to-query-an-active-directory-attribute-store-and-return-a-specified-value"></a>示例：如何查询 Active Directory 特性存储和退还指定的值  
Active Directory 查询必须含有用户姓名 \（带有域 name\) 在最后一个参数以便 Active Directory 特性存储可以进行查询正确的域。 否则，支持语法相同。  
  
下面的示例显示了如何查找按用户**sAMAccountName**属性他或她的域中，然后返回**邮件**特性：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
### <a name="example-how-to-query-an-active-directory-attribute-store-based-on-the-value-of-an-incoming-claim"></a>示例：如何查询基于传入的索赔值 Active Directory 特性官方商城  
  
```  
c:[Type == "http://test/name"]  
  
   => issue(store = "Enterprise AD Attribute Store",  
  
         types = ("http://test/email"),  
  
         query = ";mail;{0}",  
  
         param = c.Value)  
  
```  
  
以前查询由以下三种部分组成：  
  
-   LDAP 筛选器-指定查询检索你想要查询属性对象的这个部分。 有效的 LDAP 查询的一般信息，请参阅 RFC 2254。 当你正在查询 Active Directory 特性官方商城和不指定 LDAP 筛选器，samAccountName\ = {0} 假定查询，并且 Active Directory 特性官方商城期望可以源的值为 {0} 参数。 否则，查询将导致错误。 不能省略查询 LDAP 筛选部件 LDAP 特性应用商店以外的 Active Directory 或查询将导致错误。  
  
-   特性规范 — 查询该第二部分中，您指定的属性 \（这是 comma\ 的分隔如果使用多个特性 values\）你想要退出筛选对象。 你所指定的属性大量必须匹配你查询中定义的索赔类型的数。  
  
-   域的 Active Directory-你指定仅在特性官方商城的 Active Directory 查询的最后一部分。 \ (它时不需要你查询其他属性官方商城。\) 查询的这个部分用于窗体 domain\\name 中指定的用户帐户。 Active Directory 特性应用商店使用域部分来确定要连接到运行查询和请求属性相应域控制器。  
  
### <a name="example-how-to-use-two-custom-rules-to-extract-the-manager-e-mail-from-an-attribute-in-active-directory"></a>示例：如何使用两个自定义规则从属性 Active Directory 中提取管理器 e\ 邮件  
以下两个自定义规则，一起使用，下方显示的顺序时查询 Active Directory**管理器**特性的用户帐户 \(Rule 1\)，然后使用该特性查询用于管理器的用户帐户**邮件**特性 \(Rule 2\)。 最后，**邮件**因为"ManagerEmail"声称发出属性。 总之，规则 1 查询 Active Directory，并向规则 2，则会提取管理器 e\ 邮件值传递查询的结果。  
  
例如，如果这些规则完成运行，索赔颁发包含管理器的 corp.fabrikam.com 域中的用户 e\ 邮件地址。  
  
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
> 这些规则工作只有管理器中的用户是用户登录 \ (在此 example\ corp.fabrikam.com)。  
  
## <a name="additional-references"></a>其他参考  
[创建规则作为索赔发送 LDAP 属性](https://technet.microsoft.com/library/dd807115.aspx)  
  

