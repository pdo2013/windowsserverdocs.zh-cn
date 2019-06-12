---
title: fc
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f7ccc3d268b58bfa5e848f2336f4315baae8b4e1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439308"
---
# <a name="fc"></a>fc



比较两个文件或文件的设置并显示它们之间的差异。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fc /a [/c] [/l] [/lb<N>] [/n] [/off[line]] [/t] [/u] [/w] [/<NNNN>] [<Drive1>:][<Path1>]<FileName1> [<Drive2>:][<Path2>]<FileName2>
fc /b [<Drive1:>][<Path1>]<FileName1> [<Drive2:>][<Path2>]<FileName2>
```

## <a name="parameters"></a>Parameters

|            参数             |                                                                                                                                     描述                                                                                                                                      |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                /a                |                                                 缩写 ASCII 比较的输出。 而不是显示所有行的不同， **fc**显示仅差异的每个集的第一个和最后一个行。                                                  |
|                /b                |             比较两个文件在二进制模式下，按字节，并且不会尝试重新将文件同步后找到不匹配。 这是默认模式以比较具有以下文件扩展名的文件：.exe、.com、.sys、.obj、.lib 或.bin。              |
|                /c                |                                                                                                                               将忽略大小写。                                                                                                                               |
|                /l                |               比较在 ASCII 模式下，行的行和尝试重新将文件同步后找到不匹配的文件。 这是默认模式以比较文件，除具有以下文件扩展名的文件：.exe、.com、.sys、.obj、.lib 或.bin。                |
|             /lb\<N>              |                         设置到的内部缓冲区的行数*N*。行缓冲区的默认长度为 100 行。 如果要比较的文件具有 100 多个连续的不同线条**fc**取消比较。                         |
|                /n                |                                                                                                                ASCII 比较过程中显示的行号。                                                                                                                 |
|            /off[line]            |                                                                                                               不跳过已设置了脱机属性的文件。                                                                                                               |
|                /t                |                                                                    可防止**fc**从将制表符转换为空格。 默认行为是将选项卡视为空格，以及在每个第八个字符位置处停止。                                                                    |
|                /u                |                                                                                                                        将文件作为 Unicode 文本文件进行比较。                                                                                                                         |
|                /w                |         将空白区域 （即，制表符和空格） 压缩的比较过程。 如果某个行包含多个连续空格或制表符， **/w**会将这些字符视为一个空格。 与一起使用时 **/w**， **fc**忽略开头和行尾的空白区域。         |
|             /\<NNNN>             | 指定必须匹配以下不匹配，之前的连续行数**fc**将重新同步的文件。 文件中匹配的行数是否小于*NNNN*， **fc**将匹配的行显示为不同。 默认值为 2。 |
| [\<Drive1>:][<Path1>]<FileName1> |                                                                                        指定的位置和名称的第一个文件或一组文件进行比较。 *FileName1*是必需的。                                                                                        |
| [\<Drive2>:][<Path2>]<FileName2> |                                                                                       指定的位置和名称的第二个文件或一组文件进行比较。 *FileName2*是必需的。                                                                                        |
|                /?                |                                                                                                                         在命令提示符下显示帮助。                                                                                                                         |

## <a name="remarks"></a>备注

-   此命令将通过 c:\WINDOWS\fc.exe implemeted。 可以使用此命令在 PowerShell 中，但请务必拼写出完整可执行文件 (fc.exe)，因为 fc 是自定义格式的别名。

-   报告有关 ASCII 比较文件之间的差异

    当你使用**fc** ASCII 比较**fc**显示按以下顺序的两个文件之间的差异：  
    -   第一个文件的名称
    -   从行*FileName1*在文件之间的区别
    -   在这两个文件中匹配的第一行
    -   第二个文件的名称
    -   从行*FileName2*不同
    -   若要匹配的第一行
-   使用**b </b**进行二进制比较

    **b </b**显示在下面的语法在二进制比较期间找到的不匹配：

    `\<XXXXXXXX: YY ZZ>`

    值*XXXXXXXX*指定的字节，从该文件的开头测量对十六进制的相对地址。 地址开始 00000000。 十六进制值为*YY*并*ZZ*表示不匹配个字节从*FileName1*并*FileName2*分别。
-   使用通配符字符

    可以使用通配符 ( **&#42;** 并 **？** ) 中*FileName1*并*FileName2*。 如果使用中的通配符*FileName1*， **fc**进行比较的文件或指定的文件集到所有指定的文件*FileName2*。 如果使用中的通配符*FileName2*， **fc**使用的相应值*FileName1*。
-   使用内存

    比较 ASCII 文件时**fc**使用内部缓冲区 （大小不足以容纳 100 行） 作为存储。 如果文件大于缓冲区**fc**比较什么它能直接加载到缓冲区。 如果**fc**未找到匹配项中已加载的文件部分，它将停止，并显示以下消息：

    `Resynch failed. Files are too different.`

    在比较二进制文件大于可用内存**fc**比较这两个文件完全覆盖在内存中的部分，使用磁盘中的下一部分。 输出是相同的能完全适合在内存中的文件。

## <a name="BKMK_examples"></a>示例

若要进行的两个文本文件，Monthly.rpt 和 Sales.rpt 的 ASCII 比较，并以缩写格式显示结果，，请键入：
```
fc /a monthly.rpt sales.rpt 
```
若要进行二进制比较的两个批处理文件，Profits.bat 和 Earnings.bat，键入：
```
fc /b profits.bat earnings.bat
```
结果类似于以下内容：
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
Profits.bat 和 Earnings.bat 文件是相同的如果**fc**会显示以下消息：
```
Comparing files Profits.bat and Earnings.bat
FC: no differences encountered
```
若要比较文件 New.bat 当前目录中的每个.bat 文件，请键入：
```
fc *.bat new.bat
```
若要比较的文件 New.bat 驱动器 C 上的驱动器 D 上的文件 New.bat，键入：
```
fc c:new.bat d:*.bat
```
若要比较每个批处理文件的根目录中的文件的驱动器 C 上具有相同名称的驱动器 D 上的根目录中，键入：
```
fc c:*.bat d:*.bat
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
