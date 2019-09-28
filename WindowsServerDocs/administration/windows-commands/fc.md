---
title: fc
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 485fc3d8-b7c5-496d-87be-0011112f27d5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f6c004fcebcf5eb743354d9e0a121ff8598217a4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377246"
---
# <a name="fc"></a>fc



比较两个文件或文件集，并显示它们之间的差异。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fc /a [/c] [/l] [/lb<N>] [/n] [/off[line]] [/t] [/u] [/w] [/<NNNN>] [<Drive1>:][<Path1>]<FileName1> [<Drive2>:][<Path2>]<FileName2>
fc /b [<Drive1:>][<Path1>]<FileName1> [<Drive2:>][<Path2>]<FileName2>
```

## <a name="parameters"></a>Parameters

|            参数             |                                                                                                                                     描述                                                                                                                                      |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                /a                |                                                 缩写 ASCII 比较的输出。 **Fc**只显示每个差异集的第一行和最后一行，而不是显示所有不同的行。                                                  |
|                /b                |             在二进制模式下将两个文件按字节进行比较，并且不会在找到不匹配后尝试重新同步文件。 这是比较具有以下文件扩展名的文件的默认模式： .exe、.com、.sys、.obj、.lib 或 bin。              |
|                /c                |                                                                                                                               忽略字母大小写。                                                                                                                               |
|                /l                |               在 ASCII 模式下逐行比较文件，并在找到不匹配项后尝试重新同步文件。 这是用于比较文件的默认模式，但文件扩展名为： .exe、.com、.sys、.obj、.lib 或 bin。                |
|             /lb @ no__t-0N >              |                         将内部行缓冲区的行数设置为*N*。行缓冲区的默认长度为100行。 如果要比较的文件具有超过100个连续不同的行， **fc**会取消比较。                         |
|                /n                |                                                                                                                在 ASCII 比较中显示行号。                                                                                                                 |
|            /off [line]            |                                                                                                               不会跳过设置了脱机属性的文件。                                                                                                               |
|                /t                |                                                                    阻止**fc**将制表符转换为空格。 默认行为是将制表符视为空格，并在每八个字符位置停止。                                                                    |
|                /u                |                                                                                                                        将文件作为 Unicode 文本文件进行比较。                                                                                                                         |
|                /w                |         在比较期间压缩空白（即，制表符和空格）。 如果行包含多个连续的空格或制表符，则 **/w**会将这些字符视为单个空格。 与 **/w**一起使用时， **fc**将忽略行开头和结尾处的空格。         |
|             / @ NO__T-1NNNN >             | 指定在**fc**认为要重新同步的文件之前，必须在不匹配的情况下匹配的连续行数。 如果文件中的匹配行数小于*NNNN*， **fc**会将匹配行显示为不同。 默认值为2。 |
| [@no__t >：][<Path1>] <FileName1> |                                                                                        指定要比较的第一个文件或一组文件的位置和名称。 *FileName1*是必需的。                                                                                        |
| [@no__t >：][<Path2>] <FileName2> |                                                                                       指定要比较的第二个文件或文件集的位置和名称。 *FileName2*是必需的。                                                                                        |
|                /?                |                                                                                                                         在命令提示符下显示帮助。                                                                                                                         |

## <a name="remarks"></a>备注

-   此命令由 c:\WINDOWS\fc.exe. implemeted 可以在 PowerShell 中使用此命令，但请务必对完整的可执行文件（fc-al）进行拼写检查，因为 "fc" 是格式自定义的别名。

-   报告 ASCII 比较文件之间的差异

    使用**fc**进行 ASCII 比较时， **fc**会按以下顺序显示两个文件之间的差异：  
    -   第一个文件的名称
    -   不同文件之间的*FileName1*的行
    -   要在两个文件中匹配的第一行
    -   第二个文件的名称
    -   不同*FileName2*的行
    -   要匹配的第一行
-   使用 **/b**进行二进制比较

    **/b**显示在二进制比较过程中发现的不匹配项，采用以下语法：

    `\<XXXXXXXX: YY ZZ>`

    *Xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx*的值指定从文件开头开始度量的字节对的相对十六进制地址。 地址以00000000开头。 *YY*和*ZZ*的十六进制值分别表示*FileName1*和*FileName2*中不匹配的字节。
-   使用通配符

    可以在*FileName1*和*FileName2*中 **&#42;** 使用通配符（和 **？** ）。 如果在*FileName1*中使用通配符， **fc**会将所有指定的文件与*FileName2*指定的文件或文件集进行比较。 如果在*FileName2*中使用通配符， **Fc**将使用*FileName1*中的相应值。
-   使用内存

    比较 ASCII 文件时， **fc**使用内部缓冲区（足以容纳100行）作为存储。 如果文件大于缓冲区， **fc**就会将它加载到缓冲区中的内容进行比较。 如果**fc**在文件的加载部分中找不到匹配项，它将停止并显示以下消息：

    `Resynch failed. Files are too different.`

    比较大于可用内存的二进制文件时， **fc**会完全比较这两个文件，将内存中的部分覆盖到磁盘中的下一部分。 此输出与完全容纳在内存中的文件的输出相同。

## <a name="BKMK_examples"></a>示例

若要对两个文本文件（rpt 和 rpt）进行 ASCII 比较，并以缩写格式显示结果，请键入：
```
fc /a monthly.rpt sales.rpt 
```
若要对两个批处理文件进行二进制比较，请键入：
```
fc /b profits.bat earnings.bat
```
将显示类似于以下内容的结果：
```
00000002: 72 43
00000004: 65 3A
0000000E: 56 92
...
...
...
000005E8: 00 6E
FC: Earnings.bat longer than Profits.bat
```
如果基本 .bat 和收益文件相同， **fc**将显示以下消息：
```
Comparing files Profits.bat and Earnings.bat
FC: no differences encountered
```
若要将当前目录中的每个 .bat 文件与文件 .bat 进行比较，请键入：
```
fc *.bat new.bat
```
若要将驱动器 C 上的文件 .bat 与驱动器 D 上的 .bat 文件进行比较，请键入：
```
fc c:new.bat d:*.bat
```
若要将驱动器 C 上根目录中的每个批处理文件与驱动器 D 上根目录中的文件进行比较，请键入：
```
fc c:*.bat d:*.bat
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
