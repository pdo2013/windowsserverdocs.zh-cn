---
title: if
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fd7857251b0b6a943f2eea33f56732ec57e7e8d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375478"
---
# <a name="if"></a>if



在批处理程序中执行条件处理。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
if [not] ERRORLEVEL <Number> <Command> [else <Expression>]
if [not] <String1>==<String2> <Command> [else <Expression>]
if [not] exist <FileName> <Command> [else <Expression>]
```
如果启用了命令扩展，请使用以下语法：
```
if [/i] <String1> <CompareOp> <String2> <Command> [else <Expression>]
if cmdextversion <Number> <Command> [else <Expression>]
if defined <Variable> <Command> [else <Expression>]
```

## <a name="parameters"></a>Parameters

|        参数        |                                                                                                                                                                                                                描述                                                                                                                                                                                                                 |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           非           |                                                                                                                                                                              指定仅当条件为 false 时才应执行该命令。                                                                                                                                                                              |
|  errorlevel \<Number >   |                                                                                                                                                      仅当 Cmd.exe 运行的上一个程序返回等于或大于*数字*的退出代码时，才指定 true 条件。                                                                                                                                                       |
|       \<Command >        |                                                                                                                                                                            如果满足前面的条件，则指定应执行的命令。                                                                                                                                                                             |
|  \<String1 > = = <String2>  |                                                                                                             仅当*String1*和*String2*相同时，才指定 true 条件。 这些值可以是文本字符串或批处理变量（例如% 1）。 不需要将文字字符串括在引号中。                                                                                                              |
|    存在 \<FileName >    |                                                                                                                                                                                       如果指定的文件名存在，则指定 true 条件。                                                                                                                                                                                        |
|      \<CompareOp >       |                                                                               指定由三个字母构成的比较运算符。 以下列表表示*CompareOp*的有效值：</br>**等于**等于</br>**NEQ**不等于</br>**LSS**小于</br>**LEQ**小于或等于</br>**GTR**大于</br>**GEQ**大于或等于                                                                                |
|           i            |                                                            强制字符串比较忽略大小写。  **如果**为，则可以在<em>String1</em> **==** <em>string2</em>形式使用 **/i**形式。 这些比较是泛型的，因为如果*String1*和*string2*只包含数字，则会将字符串转换为数字，并执行数值比较。                                                            |
| cmdextversion \<Number > | 仅当与 Cmd.exe 的命令扩展功能相关联的内部版本号等于或大于指定的数字时，才指定 true 条件。 第一个版本为1。 当向命令扩展添加重大增强功能时，它会递增1。 禁用命令扩展时， **cmdextversion**条件始终为 true （默认情况下，启用命令扩展）。 |
|   定义 \<Variable >   |                                                                                                                                                                                            如果定义了*变量*，则指定 true 条件。                                                                                                                                                                                            |
|      \<Expression >      |                                                                                                                                                                   指定要传递给**else**子句中的命令的命令行命令和任何参数。                                                                                                                                                                   |
|           /?            |                                                                                                                                                                                                    在命令提示符下显示帮助。                                                                                                                                                                                                    |

## <a name="remarks"></a>备注

-   如果在**if**子句中指定的条件为 true，则执行条件下的命令。如果条件为 false，则忽略**if**子句中的命令，该命令将执行**else**子句中指定的任何命令。
-   当程序停止时，它将返回退出代码。 若要使用退出代码作为条件，请使用**errorlevel**。
-   如果你使用**定义**的，则以下三个变量将添加到环境中： **% errorlevel%** 、 **% cmdcmdline%** 和 **% cmdextversion%** 。  
    -   **% errorlevel%** 展开为 errorlevel 环境变量的当前值的字符串表示形式。 这假定不存在名为 ERRORLEVEL 的环境变量（如果有），则将改为获取 ERRORLEVEL 值。
    -   **% cmdcmdline%** 将扩展到在任何由 cmd.exe 处理之前传递给 cmd.exe 的原始命令行。 这假定不存在名为 CMDCMDLINE 的现有环境变量-如果存在，则将改为获取 CMDCMDLINE 值。
    -   **% cmdextversion%** 展开为**cmdextversion**的当前值的字符串表示形式。 这假定不存在名为 CMDEXTVERSION 的现有环境变量-如果存在，则将改为获取 CMDEXTVERSION 值。
-   在**if**之后，必须在命令所在的行上使用**else**子句。

## <a name="BKMK_examples"></a>示例

若要显示消息 "找不到数据文件"，如果找不到该文件，请键入：
```
if not exist product.dat echo Cannot find data file 
```
若要格式化驱动器 A 中的磁盘，并在格式化过程中出现错误时显示一条错误消息，请在批处理文件中键入以下行：
```
:begin
@echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```
若要从当前目录中删除文件 Product .dat，或在找不到 Product .dat 时显示消息，请在批处理文件中键入以下行：
```
IF EXIST Product.dat (
del Product.dat
) ELSE (
echo The Product.dat file is missing.
)
```

> [!NOTE]
> 可以按如下所示将这些行合并为一行：
> ```
> IF EXIST Product.dat (del Product.dat) ELSE (echo The Product.dat file is missing.)
> ```
> 若要在运行批处理文件后回显 ERRORLEVEL 环境变量的值，请在批处理文件中键入以下行：
> ```
> goto answer%errorlevel%
> :answer1
> echo Program had return code 1
> :answer0
> echo Program had return code 0
> goto end
> :end
> echo Done! 
> ```
> 若要在 ERRORLEVEL 环境变量的值小于或等于1的情况下切换到 "正常" 标签，请键入：
> ```
> if %errorlevel% LEQ 1 goto okay
> ```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

[如果](if.md)

[语句](goto.md)