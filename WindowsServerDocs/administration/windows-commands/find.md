---
title: find
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ca66b22-3b7c-4166-8503-eb75fc53ab46
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b4639ff780687ad7a69ddba5374a722a15b06542
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848258"
---
# <a name="find"></a>find



搜索的字符串文本中的文件或文件，并显示包含指定的字符串的文本行。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
find [/v] [/c] [/n] [/i] [/off[line]] "<String>" [[<Drive>:][<Path>]<FileName>[...]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/v|显示不包含指定的所有行\<字符串 >。|
|/c|对包含指定的行进行计数\<字符串 >，并显示总数。|
|/n|置于每行开头的文件的行号。|
|i|指定搜索不区分大小写。|
|[/off[line]]|不跳过已设置了脱机属性的文件。|
|"\<字符串 >"|必需。 指定你想要搜索的字符 （括在引号中） 的组。|
|[\<Drive>:][<Path>]<FileName>|指定的位置和要在其中搜索指定字符串的文件的名称。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   指定的字符串

    如果不使用 **/i**，**查找**完全为指定的内容的搜索*字符串*。 例如，**查找**命令会将字符"a"和"A"以不同的方式。 如果您使用 **/i**，但**查找**不区分大小写，并且它将"a"和"A"作为相同字符。

    如果你想要搜索的字符串包含引号，则必须使用双引号引起来的字符串中包含每个引号 （例如，"此""字符串""包含引号"）。
-   使用**查找**作为筛选器

    如果省略文件名**查找**充当筛选器，从标准输入源 （通常是键盘、 竖线 (|) 或重定向的文件） 得到输入，然后显示包含任何行*字符串*。
-   排序命令语法

    您可以键入参数和命令行选项**查找**命令按任何顺序。
-   使用通配符

    不能使用通配符 (**&#42;** 并 **？**) 中文件的名称或使用指定的扩展**查找**命令。 若要搜索的一组使用通配符指定的文件中的字符串，可以使用**查找**命令内**为**命令。
-   使用 **/v**或 **/n**与 **/c**

    如果您使用 **/c**和 **/v**相同的命令行中，在**查找**显示不包含指定的字符串的行的计数。 如果指定 **/c**并 **/n**相同的命令行中，在**查找**忽略 **/n**。
-   使用**查找**回车符将返回

    **查找**命令无法识别的回车。 当你使用**查找**可搜索的文本包含回车文件中，您必须限制可以找到回车 （不太可能会返回一个回车符被中断的字符串） 之间的文本的搜索字符串。 例如，**查找**不会报告"税务文件"的字符串的匹配项，如果回车符后发生之间的单词"tax"和"文件。

## <a name="BKMK_examples"></a>示例

若要显示从 Pencil.ad 包含字符串"铅笔 Sharpener"的所有行，请键入：
```
find "Pencil Sharpener" pencil.ad
```
若要查找包含在引号内的文本的字符串，必须将整个字符串括在引号中。 则必须使用两个引号的字符串中包含每个引号。 若要查找"的科学家标记为他们的文章"为仅限讨论。" 它不是最终报表。" 在 Report.doc，键入：
```
find "The scientists labeled their paper ""for discussion only."" It is not a final report." report.doc
```
如果你想要搜索的一组文件，则可以使用**查找**命令内**为**命令。 若要查找当前目录中的文件扩展名为.bat 和，包含字符串"提示符"，键入：
```
for %f in (*.bat) do find "PROMPT" %f 
```
若要搜索硬盘以查找并显示文件名称包含字符串"CPU"的驱动器 C 上，使用竖线 (|) 将输出的定向**dir**命令**查找**命令，如下所示：
```
dir c:\ /s /b | find "CPU" 
```
因为**查找**搜索是区分大小写和**dir**生成大写输出，您必须以大写字母键入字符串"CPU"，或者可以使用 **/i**命令行选项与**查找**。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)