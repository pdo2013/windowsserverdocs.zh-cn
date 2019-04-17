---
ms.assetid: e831f781-3c45-4d44-b411-160d121d1324
title: "索赔转换规则语言"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4a6b378bc4aef180ebedd260008febaa2f2a76ae
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="claims-transformation-rules-language"></a>索赔转换规则语言

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在森林索赔转换功能使你大桥索赔动态访问控制森林跨通过上林跨信任转换策略设置索赔。 所有策略主要组件是规则编写的索赔转换规则语言。 本主题提供有关该语言的详细信息，并提供有关创作索赔转换规则指南。  
  
在森林转换策略的 Windows PowerShell cmdlet 信任选项将简单的策略设置需要何共同点方案。 这些 cmdlet 翻译策略和中索赔转换规则语言，规则的用户输入，然后将它们规定格式存储 Active Directory 中。 有关 cmdlet 索赔转换的详细信息，请参阅[动态访问控制广告 DS Cmdlet](https://go.microsoft.com/fwlink/?LinkId=243150)。  
  
根据我们的索赔配置和置于 Active Directory 林中跨森林信任的要求，你的索赔转换策略可能需要比适用 Active Directory 的 Windows PowerShell cmdlet 支持策略复杂。 若要实际上创作此类策略，非常重要，了解的索赔转换规则语言语法和语义。 此声明转换规则语言（"语言"）的 Active Directory 中为所使用的语言子集[Active Directory 联合身份验证服务](https://go.microsoft.com/fwlink/?LinkId=243982)类似目标，并且具有非常类似语法和语义有关。 但是，有更少操作允许，并且其他语法限制 Active Directory 的语言的版本中。  
  
本主题简要说明语法和语义中 Active Directory 和注意事项创作策略时进行索赔转换规则语言。 它提供了多个组规则示例，以帮助你开始使用，并示例语法错误，以及他们生成，以帮助你在创作规则时解密错误消息的消息。  
  
## <a name="tools-for-authoring-claims-transformation-policies"></a>工具创作索赔转换策略  
**Active Directory 的 Windows PowerShell cmdlet**：这是首选和建议方法编写并设置转换策略的索赔。 这些 cmdlet 简单策略提供开关，并验证规则有关更复杂的策略设置。  
  
**LDAP**：索赔转换策略可轻型目录访问协议 (LDAP) 通过 Active Directory 中进行编辑。 但是，这不被推荐由于策略几个复杂的组件，而且你使用的工具可能不然后再写入 Active Directory 验证策略。 随后，这可能需要花费的时间来诊断问题。  
  
## <a name="active-directory-claims-transformation-rules-language"></a>Active Directory 索赔转换规则语言  
  
### <a name="syntax-overview"></a>语法概述  
下面介绍了语法的简短概述以及语义语言：  
  
-   索赔转换规则集包含或有多个规则。 每个规则有两个活动部分：**条件列表中选择**和**规则操作**。 如果**条件列表中选择**为 TRUE，评估执行相应规则操作。  
  
-   **选择条件列表**具有零或多个**选择条件**。 所有**选择条件**必须为 TRUE 评估**条件列表中选择**为 TRUE 评估。  
  
-   每个**选择条件**有零量不少于一组**匹配条件**。 所有**匹配条件**必须为 TRUE 选择条件为 TRUE 评估评估。 所有这些条件评估防御单个声明。 匹配的索赔**选择条件**可以通过标记**标识符**和中提到**规则操作**。  
  
-   每个**匹配条件**指定匹配的条件**类型**或**值**或**值**索赔使用不同的**条件运算符**和**字符串**。  
  
    -   指定时**匹配条件**为**值**，还必须指定**匹配条件**特定**值**，反之亦然。 这些条件必须在语法彼此旁边。  
  
    -   **值**匹配的情形必须使用特定**值**仅文本。  
  
-   一个**规则操作**可以复制使用标记的一个声明**标识符**或发出一个声明根据标记有标识符索赔和/或提供字符串。  
  
**示例规则**  
  
此示例显示了用于翻译两个林，之间的类型索赔，前提是他们使用同一索赔的值和有为此类提起的索赔值同一解释规则。 规则有一个匹配的条件以及使用字符串和匹配的索赔参考问题声明。  
  
```  
C1: [TYPE=="EmployeeType"]    
                 => ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE);  
[TYPE=="EmployeeType"] == Select Condition List with one Matching Condition for claims Type.  
ISSUE (TYPE= "EmpType", VALUE = C1.VALUE, VALUETYPE = C1.VALUETYPE) == Rule Action that issues a claims using string literal and matching claim referred with the Identifier.  
  
```  
  
### <a name="runtime-operation"></a>运行时操作  
请务必了解的索赔转换创作规则有效地运行时运行。 运行时操作使用索赔三个组：  
  
1.  **输入索赔组**：为索赔转换操作的索赔输入的设置。  
  
2.  **工作索赔组**：中间从读取和写入索赔转换期间的索赔。  
  
3.  **输出索赔组**：索赔转换操作的输出。  
  
以下是运行时索赔转换操作的简要概述方法：  
  
1.  输入提起的索赔索赔转换用于初始化工作索赔集。  
  
    1.  在处理每个规则时, 工作索赔集用于输入的索赔。  
  
    2.  选择条件列表规则中进行匹配对所有可能的索赔工作集的索赔。  
  
    3.  每个套匹配的索赔用于在该规则运行操作。  
  
    4.  运行规则操作产生的一个索赔，这将其附加到输出索赔集和工作索赔集。 因此，从规则输出作为输入使用规则集中后续规则。  
  
2.  按顺序从第一个规则开始处理规则集中规则。  
  
3.  当处理整个规则组时，输出索赔集处理删除重复索赔和 issues.The 结果索赔是索赔转换进程的输出。  
  
它是可以编写复杂索赔转换根据以前运行时行为。  
  
**示例：运行时操作**  
  
此示例显示了使用两个规则索赔转换的运行时运行。  
  
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
  
### <a name="special-rules-semantics"></a>特殊的规则语义  
特殊的语法，规则如下：  
  
1.  清空规则集 = = 任何输出声明  
  
2.  清空条件列表中选择 = = 每隔索赔匹配条件列表中选择  
  
    **示例：空白选择条件列表**  
  
    以下规则匹配每工作集中的索赔。  
  
    ```  
    => Issue (Type = "UserType", Value = "External", ValueType = "string")  
    ```  
  
3.  清空中选择匹配列表 = = 每索赔匹配条件列表中选择  
  
    **示例：清空匹配的情形**  
  
    以下规则匹配每工作集中的索赔。 如果单独使用它，这是基本"允许全部"规则。  
  
    ```  
    C1:[] => Issule (claim = C1);  
    ```  
  
## <a name="security-considerations"></a>安全注意事项  
**输入森林的声明**  
  
多个传入到森林的主体呈现索赔需要完全擦除检查以确保我们允许或发出只是正确的索赔。 不正确的索赔可以破坏森林安全性，并创作输入森林的索赔策略转换时，这应该是顶部的考虑因素。  
  
Active Directory 包含以下功能，用于防止错误配置的输入森林的索赔：  
  
-   如果森林信任已设置为输入森林，为安全起见，索赔没有索赔转换策略 Active Directory 丢弃输入森林所有主体索赔。  
  
-   如果运行的规则索赔上设置进入森林结果中未树林中定义的索赔，将会从输出索赔删除不确定的索赔。  
  
**退出森林的声明**  
  
退出森林的索赔出示林比输入森林索赔较小的安全问题。 允许索赔离开作为森林的是，即使没有相应的索赔转换策略在就位。 它还有可能处理未作为转换离开森林的声明树林中定义的索赔。 这是为了方便地设置跨森林信任的索赔。 管理员可以确定是否输入森林的索赔需要将其转换，并设置相应的策略。 例如，是否需要隐藏索赔以防止信息泄露管理员，就可以设置策略。  
  
**语法错误索赔转换规则中**  
  
如果给定的索赔转换策略具有规则集语法不正确，或者如果有其他语法或存储的问题，该策略视为无效。 这被视为不同的默认要件就上文所述。  
  
Active Directory 无法确定目的在此情况下，将进入一故障保护模式、没有输出索赔该信任 + 遍历方向上生成的位置。 管理员干预需要解决该问题。 如果 LDAP 用于编辑索赔转换策略，则可能发生这种情况。 Active Directory 的 Windows PowerShell cmdlet 以防止编写语法问题策略了验证。  
  
## <a name="other-language-considerations"></a>其他语言注意事项  
  
1.  有几个关键词或采用此语言（也称为终端）特殊的字符。 这些均以[语言终端](Claims-Transformation-Rules-Language.md#BKMK_LT)本主题后面的表。 错误消息用于为区分这些终端标记。  
  
2.  有时可以终端用作字符串。 但是，此类使用可能冲突与语言清晰度，或者意外的后果。 不建议使用此类。  
  
3.  上，规则操作无法执行任何类型转换声称值，并包含规则操作规则集被认为是无效。 这将导致运行时错误，并且会产生任何输出索赔。  
  
4.  如果规则操作是指不再使用规则的条件列表中选择部分中，标识符，则无效的使用量。 这会导致语法错误。  
  
    **示例：错误标识符参考**  
    以下规则阐释了错误的使用规则孔标识符。  
  
    ```  
    C1:[] => Issue (claim = C2);  
    ```  
  
## <a name="sample-transformation-rules"></a>示例转换规则  
  
-   **允许某些类型的所有声明**  
  
    精确类型  
  
    ```  
    C1:[type=="XYZ"] => Issue (claim = C1);  
    ```  
  
    使用 Regex  
  
    ```  
    C1: [type =~ "XYZ*"] => Issue (claim = C1);  
    ```  
  
-   **不允许某一索赔类型**  
    精确类型  
  
    ```  
    C1:[type != "XYZ"] => Issue (claim=C1);  
    ```  
  
    使用 Regex  
  
    ```  
    C1:[Type !~ "XYZ?"] => Issue (claim=C1);  
    ```  
  
## <a name="examples-of-rules-parser-errors"></a>规则分析器错误示例  
自定义分析器语法错误检查都分析索赔转换规则。 此分析器由相关的 Windows PowerShell cmdlet 运行 Active Directory 中存储规则之前。 在主机上打印分析规则，包括语法错误中的任何错误。 域控制器也在使用规则的转换声明中之前, 运行分析器和错误登录事件日志（添加事件日志号码）。  
  
此部分中所示编写语法错误和相应语法错误，所生成的分析器规则一些的示例。  
  
1.  示例：  
  
    ```  
    c1;[]=>Issue(claim=c1);  
    ```  
  
    此示例中有错误二手的分号来替代分号。   
    **错误消息：**  
    *POLICY0002：无法分析策略数据。*  
    *行号：月 1 日，列号：2，错误令牌:;。 行: c1;= > Issue(claim=c1);。*  
    *分析器错误: POLICY0030：语法错误，但意外 ';，需要以下选项之一::'。*  
  
2.  示例：  
  
    ```  
    c1:[]=>Issue(claim=c2);  
    ```  
  
    在此示例中，副本颁发声明中的标识符标记未定义。   
    **错误消息**:   
    *POLICY0011：索赔规则中的没有条件匹配 CopyIssuanceStatement 中指定的状态标记: 'c2。*  
  
3.  示例：  
  
    ```  
    c1:[type=="x1", value=="1", valuetype=="bool"]=>Issue(claim=c1)  
    ```  
  
    "布尔"终端语言，并不有效值。 有效的终端以下错误消息中列出。   
    **错误消息：**  
    *POLICY0002：无法分析策略数据。*  
    行号：月 1 日，列号：39，错误令牌:"布尔"。 行: c1: [类型 = ="x1"，值 = ="1"值"布尔"= =] = > Issue(claim=c1);。   
    *分析器错误: POLICY0030：语法错误，意外字符串，预期会收到以下选项之一: INT64_TYPE' UINT64_TYPE' STRING_TYPE' BOOLEAN_TYPE' 标识符*  
  
4.  示例：  
  
    ```  
    c1:[type=="x1", value==1, valuetype=="boolean"]=>Issue(claim=c1);  
    ```  
  
    数值形式**1**在此示例中有效标记语言，并不匹配条件中不允许使用这种情况。 它还会包含在双引号以使它成为字符串中。   
    **错误消息：**  
    *POLICY0002：无法分析策略数据。*  
    *行号：月 1 日，列号：23 日错误令牌：1。行: c1: [类型 = ="x1"，值 = = 1 的值 ="布尔"=] = > Issue(claim=c1);。**分析器错误: POLICY0029：意外的输入。*  
  
5.  示例：  
  
    ```  
    c1:[type == "x1", value == "1", valuetype == "boolean"] =>   
  
         Issue(type = c1.type, value="0", valuetype == "boolean");  
    ```  
  
    此示例中使用双等于号（= =），而不是一个等于号（=）。   
    **错误消息：**  
    *POLICY0002：无法分析策略数据。*  
    *行号：月 1 日，列号：91，错误令牌: = =。 行: c1: [类型"x1"，值 = = = ="1"，*  
    *值 ="布尔值"=] = > 问题 (type=c1.type、值 ="0"，值 = ="布尔");。*  
    *分析器错误: POLICY0030：语法错误，意外 = =，预期会收到以下选项之一: =*  
  
6.  示例：  
  
    ```  
    c1:[type=="x1", value=="boolean", valuetype=="string"] =>   
  
          Issue(type=c1.type, value=c1.value, valuetype = "string");  
    ```  
  
    此示例中是语法和语义正确。 但是，在字符串值绑定导致的困惑，以及应避免使用"布尔值"。 如上文所述，使用语言终端，因为尽可能应避免索赔值。  
  
## <a name="BKMK_LT"></a>语言终端  
下表列出了终端字符串一整套索赔转换规则语言中使用的关联的语言终端。 这些定义使用不区分大小写 UTF-16 字符串。  
  
|字符串|终端|  
|----------|------------|  
|"=>"|意味着|  
|";"|分号|  
|":"|分号|  
|","|逗号|  
|"."|点|  
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
|"问题"|问题|  
|"系统类型"|键入|  
|"值"|值|  
|"值"|VALUE_TYPE|  
|"索赔"|声明|  
|"[_A-Za-z][_A-Za-z0-9]*"|标识符|  
|"\\"[^\\"\n]*\\""|字符串|  
|"uint64"|UINT64_TYPE|  
|"int64"|INT64_TYPE|  
|"字符串"|STRING_TYPE|  
|"布尔"|BOOLEAN_TYPE|  
  
## <a name="language-syntax"></a>语法语言  
下面的索赔转换规则语言 ABNF 表单中指定。 此定义使用上表除了此处定义 ABNF 生产中指定的终端。 必须进行编码规则，请在和字符串比较必须处理区分。  
  
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
  


