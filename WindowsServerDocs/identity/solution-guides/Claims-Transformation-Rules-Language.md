---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: 声明转换规则语言
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 200d592bc68562856bbdee623e70d73d41457c15
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357577"
---
# <a name="claims-transformation-rules-language"></a>声明转换规则语言

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

利用跨林声明转换功能，可以通过在跨林信任上设置声明转换策略来跨林边界桥接用于动态访问控制的声明。 所有策略的主要组件都是用声明转换规则语言编写的规则。 本主题提供有关此语言的详细信息，并提供有关创作声明转换规则的指导。  
  
跨林信任上的转换策略的 Windows PowerShell cmdlet 具有设置常见方案中所需的简单策略的选项。 这些 cmdlet 将用户输入转换为声明转换规则语言中的策略和规则，然后以规定的格式将其存储在 Active Directory 中。 有关用于声明转换的 cmdlet 的详细信息，请参阅[适用于动态访问控制的 AD DS cmdlet](https://go.microsoft.com/fwlink/?LinkId=243150)。  
  
根据对 Active Directory 林中的跨林信任的声明配置和要求，声明转换策略可能必须比用于活动的 Windows PowerShell cmdlet 支持的策略更复杂文件夹. 若要有效地创作此类策略，必须了解声明转换规则语言语法和语义。 Active Directory 中的声明转换规则语言（"语言"）是[Active Directory 联合身份验证服务](https://go.microsoft.com/fwlink/?LinkId=243982)用于类似用途的语言子集，并且它具有非常类似的语法和语义。 但允许的操作较少，并且其他语法限制会置于语言的 Active Directory 版本中。  
  
本主题简要介绍了中的声明转换规则语言的语法和语义，以及在创作策略时应考虑的注意事项 Active Directory。 它提供了几组示例规则来帮助您入门，并举例说明了如何在编写规则时破译错误消息。  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>用于创作声明转换策略的工具  
**适用于 Active Directory 的 Windows PowerShell cmdlet**：这是创作和设置声明转换策略的首选和建议的方法。 这些 cmdlet 提供简单策略的开关，并验证为更复杂的策略设置的规则。  
  
**LDAP**：可以通过轻型目录访问协议（LDAP）在 Active Directory 中编辑声明转换策略。 但是，不建议这样做，因为策略具有几个复杂的组件，而你使用的工具可能在将策略写入 Active Directory 之前不会对其进行验证。 这可能需要花费相当长的时间来诊断问题。  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Active Directory 声明转换规则语言  
  
### <a name="syntax-overview"></a>语法概述  
下面是该语言的语法和语义的简要概述：  
  
-   声明转换规则集包含零个或多个规则。 每个规则有两个活动部分：**选择条件列表**和**规则操作**。 如果 "**选择条件" 列表**的计算结果为 TRUE，则执行相应的规则操作。  
  
-   **选择条件列表**中有零个或多个**选择条件**。 所有**选择条件**的计算结果都必须为 True，**选择条件列表**的计算结果为 true。  
  
-   每个**选择条件**都具有一组零个或多个**匹配条件**。 所有**匹配条件**的计算结果都必须为 true，否则，Select 条件的计算结果为 true。 所有这些条件都针对单个声明进行评估。 与**选择条件**匹配的声明可由**标识符**标记，并在**规则操作**中引用。  
  
-   每个**匹配条件**都通过使用不同的**条件运算符**和**字符串文本**来指定匹配声明的**类型**或**值** **的条件**。  
  
    -   为某个**值**指定**匹配条件**时，还必须为特定**ValueType**指定**匹配条件**，反之亦然。 这些条件必须在语法中彼此相邻。  
  
    -   **ValueType**匹配条件只能使用特定的**ValueType**文本。  
  
-   **规则操作**可以复制用**标识符**标记的一个声明，或者根据用标识符和/或给定字符串文本标记的声明发出一个声明。  
  
**示例规则**  
  
此示例显示了一个规则，该规则可用于在两个林之间转换声明类型，前提是它们使用相同的声明具有，并对此类型的声明值具有相同的解释。 此规则具有一个匹配条件和一个使用字符串文本和匹配声明引用的发出语句。  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>运行时操作  
务必要了解声明转换的运行时操作，以便有效地创作规则。 运行时操作使用三组声明：  
  
1.  **输入声明集**：为声明转换操作提供的输入声明集。  
  
2.  **工作声明集**：在声明转换期间从或写入的中间声明。  
  
3.  **输出声明集**：声明转换操作的输出。  
  
下面是运行时声明转换操作的简要概述：  
  
1.  声明转换的输入声明用于初始化工作声明集。  
  
    1.  处理每个规则时，工作声明集用于输入声明。  
  
    2.  规则中的 "选择条件" 列表与工作声明集中的所有可能声明集相匹配。  
  
    3.  每组匹配声明用于在该规则中运行操作。  
  
    4.  运行规则操作将生成一个声明，并将其追加到输出声明集和工作声明集。 因此，规则的输出用作规则集中后续规则的输入。  
  
2.  规则集中的规则按从第一条规则开始的顺序进行处理。  
  
3.  处理整个规则集时，将处理输出声明集以删除重复的声明和其他安全问题。生成的声明是声明转换过程的输出。  
  
可以根据以前的运行时行为编写复杂的声明转换。  
  
示例：**运行时操作 @ no__t-0  
  
此示例演示使用两个规则的声明转换的运行时操作。  
  
```  
  
     C1:[Type=="EmpType", Value=="FullTime",ValueType=="string"] =>  
                Issue(Type=="EmployeeType", Value=="FullTime",ValueType=="string");  
     [Type=="EmployeeType"] =>   
               Issue(Type=="AccessType", Value=="Privileged", ValueType=="string");  
Input claims and Initial Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"),(Value="Marketing"),(ValueType="String")}  
After Processing Rule 1:  
 Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"), (Value="Marketing"),(ValueType="String")}  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
Output Context:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  
After Processing Rule 2:  
Evaluation Context:  
  {(Type= "EmpType"),(Value="FullTime"),(ValueType="String")}  
{(Type= "Organization"),(Value="Marketing"),(ValueType="String")}  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
Output Context:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
  
Final Output:  
  {(Type= "EmployeeType"),(Value="FullTime"),(ValueType="String")}  
  {(Type= "AccessType"),(Value="Privileged"),(ValueType="String")}  
  
```  
  
### <a name="special-rules-semantics"></a>特殊规则语义  
下面是规则的特殊语法：  
  
1.  空规则集 = = 无输出声明  
  
2.  空选择条件列表 = = 每个声明都匹配选择条件列表  
  
    示例：**空选择条件列表 @ no__t-0  
  
    以下规则匹配工作集中的每个声明。  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  空选择匹配列表 = = 每个声明都匹配选择条件列表  
  
    示例：**空匹配条件 @ no__t-0  
  
    以下规则匹配工作集中的每个声明。 如果单独使用，则这是基本的 "全部允许" 规则。  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>安全注意事项  
**输入林的声明**  
  
需要对传入林的主体所提供的声明进行彻底的检查，以确保仅允许或发出正确的声明。 不正确的声明可能会危及林的安全性，因此，在为进入林的声明创建转换策略时，应该考虑到这一点。  
  
Active Directory 具有以下功能来防止错误配置输入林的声明：  
  
-   如果林信任没有为输入林的声明设置声明转换策略，则为安全起见，Active Directory 删除进入林的所有主体声明。  
  
-   如果在输入林的声明上运行规则集导致声明未在林中定义，则会从输出声明中删除未定义的声明。  
  
**离开林的声明**  
  
与进入林的声明相比，离开林的声明为林带来的安全问题更少。 即使不存在相应的声明转换策略，也允许声明将林保留原样。 在转换离开林的声明的过程中，还可以发出未在林中定义的声明。 这就是通过声明轻松设置跨林信任。 管理员可以确定进入林的声明是否需要转换，并设置相应的策略。 例如，如果需要隐藏声明以防止信息泄露，管理员可以设置策略。  
  
**声明转换规则中的语法错误**  
  
如果给定的声明转换策略具有语法不正确的规则集，或者存在其他语法或存储问题，则该策略被视为无效。 此值的处理方式不同于前面提到的默认条件。  
  
在这种情况下 Active Directory 无法确定目的并进入防故障模式，在该模式下，不会在该信任 + 遍历方向上生成输出声明。 若要解决此问题，需要管理员干预。 如果 LDAP 用于编辑声明转换策略，则会发生这种情况。 适用于 Active Directory 的 Windows PowerShell cmdlet 进行了验证，以防止编写带有语法问题的策略。  
  
## <a name="other-language-considerations"></a>其他语言注意事项  
  
1.  此语言中有几个特殊的关键字或字符（称为端子）。 本主题后面的[语言终端](Claims-Transformation-Rules-Language.md#BKMK_LT)表中提供了这些项。 错误消息将这些终端的标记用于消除歧义。  
  
2.  终端有时可以用作字符串文本。 但是，这种用法可能与语言定义冲突，或者会产生意外后果。 不建议使用这种用法。  
  
3.  规则操作无法对声明值执行任何类型转换，并且包含此类规则操作的规则集被视为无效。 这将导致运行时错误，并且不生成任何输出声明。  
  
4.  如果规则操作引用规则的 "选择条件" 列表部分中未使用的标识符，则该标识符无效。 这会导致语法错误。  
  
    示例：**不正确的标识符引用 @ no__t-0  
    以下规则说明了规则操作中使用的错误标识符。  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>示例转换规则  
  
-   **允许特定类型的所有声明**  
  
    精确类型  
  
    ```  
    C1:[type=="XYZ"] => Issue (claim = C1);  
    ```  
  
    使用 Regex  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **禁止某个声明类型**  
    精确类型  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    使用 Regex  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>规则分析器错误示例  
声明转换规则由自定义分析器进行分析，以检查是否存在语法错误。 在 Active Directory 中存储规则之前，此分析器由相关的 Windows PowerShell cmdlet 来运行。 分析规则（包括语法错误）中的任何错误都将打印在控制台上。 域控制器还在使用转换声明的规则之前运行分析器，并在事件日志中记录错误（添加事件日志号）。  
  
本部分说明了使用不正确的语法编写的规则以及分析器生成的相应语法错误的一些示例。  
  
1. 例如：  
  
   ```  
   c1;[]=>Issue(claim=c1);  
   ```  
  
   此示例使用分号代替了冒号。   
   **错误消息：**  
   *POLICY0002：无法分析策略数据。*  
   *Line 号：1，列号：2，错误令牌：;。行： ' c1;[] = > 问题（声明 = c1）; '。*  
   *Parser 错误：'POLICY0030:语法错误，意外的 ";"，应为以下其中一项： "："。 "*  
  
2. 例如：  
  
   ```  
   c1:[]=>Issue(claim=c2);  
   ```  
  
   在此示例中，复制颁发语句中的标识符标记是不确定的。   
   **错误消息**:   
   *POLICY0011：声明规则中没有符合 CopyIssuanceStatement 中指定的条件标记的条件： "c2"。*  
  
3. 例如：  
  
   ```  
   c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
   ```  
  
   "bool" 不是语言的终端，它不是有效的 ValueType。 以下错误消息中列出了有效的终端。   
   **错误消息：**  
   *POLICY0002：无法分析策略数据。*  
   行号：1，列号：39，错误令牌： "bool"。 行： ' c1： [type = = "x1"，值 = = "1"，valuetype = = "bool"] = > 问题（声明 = c1）; '。   
   *Parser 错误：'POLICY0030:语法错误，意外的 "STRING"，应为以下其中一项："INT64_TYPE" "UINT64_TYPE" "STRING_TYPE" "BOOLEAN_TYPE" "标识符"*  
  
4. 例如：  
  
   ```  
   c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
   ```  
  
   在此示例中，数字**1**不是语言中的有效令牌，并且不允许在匹配条件中使用此类。 必须用双引号引起来，使其成为字符串。   
   **错误消息：**  
   *POLICY0002：无法分析策略数据。*  
   *Line 号：1，列号：23，错误令牌：1.行： ' c1： [type = = "x1"，值 = = 1，valuetype = = "bool"] = > 问题（声明 = c1）; '。* @ no__t-1Parser 错误：'POLICY0029:意外输入。 </em>  
  
5. 例如：  
  
   ```  
   c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
        Issue(type = c1.type, value="0", valuetype == "boolean");  
   ```  
  
   此示例使用了双等号（= =），而不是使用单个等号（=）。   
   **错误消息：**  
   *POLICY0002：无法分析策略数据。*  
   *Line 号：1，列号：91，错误令牌： = =。行： "c1： [type = =" x1 "，值 = =" 1 "，*  
   *valuetype = = "boolean"] = > 问题（类型为 c1. type，value = "0"，valuetype = = "boolean"）; '。*  
   *Parser 错误：'POLICY0030:语法错误，意外的 "= ="，应为以下其中一项： "="*  
  
6. 例如：  
  
   ```  
   c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
         Issue(type=c1.type, value=c1.value, valuetype = "string");  
   ```  
  
   此示例在语法和语义上是正确的。 然而，使用 "boolean" 作为字符串值会导致混淆，应避免此问题。 如前文所述，应尽可能避免使用语言终端作为声明值。  
  
## <a name="BKMK_LT"></a>语言终端  
下表列出了在声明转换规则语言中使用的一组完整的终端字符串和关联的语言终端。 这些定义使用不区分大小写的 UTF-16 字符串。  
  
|字符串|终端|  
|----------|------------|  
|"= >"|隐含|  
|";"|之间|  
|":"|开头|  
|","|跟|  
|"."|着重号|  
|"["|O_SQ_BRACKET|  
|"]"|C_SQ_BRACKET|  
|"("|O_BRACKET|  
|")"|C_BRACKET|  
|"=="|EQ|  
|"!="|NEQ|  
|"=~"|REGEXP_MATCH|  
|"!~"|REGEXP_NOT_MATCH|  
|"="|将|  
|"& &"|和|  
|本期|问题|  
|类别|类型|  
|负值|负值|  
|valuetype|VALUE_TYPE|  
|提及|提及|  
|"[_A-z] [_A-a-za-z0-9] *"|标志|  
|"\\" [^ \\ "\n] * \\" "|STRING|  
|uint64|UINT64_TYPE|  
|int64|INT64_TYPE|  
|类似|STRING_TYPE|  
|变量|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>语言语法  
以下声明转换规则语言是在 ABNF 窗体中指定的。 除了在此处定义的 ABNF 生产外，此定义还使用上表中指定的终端。 规则必须以 UTF-16 编码，并且必须将字符串比较视为不区分大小写。  
  
```  
Rule_set        = ;/*Empty*/  
             / Rules  
Rules         = Rule  
             / Rule Rules  
Rule          = Rule_body  
Rule_body       = (Conditions IMPLY Rule_action SEMICOLON)  
Conditions       = ;/*Empty*/  
             / Sel_condition_list  
Sel_condition_list   = Sel_condition  
             / (Sel_condition_list AND Sel_condition)  
Sel_condition     = Sel_condition_body  
             / (IDENTIFIER COLON Sel_condition_body)  
Sel_condition_body   = O_SQ_BRACKET Opt_cond_list C_SQ_BRACKET  
Opt_cond_list     = /*Empty*/  
             / Cond_list  
Cond_list       = Cond  
             / (Cond_list COMMA Cond)  
Cond          = Value_cond  
             / Type_cond  
Type_cond       = TYPE Cond_oper Literal_expr  
Value_cond       = (Val_cond COMMA Val_type_cond)  
             /(Val_type_cond COMMA Val_cond)  
Val_cond        = VALUE Cond_oper Literal_expr  
Val_type_cond     = VALUE_TYPE Cond_oper Value_type_literal  
claim_prop       = TYPE  
             / VALUE  
Cond_oper       = EQ  
             / NEQ  
             / REGEXP_MATCH  
             / REGEXP_NOT_MATCH  
Literal_expr      = Literal  
             / Value_type_literal  
  
Expr          = Literal  
             / Value_type_expr  
             / (IDENTIFIER DOT claim_prop)  
Value_type_expr    = Value_type_literal  
             /(IDENTIFIER DOT VALUE_TYPE)  
Value_type_literal   = INT64_TYPE  
             / UINT64_TYPE  
             / STRING_TYPE  
             / BOOLEAN_TYPE  
Literal        = STRING  
Rule_action      = ISSUE O_BRACKET Issue_params C_BRACKET  
Issue_params      = claim_copy  
             / claim_new  
claim_copy       = CLAIM ASSIGN IDENTIFIER  
claim_new       = claim_prop_assign_list  
claim_prop_assign_list = (claim_value_assign COMMA claim_type_assign)  
             /(claim_type_assign COMMA claim_value_assign)  
claim_value_assign   = (claim_val_assign COMMA claim_val_type_assign)  
             /(claim_val_type_assign COMMA claim_val_assign)  
claim_val_assign    = VALUE ASSIGN Expr  
claim_val_type_assign = VALUE_TYPE ASSIGN Value_type_expr  
Claim_type_assign   = TYPE ASSIGN Expr  
  
```  
  


