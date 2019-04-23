---
title: if
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 698b3fb9-532b-4c2b-af7f-179f8dc57131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2fafd14b0b620a8b5630c869e33c5dbc7cd902f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869228"
---
# <a name="if"></a>if



在批处理程序中执行的有条件处理。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
if [not] ERRORLEVEL <Number> <Command> [else <Expression>]
if [not] <String1>==<String2> <Command> [else <Expression>]
if [not] exist <FileName> <Command> [else <Expression>]
```
如果启用了命令扩展，使用以下语法：
```
if [/i] <String1> <CompareOp> <String2> <Command> [else <Expression>]
if cmdextversion <Number> <Command> [else <Expression>]
if defined <Variable> <Command> [else <Expression>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|非|指定该命令仅当条件为 false 时，才执行。|
|errorlevel\<数 >|仅当通过 Cmd.exe 运行上一个程序返回了退出代码，等于或大于指定条件为真*数*。|
|\<命令 >|指定如果满足上述条件，则应执行的命令。|
|\<String1>==<String2>|指定为真的条件才*String1*并*String2*是相同的。 这些值可以是文本字符串或 batch 变量 (例如，%1)。 不需要用引号括住文字字符串。|
|存在\<文件名 >|如果存在指定的文件名称，指定为真的条件。|
|\<CompareOp>|指定三个字母的比较运算符。 下表显示有效值*CompareOp*:</br>**EQU**等于</br>**NEQ**不等于</br>**LSS**小于</br>**LEQ**小于或等于</br>**GTR**大于</br>**GEQ**大于或等于|
|i|强制的字符串比较中忽略大小写。  可以使用 **/i**上*String1***==*** String2*形式**如果**。 这些比较是通用的如果两个*String1*并*String2*由的数字组成，将字符串转换为数字，执行数值比较。|
|cmdextversion\<数 >|指定为真的条件仅当与 Cmd.exe 的功能是等于命令扩展相关联或大于指定的数字的内部版本号。 第一个版本为 1。 它通过一为增量提高时重要的增强功能添加到命令扩展。 **Cmdextversion**条件永远不会真是当命令扩展已禁用 （默认情况下，启用扩展的命令）。|
|定义\<变量 >|如果指定条件为真*变量*定义。|
|\<表达式 >|指定要传递到中的命令的命令行命令和任何参数**其他**子句。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   如果在指定的条件**如果**子句为 true 时，系统将执行该条件命令。如果条件为 false 中的命令**如果**忽略子句和执行的命令中指定任何命令**其他**子句。
-   如果程序停止，它会返回退出代码。 若要使用退出代码作为条件，请使用**errorlevel**。
-   如果您使用**定义**，以下三个变量添加到环境： **%errorlevel%**， **%cmdcmdline%**，和 **%cmdextversion%**.  
    -   **%errorlevel%** 会扩展到 ERRORLEVEL 环境变量的当前值的字符串表示形式。 这假定是不现有环境变量，其名称 ERRORLEVEL，如果没有，你将改为获取该 ERRORLEVEL 值。
    -   **%cmdcmdline%** 会扩展到原始命令行传递到 Cmd.exe 在任何处理之前由 Cmd.exe。 这假定是不现有环境变量，其名称 CMDCMDLINE — 如果没有，你将改为获取 CMDCMDLINE 值。
    -   **%cmdextversion%** 会扩展到的当前值的字符串表示形式**cmdextversion**。 这假定是不现有环境变量，其名称 CMDEXTVERSION — 如果没有，你将改为获取 CMDEXTVERSION 值。
-   必须使用**else**后命令所在的同一行上的子句**如果**。

## <a name="BKMK_examples"></a>示例

若要显示消息"找不到数据文件"如果文件不能找到 Product.dat，键入：
```
if not exist product.dat echo Cannot find data file 
```
要格式化驱动器 A 中的磁盘，并显示一条错误消息，如果在格式设置过程中发生错误，请在批处理文件中键入以下行：
```
:begin
@echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```
若要从当前目录中删除文件 Product.dat 或如果找不到 Product.dat 显示一条消息，请在批处理文件中键入以下行：
```
IF EXIST Product.dat (
del Product.dat
) ELSE (
echo The Product.dat file is missing.
)
```

> [!NOTE]
> 这些线可以结合起来，为单个行，如下所示：
```
IF EXIST Product.dat (del Product.dat) ELSE (echo The Product.dat file is missing.)
```
运行批处理文件后回显 ERRORLEVEL 环境变量的值，请在批处理文件中键入以下行：
```
goto answer%errorlevel%
:answer1
echo Program had return code 1
:answer0
echo Program had return code 0
goto end
:end
echo Done! 
```
ERRORLEVEL 环境变量的值是否小于或等于 1，类型，请转到"正常"标签：
```
if %errorlevel% LEQ 1 goto okay
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

[If](if.md)

[转到](goto.md)