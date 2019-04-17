---
ms.assetid: 20d183f0-ef94-44bb-9dfc-ed93799dd1a6
title: "何时使用自定义声明规则"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f5398f0b10d9e62548145fdde0354a3d047eb0ae
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

# <a name="when-to-use-a-custom-claim-rule"></a>何时使用自定义声明规则
你在使用索赔规则语言，即框架索赔颁发引擎用来可编程生成转换，通过 Active Directory 联合身份验证服务 \(AD FS\) 编写自定义声明规则和筛选索赔。 通过使用自定义规则，你可以创建规则具有比标准规则模板更复杂的逻辑。 请考虑使用自定义规则根据需要：  
  
-   发送基于提取结构化查询语言 \(SQL\) 特性官方商城的值的索赔。  
  
-   发送根据从轻型目录访问协议 \(LDAP\) 特性提取的值的索赔存储使用自定义 LDAP 筛选器。  
  
-   发送基于提取自定义特性官方商城的值的索赔。  
  
-   仅当有两个或多个传入的索赔发送索赔。  
  
-   仅当传入声称值匹配复杂模式发送索赔。  
  
-   发送到传入复杂的变化索赔声称值。  
  
-   仅在以后规则，创建使用提起的索赔，实际上并不发送的索赔。  
  
-   构建传出声明从多个传入的声明的内容。  
  
你还可以使用自定义规则时传出声明的索赔值必须基于值为传入的索赔，但必须也包括其他内容。  
  
索赔规则语言是基于规则。 它具有条件部分和执行部分。 你可以使用索赔规则语言语法枚举、添加、删除或修改以满足你的组织的索赔。 有关每个反馈如何部件工作原理的详细信息，请参阅[索赔规则语言的角色](The-Role-of-the-Claim-Rule-Language.md)。  
  
下面提供了基本简介声称规则。 它们还提供了有关何时使用自定义声明规则的详细信息。  
  
## <a name="about-claim-rules"></a>有关索赔规则  
索赔规则表示采用传入的声明的业务逻辑实例、到该应用条件 \（如果 x，然后 y\）和产生传出声明条件参数而异。  
  
> [!IMPORTANT]  
> -   在广告 FS snap\ 中管理，声称可以只使用索赔规则模板创建规则  
> -   索赔规则进程传入索赔直接从索赔提供商 \（如 Active Directory 或其他联盟 Service\）或的输出中验收转换上索赔提供商信任规则。  
> -   索赔规则处理给定的规则组内的时间顺序索赔颁发引擎。 通过在规则设置优先级，可以进一步优化或筛选将生成一给定的规则组中的以前规则的索赔。  
> -   索赔规则模板始终需要你指定传入的索赔类型。 但是，可以使用单个规则具有相同的索赔类型处理多个索赔值。  
  
有关详细信息索赔规则和索赔规则集，请参阅[声称规则的角色](The-Role-of-Claim-Rules.md)。 有关如何处理规则的详细信息，请参阅[的索赔引擎角色](The-Role-of-the-Claims-Engine.md)。 如何处理声明规则集的详细信息，请参阅[的索赔管道角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="how-to-create-this-rule"></a>如何创建此规则  
您可以通过第一个创作语法，你需要使用的索赔规则语言你操作然后发送中提供了在文本框中粘贴结果索赔使用自定义规则属性索赔提供的模板信任或依赖方信任 snap\ 中广告 FS 管理在创建此规则。  
  
此规则模板提供以下选项：  
  
-   指定声明规则名称  
  
-   然后键入一个或多个可选条件和使用广告 FS 颁发声明声称规则语言  
  
用于创建自定义规则使用此模板相关说明进行操作，请参阅[创建规则应用于发送索赔使用自定义规则](https://technet.microsoft.com/library/dd807049.aspx)广告 FS 部署指南中。  
  
对于更好地了解索赔规则语言的工作方式，查看已存在其他规则索赔规则语言语法中 snap\ 方法是依次单击**视图规则语言**该规则属性选项卡中的。 使用此选项卡上的此部分中的信息和语法信息可以提供深入地了解如何创建你自己的自定义规则。  
  
有关如何使用索赔规则语言的详细信息，请参阅[声称规则语言的角色](The-Role-of-the-Claim-Rule-Language.md)。  
  
## <a name="using-the-claim-rule-language"></a>使用索赔规则语言  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>示例：如何结合根据用户的名称属性值的姓氏和名字  
以下规则语法结合姓氏和名字从属性给定的特性应用商店中的值。 此策略引擎形成笛卡尔积的每个条件匹配。 例如，{"晓辉"、"阿伦"} 的名字和姓氏和名字 {"婉芬"、"晓皓"} 输出为 {为"Frank 婉芬"、"Frank Shen"、"阿伦婉芬"、"晓兰"}:  
  
```  
c1:[type == "http://exampleschema/firstname" ]  
&&  c2:[type == "http://exampleschema/lastname",]   
=> issue(type = "http://exampleschema/name", value = c1.value + “  “ + c2.value);  
```  
  
### <a name="example-how-to-issue-a-manager-claim-based-on-whether-users-have-direct-reports"></a>示例：如何发出管理器索赔基于用户是否有直接报告  
以下规则发出管理器索赔，仅当用户可以直接报告：  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => add(store = "SQL Store", types = ("http://schemas.xmlsoap.org/claims/Reports"), query = "SELECT Reports FROM dbo.DirectReports WHERE UserName = {0}", param = c.value );  
count([type == “http://schemas.xmlsoap.org/claims/Reports“] ) > 0 => issue(= "http://schemas.xmlsoap.org/claims/ismanager", value = "true");  
```  
  
### <a name="example-how-to-issue-a-ppid-claim-based-on-an-ldap-attribute"></a>示例：如何处理 PPID 索赔基于 LDAP 属性  
以下规则问题专用个人标识符 \(PPID\) 索赔根据**windowsaccountname**和**originalissuer**属性 LDAP 特性应用商店中的用户：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
可用来识别用户输入该查询的通用属性如下所示：  
  
-   **用户 SID**  
  
-   **windowsaccountname**  
  
-   **samaccountname**  
  

