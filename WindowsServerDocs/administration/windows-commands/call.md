---
title: 调用
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d34a41dc-e6c7-4467-bf6a-15cec704833e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 0e5f9f2b0102c12ee0925bb434fdeddde85e34cd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379716"
---
# <a name="call"></a>调用



从一个批处理程序调用另一个批处理程序，而不停止父批处理程序。 **Call**命令接受标签作为调用的目标。

> [!NOTE]
> 当在脚本或批处理文件外使用时，**调用**在命令提示符下不起作用。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
call [Drive:][Path]<FileName> [<BatchParameters>] [:<Label> [<Arguments>]]
```

## <a name="parameters"></a>Parameters

|           参数           |                                                                         描述                                                                          |
|-------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [@no__t >：][\<Path >] <FileName> | 指定要调用的批处理程序的位置和名称。 *FileName*参数是必需的，它必须具有 .bat 或 .cmd 扩展名。 |
|      \<BatchParameters >       |                                            指定批处理程序所需的任何命令行信息。                                             |
|           ： \<Label >           |                                            指定您希望批处理程序控件跳转到的标签。                                             |
|         \<Arguments >          |                     指定要传递给批处理程序的新实例的命令行信息，从 *：标签开始。*                     |
|              /?               |                                                             在命令提示符下显示帮助。                                                             |

## <a name="batch-parameters"></a>批处理参数

下表列出了批处理脚本参数引用（ **% 0**， **% 1**...）。

批处理脚本中 **% @ no__t**引用所有参数（例如， **% 1**、 **% 2**、 **% 3**...）

你可以使用以下可选语法作为批处理参数（ **% n**）的替换：

|批处理参数|描述|
|---------------|-----------|
|% ~ 1|展开 **% 1**并删除周围的引号（""）。|
|% ~ f1|将 **% 1**扩展到完全限定的路径。|
|% ~ d1|仅将 **% 1**扩展到驱动器号。|
|% ~ p1|仅将 **% 1**展开为路径。|
|% ~ n1|仅将 **% 1**扩展到文件名。|
|% ~ x1|仅将 **% 1**扩展到文件扩展名。|
|% ~ s1|将 **% 1**扩展到仅包含短名称的完全限定路径。|
|% ~ a1|将 **% 1**扩展到文件属性。|
|% ~ t1|将 **% 1**扩展到文件的日期和时间。|
|% ~ z1|将 **% 1**扩展到文件的大小。|
|% ~ $PATH：1|搜索 PATH 环境变量中列出的目录，并将 **% 1**扩展到找到的第一个目录的完全限定名称。 如果未定义环境变量名称或搜索找不到该文件，则此修饰符将扩展为空字符串。|

下表显示了如何将修饰符与复合结果的批处理参数合并：

|带有修饰符的批处理参数|描述|
|-----------------------------|-----------|
|% ~ sjc-dp1|将 **% 1**扩展到驱动器号和路径。|
|% ~ nx1|仅将 **% 1**扩展到文件名和扩展名。|
|% ~ dp $ PATH：1|搜索 **% 1**的 PATH 环境变量中列出的目录，然后扩展到找到的第一个目录的驱动器号和路径。|
|% ~ ftza1|展开 **% 1**以显示类似于**dir**命令的输出。|

在上面的示例中， **% 1**和 PATH 可替换为其他有效值。 <strong>@No__t-1</strong>语法由有效参数号终止。 <strong>@No__t-1</strong>修饰符不能与 **% @ no__t; no__t-5**一起使用。

## <a name="remarks"></a>备注

-   使用批处理参数

    批处理参数可以包含可传递给批处理程序的任何信息，包括命令行选项、文件名、批处理参数 **% 0**到 **% 9**和变量（例如， **% 波特%** ）。
-   使用*Label*参数

    通过对*标签*参数使用**call** ，可以创建新的批处理文件上下文，并将控制传递给指定标签后的语句。 第一次遇到批处理文件的末尾时（即，跳转到标签之后），控制将返回到**call**语句之后的语句。 第二次遇到批处理文件的末尾时，批处理脚本将退出。
-   使用管道和重定向符号

    不要将管道（ **|** ）和重定向符号（ **<** 或 **>** ）用于**调用**。
-   进行递归调用

    可以创建调用自身的批处理程序。 但是，必须提供退出条件。 否则，父批处理程序和子批处理程序可以无限循环。
-   使用命令扩展

    如果启用了命令扩展，则**调用**接受*标签*作为调用的目标。 正确的语法如下所示：

    `call :\<Label> <Arguments>`

## <a name="BKMK_examples"></a>示例

若要从另一个批处理程序运行 Checknew 程序，请在父批处理程序中键入以下命令：
```
call checknew
```
如果父批处理程序接受两个批处理参数并且您希望将这些参数传递给 Checknew，请在父批处理程序中键入以下命令：
```
call checknew %1 %2
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
