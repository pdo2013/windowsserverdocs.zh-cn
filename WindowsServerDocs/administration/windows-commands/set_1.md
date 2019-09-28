---
title: 设置
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fdd60d6-addf-4574-8c92-8aa53fa73d76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 12dce38bf8ad050c65a7a8c0fca4a71267cca93f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384101"
---
# <a name="set"></a>设置



显示、设置或删除 CMD。EXE 环境变量。 如果不使用参数，则**set 将**显示当前环境变量设置。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
set [<Variable>=[<String>]]
set [/p] <Variable>=[<PromptString>]
set /a <Variable>=<Expression>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Variable >|指定要设置或修改的环境变量。|
|\<String >|指定与指定的环境变量关联的字符串。|
|/p|将*变量*的值设置为用户输入的输入行。|
|\<PromptString >|可选。 指定提示用户输入的消息。 此参数与 **/p**命令行选项一起使用。|
|/a|将*字符串*设置为计算所得的数值表达式。|
|\<Expression >|指定数值表达式。 有关可在*表达式*中使用的有效运算符，请参阅备注。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

- 使用启用了命令扩展的**set**

  启用命令扩展（默认设置）并使用值运行**set**时，将显示以该值开头的所有变量。
- 使用特殊字符

  字符 **<** ， **>** ， **|** ， **&** ， **^** 是特殊命令 shell 字符，它们前面必须是转义符（**1**）或用引号引起来。在*字符串*中使用时（例如， **"StringContaining & Symbol"** ）。 如果使用引号将包含一个特殊字符的字符串括起来，则会将引号设置为环境变量值的一部分。
- 使用环境变量

  使用环境变量控制某些批处理文件和程序的行为，并控制 Windows 和 MS-DOS 子系统的显示和工作方式。 **Set**命令通常在 autoexec.bat 文件中用于设置环境变量。
- 显示当前环境设置

  单独键入**set**命令时，将显示当前的环境设置。 这些设置通常包括 COMSPEC 和 PATH 环境变量，这些变量用于帮助查找磁盘上的程序。 Windows 使用的其他两个环境变量是 PROMPT 和 DIRCMD。
- 使用参数

  当指定*变量*和*字符串*的值时，会将指定的*变量*值添加到环境中，并将*字符串*与该变量关联。 如果环境中已经存在该变量，则新的字符串值将替换旧的字符串值。

  如果为**set**命令仅指定一个变量和一个等号（不包含*string*），则将清除与该变量关联的*字符串*值（就好像该变量不存在）。
- 使用 **/a**

  下表按优先顺序列出了 **/a**支持的运算符。  

  |        Operator         | 执行的操作  |
  |-------------------------|----------------------|
  |           ( )           |       分组       |
  |          ! ~ -          |        一元         |
  |         \*/%          |      算术      |
  |           + -           |      算术      |
  |          < < > >          |    逻辑移位     |
  |            &            |     位与      |
  |            ^            | 位异或 |
  |                         |                      |
  | =  @ no__t =/=% = + =-= & = ^ = |      = < < = > > =       |
  |            ，            | 表达式分隔符 |

  如果使用 logical （ **&&** 或 **||** ）或取模（ **%** ）运算符，请将表达式字符串用引号引起来。 表达式中的任何非数值字符串都被视为环境变量名称，它们的值在处理前转换为数字。 如果指定未在当前环境中定义的环境变量名称，则会分配零值，从而使你可以使用环境变量值执行算术运算，而无需使用% 来检索值。

  如果从命令脚本之外的命令行运行**set/a** ，将显示表达式的最终值。

  数值是十进制数字，除非以0×作为十六进制数字，以0作为八进制数的值。 因此，0×12等效于18，这与022相同。
- 支持延迟的环境变量扩展

  延迟环境变量扩展支持默认情况下处于禁用状态，但你可以使用**cmd/v**启用或禁用该功能。
- 使用命令扩展

  启用命令扩展（默认设置）并单独运行 "设置" 时，它**将**显示所有当前环境变量。 如果使用值运行**set** ，则会显示与该值匹配的变量。
- 在批处理文件中使用**set**

  创建批处理文件时，您可以使用**设置**来创建变量，然后使用与使用编号变量 **% 0**到 **% 9**相同的方式来使用它们。 你还可以使用变量 **% 0**到 **% 9**作为**集**的输入。
- 从批处理文件调用**set**变量

  从批处理文件调用变量值时，请将值括在百分号（ **%** ）中。 例如，如果批处理程序创建名为波特的环境变量，则可通过在命令提示符下键入 **% 波特%** ，将与波特关联的字符串用作可替换参数。
- 在恢复控制台中使用**set**

  可从恢复控制台获取带有不同参数的**set**命令。

## <a name="BKMK_examples"></a>示例

若要设置名为 TEST ^ 1 的环境变量，请键入：
```
set testVar=test^^1
```

> [!NOTE]
> **Set**命令将跟在等号（=）后的所有内容都分配给变量的值。 如果键入：
> ```
> set testVar="test^1"
> ```
> 将得到以下结果：
> ```
> testVar="test^1"
> ```
> 若要设置名为 TEST & 1 的环境变量，请键入：
> ```
> set testVar=test^&1
> ```
> 若要设置名为 INCLUDE 的环境变量，使字符串 C:\Inc （驱动器 C 上的 \Inc 目录）与其关联，请键入：
> ```
> set include=c:\inc
> ```
> 然后，可以通过将名称包含在百分号（ **%** ）中，在批处理文件中使用字符串 C:\Inc。 例如，你可以在批处理文件中包含以下命令，以便显示与 INCLUDE 环境变量关联的目录的内容：
> ```
> dir %include%
> ```
> 处理此命令时，字符串 C:\Inc 将替换 **% include%** 。

你还可以在将新目录添加到 PATH 环境变量的批处理程序中使用**set** 。 例如：
```
@echo off
rem ADDPATH.BAT adds a new directory
rem to the path environment variable.
set path=%1;%path%
set
```
若要显示以字母 P 开头的所有环境变量的列表，请键入：
```
set p 
```

> [!NOTE]
> 此命令需要命令扩展，这些扩展在默认情况下是启用的。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)