---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: 声明转换规则语言
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a1f5c724d041a9f64c3b2697a8b5acd17a2a7bd9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445811"
---
# <a name="claims-transformation-rules-language"></a>声明转换规则语言

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

跨林声明转换功能，你向桥声明进行动态访问控制跨林边界通过跨林信任上设置声明转换策略。 所有策略的主要组件是声明转换规则语言编写而成的规则。 本主题提供有关此语言的详细信息，并提供有关创作声明转换规则的指导。  
  
跨林的转换策略的 Windows PowerShell cmdlet 信任选项设置的简单策略需要通用方案。 这些 cmdlet 将用户输入转换策略和在声明转换规则语言中，规则，然后将其存储在 Active Directory 中的规定格式。 有关声明转换的 cmdlet 的详细信息，请参阅[动态访问控制的 AD DS Cmdlet](https://go.microsoft.com/fwlink/?LinkId=243150)。  
  
具体取决于声明配置和放置在 Active Directory 林中的跨林信任上的要求，在声明转换策略可能需要不止适用于处于活动状态的 Windows PowerShell cmdlet 支持的策略目录。 若要有效地创作此类策略，它是一定要注意的声明转换规则语言语法和语义。 此声明在 Active Directory 中的转换规则语言 （"语言"） 是由语言的子集[Active Directory 联合身份验证服务](https://go.microsoft.com/fwlink/?LinkId=243982)类似目的，并具有非常相似的语法和语义。 但是，有更少的操作允许，并且其他语法限制位于 Active Directory 版本的语言。  
  
本主题简要说明的语法和语义的声明转换规则语言中进行创作策略时的 Active Directory 和注意事项。 它提供了多个集的示例规则，以便帮助您入门和不正确的语法和它们生成，以帮助您在创作规则时解密错误消息的消息的示例。  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>用于创作声明转换策略的工具  
**适用于 Active Directory Windows PowerShell cmdlet**:这是首选的推荐方式是以创作并设置声明转换策略。 这些 cmdlet 的简单策略提供开关，并验证为更复杂的策略设置的规则。  
  
**LDAP**:可以在 Active Directory 通过轻型目录访问协议 (LDAP) 中编辑声明转换策略。 但是，建议不要因为策略具有多个复杂的组件，并且您使用的工具可能无法写入到 Active Directory 之前验证策略。 随后，这可能需要相当长的时间来诊断问题。  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Active Directory 声明转换规则语言  
  
### <a name="syntax-overview"></a>语法概述  
下面是语法的简要概述和语言的语义：  
  
-   声明转换规则集包含零个或多个规则。 每个规则具有两个活动的部分：**选择条件列表**并**规则操作**。 如果**选择条件列表**计算结果为 TRUE，执行相应的规则操作。  
  
-   **选择条件列表**具有零个或更多**选择条件**。 所有**选择条件**的计算结果必须为 TRUE**选择条件列表**来计算结果为 TRUE。  
  
-   每个**选择条件**具有零个或多组**匹配条件**。 所有**匹配条件**计算结果必须为 TRUE，选择条件的计算结果为 TRUE。 所有这些条件是针对单个声明计算。 相匹配的声明**选择条件**可以通过标记**标识符**和中引用**规则操作**。  
  
-   每个**匹配条件**指定要匹配的条件**类型**或**值**或者**ValueType**使用不同的声明**条件运算符**并**的字符串文本**。  
  
    -   当指定**匹配条件**有关**值**，还必须指定**匹配条件**特定**ValueType**和反之亦然。 这些条件必须彼此语法中。  
  
    -   **ValueType**匹配条件必须使用特定**ValueType**仅文本。  
  
-   一个**规则操作**可以将复制使用标记的一个声明**标识符**或发出一个声明基于声明的标记标识符和/或给定字符串文本。  
  
**规则示例**  
  
此示例显示可用于转换两个林之间的声明类型，前提是它们使用相同的声明的值，且具有相同的解释的声明值对于此类型的规则。 该规则具有一个匹配条件和使用字符串文本和匹配的声明引用发出语句。  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>运行时操作  
请务必了解声明转换来有效地创作规则的运行时操作。 运行时操作使用三个组的声明：  
  
1.  **输入声明集**:输入的声明转换操作为指定的声明集。  
  
2.  **使用声明集**:从读取和写入在声明转换过程的中间声明。  
  
3.  **输出声明集**:声明转换操作的输出。  
  
下面是运行时声明转换操作的简要概述：  
  
1.  声明转换的输入的声明用于初始化工作声明集。  
  
    1.  在处理每个规则时，工作声明集用于输入声明。  
  
    2.  在规则中的选择条件列表进行匹配的声明从工作声明集的所有可能集。  
  
    3.  每个组的匹配声明用于运行该规则中的操作。  
  
    4.  运行在一个声明中的规则操作结果，这追加到输出声明集和工作声明集。 因此，规则的输出作为输入使用中的规则集的后续规则。  
  
2.  从第一条规则开始按顺序处理规则集中规则。  
  
3.  处理整个规则集时，处理输出声明集以删除重复的声明和其他安全问题。生成的声明将声明转换过程的输出。  
  
它是可以用来编写复杂的声明转换基于以前的运行时行为。  
  
**示例：运行时操作**  
  
此示例演示使用两个规则的声明转换运行时操作。  
  
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
以下是特殊语法规则：  
  
1.  空规则集 = = 任何输出声明  
  
2.  空选择条件列表 = = 每个声明匹配选择条件列表  
  
    **示例：空的选择条件列表**  
  
    下面的规则匹配的工作集中的每个声明。  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  空选择匹配列表 = = 每个声明匹配选择条件列表  
  
    **示例：空匹配条件**  
  
    下面的规则匹配的工作集中的每个声明。 如果单独使用，这是基本"允许的所有"规则。  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>安全注意事项  
**输入一个林的声明**  
  
提供作为传入到一个目录林的主体的声明需要全面检查以确保我们允许或发出正确的声明。 不正确的声明可能危害林安全，并在创作的输入林的声明转换策略时，这应该是最重要的因素。  
  
Active Directory 具有以下功能，可防止配置错误的输入林的声明：  
  
-   如果林信任设置输入的林中，出于安全目的的声明没有声明转换策略 Active Directory 将丢弃输入林的所有主体的声明。  
  
-   如果运行进入林结果中未定义在林中的声明规则集声明上，从输出声明中删除未定义的声明。  
  
**声明离开林**  
  
声明离开林提供较低安全隐患林比输入林的声明。 声明可以保留与林-即使没有任何相应的声明转换策略。 还有可能来颁发声明未定义一部分转换声明离开林的林中。 这是为了轻松地设置跨林信任的声明。 如果输入在林中的声明需要转换，并设置相应的策略，管理员可以确定。 例如，管理员可以设置策略，如果需要隐藏声明以防止信息泄漏。  
  
**声明转换规则中的语法错误**  
  
如果给定的声明转换策略语法不正确的规则集或者其他语法或存储问题，该策略将被视为无效。 这不会被视为比前面所述的默认条件。  
  
Active Directory 无法在这种情况下确定意图并进入防故障模式，该信任 + 遍历方向上生成任何输出声明的位置。 若要更正此问题，需要管理员干预。 如果 LDAP 用于编辑的声明转换策略，则可能发生这种情况。 适用于 Active Directory Windows PowerShell cmdlet 来防止编写语法问题的策略进行验证。  
  
## <a name="other-language-considerations"></a>其他语言注意事项  
  
1.  有多个关键字或特殊在这种语言 （称为终端） 中的字符。 这些单位将显示在[语言终端](Claims-Transformation-Rules-Language.md#BKMK_LT)本主题后面的表。 错误消息对于消除二义性这些终端使用的标记。  
  
2.  终端有时可以用作字符串文本。 但是，此类使用可能会与语言定义冲突，或者具有意想不到的后果。 不建议使用此类型的使用情况。  
  
3.  规则操作不能对执行任何类型转换声明值，还包含这样的规则操作的规则集将被视为无效。 这将导致运行时错误，并不生成任何输出声明。  
  
4.  如果规则操作所引用的选择条件列表部分中未使用的标识符，它是规则的无效的使用情况。 这会导致语法错误。  
  
    **示例：标识符引用不正确**  
    以下规则演示了在规则操作中使用不正确的标识符。  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>示例转换规则  
  
-   **允许特定类型的所有声明**  
  
    确切的类型  
  
    ```  
    C1:[type=="XYZ"] => Issue (claim = C1);  
    ```  
  
    使用正则表达式  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **不允许某些声明类型**  
    确切的类型  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    使用正则表达式  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>规则分析器错误的示例  
若要检查有语法错误的自定义分析器进行分析声明转换规则。 此分析器是存储在 Active Directory 中的规则之前运行相关的 Windows PowerShell cmdlet。 在控制台上输出中分析规则，包括语法错误的任何错误。 域控制器还来转换声明，使用的规则之前运行分析器和它们的事件日志中记录错误 （添加事件日志号）。  
  
本部分演示了编写不正确的语法和相应的语法由分析器生成的错误的规则的一些示例。  
  
1. 例如：  
  
   ```  
   c1;[]=>Issue(claim=c1);  
   ```  
  
   此示例具有不正确使用的分号来代替冒号。   
   **错误消息：**  
   *POLICY0002:无法分析策略数据。*  
   *行号：1，列号：2 个，令牌时出错:;。Line: 'c1;[]=>Issue(claim=c1);'.*  
   *分析器错误：POLICY0030:语法错误，意外 ';'，应为下列选项之一: ':'。*  
  
2. 例如：  
  
   ```  
   c1:[]=>Issue(claim=c2);  
   ```  
  
   在此示例中，复制颁发语句中的标识符标记未定义。   
   **错误消息**:   
   *POLICY0011:中的声明规则没有条件匹配 CopyIssuanceStatement 中指定的条件标签: c2。*  
  
3. 例如：  
  
   ```  
   c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
   ```  
  
   "bool"不是在语言中，终端并不是有效的值类型。 以下的错误消息中列出了有效的终端。   
   **错误消息：**  
   *POLICY0002:无法分析策略数据。*  
   行号：1，列号：39，令牌时出错:"bool"。 Line: 'c1:[type=="x1", value=="1",valuetype=="bool"]=>Issue(claim=c1);'.   
   *分析器错误：POLICY0030:语法错误，意外 STRING，应为以下值之一：'INT64_TYPE' 'UINT64_TYPE' 'STRING_TYPE' 'BOOLEAN_TYPE' 'IDENTIFIER'*  
  
4. 例如：  
  
   ```  
   c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
   ```  
  
   数字**1**中匹配的条件不允许这类使用情况和在此示例中不是在语言中，有效的令牌。 它必须括在双引号内以使它成为字符串。   
   **错误消息：**  
   *POLICY0002:无法分析策略数据。*  
   *行号：1，列号：23，令牌时出错：1.Line: 'c1:[type=="x1", value==1, valuetype=="bool"]=>Issue(claim=c1);'.* <em>Parser error:POLICY0029:意外的输入。</em>  
  
5. 例如：  
  
   ```  
   c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
        Issue(type = c1.type, value="0", valuetype == "boolean");  
   ```  
  
   此示例使用双等号 （= =），而不是单个等号 （=）。   
   **错误消息：**  
   *POLICY0002:无法分析策略数据。*  
   *行号：1，列号：91，令牌时出错: = =。Line: 'c1:[type=="x1", value=="1",*  
   *valuetype=="boolean"]=>Issue(type=c1.type, value="0", valuetype=="boolean");'.*  
   *分析器错误：POLICY0030:语法错误、 意外 = =，应为以下值之一: =*  
  
6. 例如：  
  
   ```  
   c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
         Issue(type=c1.type, value=c1.value, valuetype = "string");  
   ```  
  
   此示例中是语法和语义上正确的。 但是，使用"boolean"字符串值被绑定到会导致混淆，以及应避免使用。 正如前面提到，声明值应尽可能避免使用语言终端。  
  
## <a name="BKMK_LT"></a>语言终端  
下表列出了一整套的终端字符串和声明转换规则语言中使用的关联的语言终端。 这些定义使用不区分大小写的 utf-16 字符串。  
  
|字符串|终端|  
|----------|------------|  
|"=>"|表示|  
|";"|以分号|  
|":"|冒号|  
|","|逗号|  
|"."|圆点|  
|"["|O_SQ_BRACKET|  
|"]"|C_SQ_BRACKET|  
|"("|O_BRACKET|  
|")"|C_BRACKET|  
|"=="|EQ|  
|"!="|NEQ|  
|"=~"|REGEXP_MATCH|  
|"!~"|REGEXP_NOT_MATCH|  
|"="|分配|  
|"&&"|和|  
|"issue"|问题|  
|"type"|类型|  
|"value"|值|  
|"valuetype"|VALUE_TYPE|  
|"claim"|CLAIM|  
|"[_A-Za-z][_A-Za-z0-9]*"|标识符|  
|"\\"[^\\"\n]*\\""|STRING|  
|"uint64"|UINT64_TYPE|  
|"int64"|INT64_TYPE|  
|"string"|STRING_TYPE|  
|"boolean"|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>语言语法  
以下声明转换规则语言 ABNF 窗体中指定。 此定义使用除了在此处定义 ABNF 生产上表中指定的终端。 必须以 utf-16，编码规则和字符串比较必须处理，不区分大小写。  
  
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
  


