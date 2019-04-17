---
title: "角色的索赔规则语言"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: dda9d148-d72f-4bff-aa2a-f2249fa47e4c
ms.technology: identity-adfs
ms.openlocfilehash: 206da33d33cbded0450db29615dc74eb1161cbce
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

# <a name="the-role-of-the-claim-rule-language"></a>角色的索赔规则语言
Active Directory 联合身份验证服务 (广告 FS) 索赔规则语言作为管理构建基块传入和传出索赔行为时索赔引擎充当定义自定义规则索赔规则语言逻辑处理引擎。 有关如何将所有规则都处理索赔引擎，请参阅[的索赔引擎角色](The-Role-of-the-Claims-Engine.md)。  
  
## <a name="creating-custom-claim-rules-using-the-claim-rule-language"></a>创建自定义声明规则使用索赔规则语言  
广告 FS 提供管理员，可以选择定义自定义规则，它们可使用确定与索赔规则语言标识声明的行为。 使用本主题中的索赔规则语言语法示例，以创建自定义规则枚举，添加、删除，并修改以满足你的组织的索赔。 可通过键入索赔规则语言语法中生成的自定义规则**发送索赔使用自定义的声称**规则模板。  
  
使用分号互相分离规则。  
  
有关何时使用自定义规则的详细信息，请参阅[何时使用自定义的索赔规则](When-to-Use-a-Custom-Claim-Rule.md)。  
  
## <a name="using-claim-rule-templates-to-learn-about-the-claim-rule-language-syntax"></a>使用索赔规则模板若要了解有关索赔规则语言语法  
广告 FS 还提供了一套预定义的索赔颁发和可用于执行常见的验收规则模板声称规则的索赔。 在**编辑声称规则**对话框对于给定信任，你可以创建一个预定义的规则，并查看该规则占据索赔规则语言语法，方法是依次单击**视图规则语言**该规则的选项卡。 在此部分中使用的信息和**视图规则语言**技术可提供深入地了解如何创建你自己的自定义规则。  
  
有关详细信息索赔规则和索赔规则模板，请参阅[声称规则的角色](The-Role-of-Claim-Rules.md)。  
  
## <a name="understanding-the-components-of-the-claim-rule-language"></a>了解索赔规则语言组件  
索赔规则语言包含以下组件的分隔"= >"运营商：  
  
-   条件  
  
-   颁发声明  
  
### <a name="conditions"></a>条件  
使用规则中的条件，检查输入的索赔，并确定是否应执行的规则颁发声明。 条件表示必须评估逻辑 expression 为真执行规则正文部分。 这就是丢失，如果逻辑真假定;也就是说，始终执行规则正文。 条件一部分列出的组合在一起逻辑运营商和条件 ("& &")。 在列表中的所有条件必须都评价到适用于整个条件部分受到评估为 true。 条件可以是索赔所选内容运营商或聚合的函数调用。 这两个是相互独家，这意味着，声称器，并且不能在单个规则条件一部分一起使用聚合的功能。  
  
条件是可选规则中。 例如，以下规则没有条件：  
  
```  
=> issue(type = "http://test/role", value = "employee");  
```  
  
有三种类型的条件：  
  
-   单条件，这是最简单的条件。 进行检查，只有一个 expression; 对于例如，windows 的帐户名称 = 用户域。  
  
-   多个情况 — 这种情况需要处理多个表情规则正文; 中的其他检查例如，windows 的帐户名称 = 域用户和组 = contosopurchasers。  
  
> [!NOTE]  
> 另一个条件存在，但它是一个条件或多个状况的一部分。 这称为常规 expression (Regex) 条件。 用于使用给定模式 expression 需要输入的 expression 搭配使用。 可以使用它的方式的一个示例如下所示。  
  
下面的示例显示了几个语法构造，它基于条件类型，可用于创建自定义规则。  
  
#### <a name="single--condition-examples"></a>一个-条件示例  
一个-expression 条件将在下表中所述。 它们被构建声称值，只需查明与指定的索赔类型索赔或者指定的索赔类型的索赔。  
  
|条件说明|条件语法示例|  
|-------------------------|----------------------------|  
|此规则具有条件，以检查输入声明与指定的索赔类型 ("http://test/name")。 如果输入的索赔匹配的索赔，规则复制匹配的索赔或输出索赔组的索赔。|``` c: [type == "http://test/name"] => issue(claim = c );```|  
|此规则具有的情况，若要检查输入声明与指定的索赔类型 ("http://test/name") 和声称值 ("Terry")。 如果输入的索赔匹配的索赔，规则复制匹配的索赔或输出索赔组的索赔。|``` c: [type == "http://test/name", value == "Terry"] => issue(claim = c);```|  
  
更-复杂的条件将会显示在下一步部分中，包括条件查看多个索赔、条件，若要检查的索赔，发行商和条件以检查有常规 expression 模式相匹配的值。  
  
#### <a name="multiple--condition-examples"></a>多个-条件示例  
下表提供一个示例的多个-expression 条件。  
  
|条件说明|条件语法示例|  
|-------------------------|----------------------------|  
|此规则具有条件以检查有两种输入索赔，每个人都带指定的索赔类型（"http://test/name"和"http://test/email"）。 如果输入声明中有两种匹配的索赔，规则复制到输出索赔集名称索赔。|``` c1: [type  == "http://test/name"] && c2: [type == "http://test/email"] => issue (claim  = c1 );```|  
  
#### <a name="regular--condition-examples"></a>常规-条件示例  
下表提供了示例，其中常规和 expression-基于条件。  
  
|条件说明|条件语法示例|  
|-------------------------|----------------------------|  
|此规则已使用常规 expression e 以检查状态-结束中的邮件索赔"@fabrikam.com"。 如果找到匹配索赔在输入的索赔，规则复制到输出索赔集匹配的索赔。|```c: [type  == "http://test/email", value  =~ "^. +@fabrikam.com$" ] => issue (claim  = c );```|  
  
### <a name="issuance-statements"></a>颁发明细表  
自定义规则处理基于颁发语句 (*问题*或*添加*) 你程序转换索赔规则。 取决于所需的结果的问题声明或添加到规则填充输入的声明设置是可写的声明或输出声称设置。 使用添加声明明确的自定义规则填充仅向输入值声称设置时使用的问题声明的自定义声明规则填充声明中的输入值声称集和输出中声称组的索赔。 当声明值旨在仅供中的一组索赔规则将来规则时，这可能很有用。  
  
例如，在下图中，传入声明添加到输入设置索赔颁发引擎的索赔。 当首自定义声明规则执行，并满足条件域用户的索赔颁发引擎处理中添加声明中，并的值的使用规则的逻辑**编辑器**添加到输入的声明设置。 因为输入的索赔集中编辑器的值，规则 2 可以成功进程在其逻辑问题声明并生成的新值**Hello**，它将添加到这两个输出声称集和设置的使用规则中的下一个规则输入声明设置。 规则 3 现在可以使用的所有值设置为用于处理其逻辑输入输入声明中所存在。  
  
![广告 FS 角色](media/adfs2_customrule.gif)  
  
#### <a name="claim-issuance-actions"></a>声称颁发操作  
规则身体表示索赔颁发操作。 有两个该语言可以识别的索赔颁发操作：  
  
-   **发出语句：**问题声明创建转至输入和输出声称集索赔。 例如，以下声明问题新的天空根据其输入的声明设置：  
  
    ```c:[type == "Name"] => issue(type = "Greeting", value = "Hello " + c.value);```  
  
-   **添加声明：**添加声明创建新的天空，仅添加到输入的声明设置集合。 例如，以下声明将添加到输入的声明设置新的天空：  
  
    ```c:[type == "Name", value == "domain user"] => add(type = "Role", value = "Editor");``` 
  
规则颁发语句定义索赔将由规则时匹配的条件。 有两种形式的颁发声明涉及参数和隐私声明行为：  
  
-   **普通**— 正常颁发明细表可使用规则中的文本值或从符合条件的索赔值发出索赔。 常规发行声明采用的一项或两以下格式：  
  
    -   *声称副本*：索赔副本输出索赔集中创建一份现有的索赔。 此颁发窗体结合"问题"颁发声明只会起作用。 当与的"添加"颁发明细表相结合时，它不会没有任何效果。  
  
    -   *新的声明*：这格式化 reates 新索赔，给定的各种声称属性。 必须指定 Claim.Type;是可选的所有其他索赔属性。 将忽略此窗体的参数的顺序。  
  
-   **特性官方商城**— 此窗体创建具有值的特性官方商城中检索索赔。 也可以使用单个颁发声明，这是非常重要的特性检索期间使网络或磁盘输入/输出（操作）的特性官方商城创建多个索赔类型。 因此，最好是限制往返之间策略引擎和属性应用商店的数。 它也是法律创建多个提起的索赔给定的索赔类型。 当特性官方商城退货给定的索赔类型的多个值时，颁发声明会自动创建为每个返回的索赔值索赔。 属性应用商店实现使用参数参数替换查询参数使用参数参数界面还设有的值中占位符。 占位符使用同一个语法作为.NET String.Format（）函数（例如，{1} {2}，以及等）。 这种形式的发行的参数订单很重要，并且必须在以下语法预定义的顺序。  
  
下表介绍了这两种类型的颁发明细表中索赔规则一些常见语法结构。  
  
|颁发声明类型|颁发声明说明|颁发声明语法示例|  
|---------------------------|----------------------------------|-------------------------------------|  
|普通|以下规则始终发出相同的索赔，只要用户，并且在指定的索赔类型值：|```c: [type  == "http://test/employee", value  == "true"] => issue (type = "http://test/role", value = "employee");```|  
|普通|以下规则转换到另一种索赔类型。 请注意，颁发声明中使用的符合条件"c"索赔值。|```c: [type  == "http://test/group" ] => issue (type  = "http://test/role", value  = c.Value );```|  
|特性官方商城|以下规则使用传入的声明的值 Active Directory 特性应用商店中查询：|```c: [Type  == "http://test/name" ] => issue (store  = "Enterprise AD Attribute Store", types  =  ("http://test/email" ), query  = ";mail;{0}", param  = c.Value )```|  
|特性官方商城|以下规则使用传入的声明的值以前配置的结构化查询语言 (SQL) 特性应用商店中查询：|```c: [type  == "http://test/name"] => issue (store  = "Custom SQL store", types  =  ("http://test/email","http://test/displayname" ), query  = "SELECT mail, displayname FROM users WHERE name ={0}", param  = c.value );```|  
  
#### <a name="expressions"></a>表情  
表情用于右侧索赔选择器约束和颁发声明参数。 有各种类型的表情语言支持。 在语言中的所有表情都是基于字符串，这意味着它们作为输入拍摄字符串，并生成字符串。 数字或其他数据类型，例如日期/时间表情中不受支持。 以下是表情语言的支持类型：  
  
-   字符串：字符串值，双方报价（"）字符的分隔。  
  
-   字符串的表情连接：结果是您可以通过连接左右值的字符串。  
  
-   函数调用：函数标识标识符，并且参数作为逗号-在方括号表情的分隔的列表 ("（)")。  
  
-   局部变量的名称点属性名称的形式的索赔的属性访问：的值的标识的索赔属性指定的变量评估结果。 变量必须先绑定到索赔选择器，可以通过这种方式使用它。 若要使用该相同的索赔选择器约束内索赔选择器绑定变量无效。  
  
访问以下索赔属性有：  
  
-   Claim.Type  
  
-   Claim.Value  
  
-   Claim.Issuer  
  
-   Claim.OriginalIssuer  
  
-   Claim.ValueType  
  
-   Claim.Properties\[property\_name\]（如果索赔属性集锦中找不到属性 _name 此属性返回空的字符串。 )  
  
可以使用 RegexReplace 函数调用 expression 内。 此函数输入的 expression，符合给定的模式。 如果模式匹配，则匹配的输出替换更换值。  
  
#### <a name="exists-functions"></a>存在功能  
存在功能可以在一个条件，用于评估是否索赔匹配中输入存在条件声称设置。 如果存在任何匹配的索赔，颁发声明称为一次。 在以下示例中，一次发出"源"索赔，如果在至少有一个已设置为"微软公司"发行商输入的声明组集锦中的索赔，无论有多少索赔发行商设置为"微软公司"。 使用此功能将阻止重复索赔发出的。  
  
```  
exists([issuer == "MSFT"])  
   => issue(type = "origin", value = "Microsoft");  
```  
  
## <a name="rule-body"></a>规则正文  
规则身体可能包含仅颁发的单个声明。 如果不使用存在函数使用条件，规则身体被执行一次条件部分相匹配每次。  
  
## <a name="additional-references"></a>其他参考  
[创建发送索赔自定义规则的使用规则](https://technet.microsoft.com/library/dd807049.aspx)  
  

