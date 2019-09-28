---
title: comp
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 84604cea36b0b4c9543a7169002551c0da4f0493
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379265"
---
# <a name="comp"></a>comp



逐字节比较两个文件或文件集的内容。 如果在没有参数的情况下使用，则**comp**会提示你输入要比较的文件。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
comp [<Data1>] [<Data2>] [/d] [/a] [/l] [/n=<Number>] [/c]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Data1 >|指定要比较的第一个文件或一组文件的位置和名称。 您可以使用通配符（ **&#42;** 和 **？** ）来指定多个文件。|
|\<Data2 >|指定要比较的第二个文件或一组文件的位置和名称。 您可以使用通配符（ **&#42;** 和 **？** ）来指定多个文件。|
|/d|以十进制格式显示差异。 （默认格式为十六进制。）|
|/a|将差异显示为字符。|
|/l|显示出现差异的行号，而不是显示字节偏移量。|
|/n = \<Number >|仅比较为每个文件指定的行数，即使文件大小不同也是如此。|
|/c|执行不区分大小写的比较。|
|/off [line]|处理具有脱机属性集的文件。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **Comp**命令如何识别不匹配的信息

    在比较过程中，"**复合**" 将显示用于标识文件之间不等信息位置的消息。 每条消息指示不等字节的偏移内存地址和字节内容（除非指定了 **/a**或 **/d**命令行参数）。 消息按以下格式显示：

    `Compare error at OFFSET xxxxxxxx`

    `file1 = xx`

    `file2 = xx`

    十个比较不相等后， **comp**将停止比较文件并显示以下消息：

    `10 Mismatches - ending compare`
-   处理*Data1*和*Data2*的特殊情况  
    -   如果省略*Data1*或*data2*的必需组件，或如果忽略*Data2*，则会提示**你输入缺少**的信息。
    -   如果*Data1*只包含驱动器号或没有文件名的目录名称，则**comp**会将指定目录中的所有文件与*Data1*中指定的文件进行比较。
    -   如果*data2*只包含驱动器号或目录名称，则*data2*的默认文件名与*Data1*中的相同。
    -   如果**comp**找不到指定的文件，它会提示您确定是否要比较多个文件。
-   比较不同位置中的文件

    **Comp**可以比较相同驱动器或不同驱动器上的文件，也可以在同一目录或不同的目录中比较文件。 当**comp**比较文件时，会显示文件的位置和文件名。
-   比较具有相同名称的文件

    你比较的文件可以具有相同的文件名，前提是它们位于不同的目录或不同的驱动器上。 如果未指定*data2*的文件名，则*data2*的默认文件名与*Data1*中的文件名相同。 您可以使用通配符（ **&#42;** 和 **？** ）来指定文件名。
-   比较不同大小的文件

    您必须指定 **/n**以比较不同大小的文件。 如果文件大小不同，并且未指定 **/n** ，则**comp**将显示以下消息：

    `Files are different sizes`

    `Compare more files (Y/N)?`

    若要比较这些文件，请按 N 以停止**comp**命令。 然后，重新运行带有 **/n**选项的**comp**命令，以便仅比较每个文件的第一部分。
-   按顺序比较文件

    **&#42;** 如果使用通配符（和 **？** ）来指定多个文件，则**comp**会找到与*Data1*匹配的第一个文件，并将它与*Data2*中的相应文件（如果存在）进行比较。 **Comp**命令报告与*Data1*匹配的每个文件的比较结果。 完成后， **comp**显示以下消息：

    `Compare more files (Y/N)?`

    若要比较多个文件，请按 Y。**Comp**命令会提示你输入新文件的位置和名称。 若要停止比较，请按 N。按 Y 时，将**提示你输入要**使用的命令行选项。 如果未指定任何命令行选项，则**comp**将使用您之前指定的任何命令行选项。

## <a name="BKMK_examples"></a>示例

若要将目录 C:\Reports 的内容与备份目录进行比较 \\ @ no__t-1Sales\Backup\April，请键入：
```
comp c:\reports \\sales\backup\april
```
若要比较 \Invoice 目录中文本文件的前10行并以十进制格式显示结果，请键入：
```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)