---
title: 声明规则语言的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: dda9d148-d72f-4bff-aa2a-f2249fa47e4c
ms.technology: identity-adfs
ms.openlocfilehash: 05728f04f6fb924cf3793bc843df3832c7c383f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855688"
---
>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

# <a name="the-role-of-the-claim-rule-language"></a>声明规则语言的角色
Active Directory 联合身份验证服务 (AD FS) 声明规则语言充当传入和传出声明，而声明引擎充当声明规则语言中的逻辑的处理引擎的行为管理构建基块，定义自定义规则。 详细了解如何处理所有规则由声明引擎，请参阅[The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。  
  
## <a name="creating-custom-claim-rules-using-the-claim-rule-language"></a>使用声明规则语言创建自定义声明规则  
AD FS 为管理员提供的选项，定义可用于确定标识声明与声明规则语言的行为的自定义规则。 可以使用本主题中的声明规则语言语法示例创建一个自定义规则，用于枚举、添加、删除和修改声明以满足组织的需求。 可以通过在“使用自定义声明发送声明”规则模板中输入声明规则语言语法，来构建自定义规则。  
  
规则使用分号相互分隔。  
  
有关何时使用自定义规则的详细信息，请参阅[何时使用自定义声明规则](When-to-Use-a-Custom-Claim-Rule.md)。  
  
## <a name="using-claim-rule-templates-to-learn-about-the-claim-rule-language-syntax"></a>使用声明规则模板了解声明规则语言语法  
AD FS 还提供了一组预定义的声明发出和声明接受规则模板，可用于实现常见声明规则。 在给定信任的“编辑声明规则”对话框中，可以创建预定义规则，并通过单击该规则的“查看规则语言”选项卡来查看组成该规则的声明规则语言语法。 使用本部分中的信息和“查看规则语言”方法可以深入了解如何构造自己的自定义规则。  
  
有关详细信息有关声明规则和声明规则模板的详细信息，请参阅[规则的角色声明](The-Role-of-Claim-Rules.md)。  
  
## <a name="understanding-the-components-of-the-claim-rule-language"></a>了解声明规则语言的组件  
声明规则语言包括以下组件分隔"= >"运算符：  
  
-   条件  
  
-   发出语句  
  
### <a name="conditions"></a>条件  
可以在规则中使用条件来检查输入声明并确定是否应执行规则的发出语句。 条件表示必须计算为 true 才能执行规则主题部分的逻辑表达式。 如果缺少此部分，则采用逻辑 true；也就是说，始终执行规则主体。 条件部分包含一系列条件与连接逻辑运算符 ("& &")。 列表中的所有条件都必须计算为 true，整个条件部分才会计算为 true。 条件可以是声明选择运算符或聚合函数调用。 这两项是互斥的，这意味着声明选择器和聚合函数不能在单个规则条件部分中组合使用。  
  
条件在规则中是可选的。 例如，以下规则没有条件：  
  
```  
=> issue(type = "http://test/role", value = "employee");  
```  
  
有三种类型的条件：  
  
-   单条件 — 这是最简单的条件形式。 只有一个表达式，则进行检查例如，windows account name = 域用户。  
  
-   多条件 — 此条件需要额外检查以处理规则主体; 中的多个表达式例如，windows account name = 域用户和组 = contosopurchasers。  
  
> [!NOTE]  
> 有其他条件存在，但它是单条件或多条件的子集。 它被称为正则表达式 (Regex) 条件。 它用于采用输入表达式并将表达式与给定模式匹配。 有关如何使用它的示例如下所示。  
  
以下示例演示几个语法构造，它们基于条件类型，可用于创建自定义规则。  
  
#### <a name="single--condition-examples"></a>单一条件示例  
单个-在下表介绍了表达式条件。 构造它们是为了仅检查具有指定声明类型的声明或具有指定声明类型和声明值的声明。  
  
|条件说明|条件语法示例|  
|-------------------------|----------------------------|  
|此规则具有一个条件来检查具有指定的声明类型的输入声明 ("http://test/name")。 如果匹配声明位于输入声明中，则规则会将匹配声明复制到输出声明集。|``` c: [type == "http://test/name"] => issue(claim = c );```|  
|此规则具有一个条件来检查具有指定的声明类型的输入声明 ("http://test/name") 和声明值 ("Terry")。 如果匹配声明位于输入声明中，则规则会将匹配声明复制到输出声明集。|``` c: [type == "http://test/name", value == "Terry"] => issue(claim = c);```|  
  
更多的复杂的条件显示在下一部分中，包括用于检查多个声明、 条件来检查声明的颁发者和条件以检查与正则表达式模式匹配的值。  
  
#### <a name="multiple--condition-examples"></a>多个-条件示例  
下表提供了一个示例的多个的表达式条件。  
  
|条件说明|条件语法示例|  
|-------------------------|----------------------------|  
|此规则具有一个条件，用于检查两个输入声明，每个都具有指定的声明类型 ("http://test/name" 和 "http://test/email")。 如果两个匹配声明位于输入声明中，则规则会将名称声明复制到输出声明集。|``` c1: [type  == "http://test/name"] && c2: [type == "http://test/email"] => issue (claim  = c1 );```|  
  
#### <a name="regular--condition-examples"></a>正则-条件示例  
下表提供了正则，表达式的一个示例-基于条件。  
  
|条件说明|条件语法示例|  
|-------------------------|----------------------------|  
|此规则有一个条件，使用正则表达式检查是否有电子-邮件声明以结尾"@fabrikam.com"。 如果在输入声明中找到匹配声明，则规则会将匹配声明复制到输出声明集。|```c: [type  == "http://test/email", value  =~ "^. +@fabrikam.com$" ] => issue (claim  = c );```|  
  
### <a name="issuance-statements"></a>发出语句  
自定义规则会根据发出语句处理 (*问题*或*添加*) 你编程到声明规则。 根据所需结果，可以将发出语句或添加语句编写到规则中以填充输入声明集或输出声明集。 使用添加语句的自定义规则仅将声明值填充到输入声明集中，而使用发出语句的自定义声明规则会将声明值同时填充到输入声明集和输出声明集中。 当声明值旨在仅供声明规则集中的将来规则使用时，这可能十分有用。  
  
例如，在下图中，传入声明由声明发出引擎添加到输入声明集。 当第一个自定义声明规则执行并满足条件的域用户，声明发出引擎处理中使用添加语句和的值的规则的逻辑**编辑器**添加到输入的声明集。 由于值 Editor 存在于输入声明集中，因此规则 2 可以成功处理其逻辑中的发出语句并生成新值“Hello”，该值会同时添加到输出声明集和输入声明集，以便供规则集中的下一个规则使用。 规则 3 现在可以使用输入声明集中存在的所有值作为输入来处理其逻辑。  
  
![AD FS 角色](media/adfs2_customrule.gif)  
  
#### <a name="claim-issuance-actions"></a>声明发出操作  
规则主体表示声明发出操作。 语言可识别两种声明发出操作：  
  
-   **发出语句：** 发出语句创建同时进入输入和输出声明集的声明。 例如，下面的语句基于其输入声明集发出新声明：  
  
    ```c:[type == "Name"] => issue(type = "Greeting", value = "Hello " + c.value);```  
  
-   **添加语句：** 添加语句创建仅添加到输入声明集集合的新声明。 例如，下面的语句向输入声明集添加新声明：  
  
    ```c:[type == "Name", value == "domain user"] => add(type = "Role", value = "Editor");``` 
  
规则的发出语句定义在匹配条件时由规则发出的声明。 在参数和语句行为方面有两种形式的发出语句：  
  
-   **正常** — 正常发出语句可以通过使用规则中的文本值或来自与条件匹配的声明的值来发出声明。 正常发出语句可以包含一个或两个以下格式：  
  
    -   *声明副本*：声明副本会在输出声明集中创建现有声明的副本。 仅当与“issue”发出语句结合使用时，此发出形式才有意义。 当与“add”发出语句结合使用时，它没有任何效果。  
  
    -   *新声明*：此格式创建的新声明，给定各个声明属性的值。 必须指定 Claim.Type；所有其他声明属性都是可选的。 会忽略此形式的参数顺序。  
  
-   **属性存储** — 此形式会使用从属性存储检索的值创建声明。 它是可以通过使用单个发出语句，这非常重要的属性检索过程中使网络或磁盘输入/输出 (I/O) 操作的属性存储中创建多个声明类型。 因此，最好限制策略引擎与属性存储之间的往返次数。 为一种给定声明类型创建多个声明也是合法的。 当属性存储为一种给定声明类型返回多个值时，发出语句会为每个返回的声明值自动创建声明。 属性存储实现使用 param 参数将 query 参数中的占位符替换为 param 参数中提供的值。 将占位符与.NET String.Format （） 函数使用相同的语法 (例如， {1}， {2}，依此类推)。 这种发出形式的参数顺序十分重要，必须是采用以下语法规定的顺序。  
  
下表介绍了声明规则中两种发出语句类型的一些常见语法构造。  
  
|发出语句类型|发出语句说明|发出语句语法示例|  
|---------------------------|----------------------------------|-------------------------------------|  
|正常|只要用户具有指定声明类型和值，以下规则便始终发出相同声明：|```c: [type  == "http://test/employee", value  == "true"] => issue (type = "http://test/role", value = "employee");```|  
|正常|以下规则将一种声明类型转换为另一种。 请注意，与条件“c”相匹配的声明的值在发出语句中使用。|```c: [type  == "http://test/group" ] => issue (type  = "http://test/role", value  = c.Value );```|  
|属性存储|以下规则使用传入声明的值查询 Active Directory 属性存储：|```c: [Type  == "http://test/name" ] => issue (store  = "Enterprise AD Attribute Store", types  =  ("http://test/email" ), query  = ";mail;{0}", param  = c.Value )```|  
|属性存储|以下规则使用传入声明的值查询以前配置的结构化查询语言 (SQL) 属性存储：|```c: [type  == "http://test/name"] => issue (store  = "Custom SQL store", types  =  ("http://test/email","http://test/displayname" ), query  = "SELECT mail, displayname FROM users WHERE name ={0}", param  = c.value );```|  
  
#### <a name="expressions"></a>表达式  
表达式在右侧用于声明选择器约束和发出语句参数。 语言支持各种类型的表达式。 语言中的所有表达式都基于字符串，这意味着它们将字符串作为输入并生成字符串。 表达式中不支持数字或其他数据类型（如日期/时间）。 以下是语言支持的表达式类型：  
  
-   字符串：字符串值，两端使用引号 （"） 字符分隔。  
  
-   表达式的字符串串联：结果是由左侧和右侧值串联生成的字符串。  
  
-   函数调用：函数通过标识符进行标识和逗号作为传递的参数的表达式用方括号括起来的分隔的列表 ("（)")。  
  
-   采用“变量名.属性名”形式进行的声明属性访问：针对给定变量计算的标识声明属性的值的结果。 变量必须首先绑定到声明选择器，然后才能采用这种方式进行使用。 在声明选择器的约束内使用绑定到该相同声明选择器的变量是非法的。  
  
以下是可用于访问的声明属性：  
  
-   Claim.Type  
  
-   Claim.Value  
  
-   Claim.Issuer  
  
-   Claim.OriginalIssuer  
  
-   Claim.ValueType  
  
-   Claim.Properties\[属性\_名称\]（如果声明的属性集合中找不到属性名称 （_n），此属性都返回空字符串。 )  
  
可以使用 RegexReplace 函数在表达式中进行调用。 此函数采用输入表达式并将它与给定模式进行匹配。 如果模式匹配，则匹配项的输出会替换为替换值。  
  
#### <a name="exists-functions"></a>Exists 函数  
Exists 函数可以在条件中用于评估输入声明集中是否存在与条件匹配的声明。 如果存在任何匹配声明，则发出语句只调用一次。 在下面的示例中，“origin”声明恰好发出一次 — 如果输入声明集集合中至少有一个声明的发出者设置为“MSFT”（无论有多少声明的发出者设置为“MSFT”）。 使用此函数可防止发出重复声明。  
  
```  
exists([issuer == "MSFT"])  
   => issue(type = "origin", value = "Microsoft");  
```  
  
## <a name="rule-body"></a>规则主体  
规则主体只能包含单个发出语句。 如果在未使用 Exists 函数的情况下使用条件，则每当与条件部分匹配时，都会执行一次规则主体。  
  
## <a name="additional-references"></a>其他参考  
[创建规则，以使用自定义规则发送声明](https://technet.microsoft.com/library/dd807049.aspx)  
  

