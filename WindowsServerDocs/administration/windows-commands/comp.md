---
title: comp
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40319d23-704d-4da1-be93-8259547275d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d10b86176d97e1afd76085516fbfc00bdc36577f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854678"
---
# <a name="comp"></a>comp



比较两个文件的内容或集的文件字节。 如果使用不带参数， **comp**会提示你输入文件进行比较。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
comp [<Data1>] [<Data2>] [/d] [/a] [/l] [/n=<Number>] [/c]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Data1 >|指定的位置和名称的第一个文件或一组你想要比较的文件。 可以使用通配符 (**&#42;** 并 **？**) 来指定多个文件。|
|\<Data2>|指定的位置和名称的第二个文件或一组你想要比较的文件。 可以使用通配符 (**&#42;** 并 **？**) 来指定多个文件。|
|/d|显示十进制格式之间的差异。 （默认格式。 十六进制）|
|/a|将差异显示为字符。|
|/l|显示出现不同，而不是显示字节偏移量的行数。|
|/n=\<Number>|比较仅为每个文件中，指定的行数，即使这些文件是不同的大小。|
|/c|执行不区分大小写的比较。|
|/off[line]|处理与脱机属性集的文件。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   如何**comp**命令标识不匹配的信息

    在比较过程**comp**显示标识的文件之间的不同信息的位置的消息。 每条消息指示不相等的字节偏移量的内存地址和字节的内容 (以十六进制表示法除非 **/a**或 **/d**指定命令行参数)。 消息将显示在以下格式：

    `Compare error at OFFSET xxxxxxxx`

    `file1 = xx`

    `file2 = xx`

    十个不相等的比较之后, **comp**停止比较文件，并显示以下消息：

    `10 Mismatches - ending compare`
-   处理的特殊用例*Data1*和*Data2*  
    -   如果省略的必要的组件*Data1*或*Data2*或如果省略*Data2*， **comp**会提示您输入缺少的信息。
    -   如果*Data1*包含驱动器号或目录名称使用任何文件名**comp**将所有指定的目录中指定的文件中的文件进行比较*Data1*。
    -   如果*Data2*包含驱动器号或目录名称，默认文件名*Data2*中的相同*Data1*。
    -   如果**comp**找不到指定，它会提示你使用一条消息，以确定你是否想要比较多个文件的文件。
-   比较在不同位置的文件

    **Comp**可以比较文件所在的驱动器上或在不同的驱动器上和相同的目录中或在不同目录中。 当**comp**比较文件，将显示其位置和文件名。
-   比较具有相同名称的文件

    进行比较的文件可以有相同的文件名，前提是在不同目录中还是在不同的驱动器。 如果未指定该文件的名称*Data2*，默认文件名*Data2*中的文件名称相同*Data1*。 可以使用通配符 (**&#42;** 并 **？**) 来指定文件的名称。
-   比较不同大小的文件

    必须指定 **/n**比较不同大小的文件。 如果不同的文件大小和 **/n**未指定，则**comp**会显示以下消息：

    `Files are different sizes`

    `Compare more files (Y/N)?`

    要比较这些文件，按 N 若要停止**comp**命令。 然后，重新运行**comp**命令 **/n**选项来比较仅每个文件的第一个部分。
-   按顺序比较文件

    如果使用通配符字符 (**&#42;** 并 **？**) 来指定多个文件**comp**查找匹配的第一个文件*Data1*并将它与中的相应文件进行比较*Data2*，如果它存在。 **Comp**命令报告的每个文件匹配的比较结果*Data1*。 完成后， **comp**会显示以下消息：

    `Compare more files (Y/N)?`

    若要比较多个文件，请按 Y。**Comp**命令会提示输入位置和新文件的名称。 若要停止比较，按 n。当您按 Y **comp**会提示您输入要使用的命令行选项。 如果未指定任何命令行选项**comp**使用之前指定的名称。

## <a name="BKMK_examples"></a>示例

若要比较的内容与备份目录的目录 C:\Reports \\ \\Sales\Backup\April，类型：
```
comp c:\reports \\sales\backup\april
```
若要比较 \Invoice 目录中文本文件的前十个行，并以十进制格式显示结果，请键入：
```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)