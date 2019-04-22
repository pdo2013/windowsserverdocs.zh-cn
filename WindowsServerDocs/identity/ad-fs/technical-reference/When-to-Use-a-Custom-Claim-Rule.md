---
ms.assetid: 20d183f0-ef94-44bb-9dfc-ed93799dd1a6
title: 何时使用自定义声明规则
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b08622cd9eefa153e2fe8403fbe85077644182f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813108"
---
>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

# <a name="when-to-use-a-custom-claim-rule"></a>何时使用自定义声明规则
在 Active Directory 联合身份验证服务中编写自定义声明规则\(AD FS\)使用声明规则语言，这是用于以编程方式生成声明发出引擎的框架，转换、 传递和筛选声明。 通过使用自定义规则，可以创建逻辑比标准规则模板更复杂的规则。 在以下情况下可考虑使用自定义规则：  
  
-   发送基础结构化查询语言从提取的值的声明\(SQL\)属性存储。  
  
-   发送基于轻型目录访问协议从提取的值的声明\(LDAP\)属性存储使用自定义 LDAP 筛选器。  
  
-   发送以从自定义属性存储提取的值为基础的声明。  
  
-   仅当有两个或更多传入声明时发送声明。  
  
-   仅当传入声明值与复杂模式匹配时发送声明。  
  
-   发送对传入声明值进行了复杂更改的声明。  
  
-   创建声明仅供以后的规则使用，而不实际发送声明。  
  
-   根据多个传入声明的内容构造传出声明。  
  
还可以在传出声明的声明值必须基于传入声明的值，但是还必须包括其他内容时使用自定义规则。  
  
声明规则语言基于规则。 它包含条件部分和执行部分。 你可以使用声明规则语言语法来枚举、添加、删除或修改声明，以满足组织的需求。 有关其中每个部分的工作原理的详细信息，请参阅[的声明规则语言角色](The-Role-of-the-Claim-Rule-Language.md)。  
  
以下部分提供声明规则的基本简介。 它们还提供有关何时使用自定义声明规则的详细信息。  
  
## <a name="about-claim-rules"></a>关于声明规则  
声明规则表示接受传入声明的业务逻辑的实例，向它应用条件\(if x，y\)和生成传出声明基于条件参数。  
  
> [!IMPORTANT]  
> -   在 AD FS 管理管理单元\-中，声明可以仅使用声明规则模板创建规则  
> -   声明规则过程传入声明既可以直接从声明提供程序\(Active Directory 或另一个联合身份验证服务等\)或输出中的接受转换规则声明提供方信任上的。  
> -   声明规则由声明颁发引擎按给定规则集内的时间顺序处理。 通过为规则设置优先级，可以进一步优化或筛选由给定规则集内以前的规则生成的声明。  
> -   声明规则模板始终要求你指定传入声明类型。 但是，你可以使用单个规则处理声明类型相同的多个声明值。  
  
有关详细信息有关声明规则和声明规则集的详细信息，请参阅[规则的角色声明](The-Role-of-Claim-Rules.md)。 有关如何处理规则的详细信息，请参阅[The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。 有关如何处理声明规则集的详细信息，请参阅[声明管道的角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="how-to-create-this-rule"></a>如何创建此规则  
您创建此规则通过第一个创作的语法所需的操作使用声明规则语言，然后粘贴到文本结果框，它是提供在发送声明使用自定义规则模板的属性的声明提供 trust 或信赖方信任 AD FS 管理管理单元\-中。  
  
此规则模板提供下列选项：  
  
-   指定声明规则名称  
  
-   键入一个或多个可选条件和发布声明使用 AD FS 声明规则语言  
  
用于创建使用此模板的自定义规则的详细说明，请参阅[创建自定义规则的规则发送声明使用](https://technet.microsoft.com/library/dd807049.aspx)AD FS 部署指南中。  
  
为了更好地了解声明规则语言的工作原理，在管理单元中查看已存在其他规则的声明规则语言语法\-中通过单击**查看规则语言**选项卡中为该规则的属性。 使用此部分中的信息和此选项卡上的语法信息可以深入了解如何构造你自己的自定义规则。  
  
有关如何使用声明规则语言的详细信息，请参阅[的声明规则语言角色](The-Role-of-the-Claim-Rule-Language.md)。  
  
## <a name="using-the-claim-rule-language"></a>使用声明规则语言  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>例如：如何基于用户的名称属性值组合名字和姓氏  
下面的规则语法根据给定属性存储中的属性值组合名字和姓氏。 策略引擎对每个条件的匹配项应用笛卡尔乘积。 例如，名字 {“Frank”, “Alan”} 和姓氏 {“Miller”, “Shen”} 的输出为 {“Frank Miller”, “Frank Shen”, “Alan Miller”, “Alan Shen”}：  
  
```  
c1:[type == "http://exampleschema/firstname" ]  
&&  c2:[type == "http://exampleschema/lastname",]   
=> issue(type = "http://exampleschema/name", value = c1.value + “  “ + c2.value);  
```  
  
### <a name="example-how-to-issue-a-manager-claim-based-on-whether-users-have-direct-reports"></a>例如：如何根据用户是否有直接下属发出管理人员声明  
以下规则仅当用户有直接下属时发出管理人员声明：  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => add(store = "SQL Store", types = ("http://schemas.xmlsoap.org/claims/Reports"), query = "SELECT Reports FROM dbo.DirectReports WHERE UserName = {0}", param = c.value );  
count([type == “http://schemas.xmlsoap.org/claims/Reports“] ) > 0 => issue(= "http://schemas.xmlsoap.org/claims/ismanager", value = "true");  
```  
  
### <a name="example-how-to-issue-a-ppid-claim-based-on-an-ldap-attribute"></a>例如：如何基于 LDAP 属性发出 PPID 声明  
以下规则发出专用个人标识符\(PPID\)声明基于**windowsaccountname**并**originalissuer**的 LDAP 属性中的用户属性存储：  
  
```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
可用于唯一标识此查询的用户的常见属性包括：  
  
-   **用户 SID**  
  
-   **windowsaccountname**  
  
-   **samaccountname**  
  

