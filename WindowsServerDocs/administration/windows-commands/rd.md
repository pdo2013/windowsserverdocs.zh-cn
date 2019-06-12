---
title: rd
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 42e672f6-5bc2-4c16-af25-18e7ed2dd555
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94231e3ec032280beb91a14db7949a1296c2d811
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442029"
---
# <a name="rd"></a>rd



删除的目录。 此命令等同于**rmdir**命令。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
rd [<Drive>:]<Path> [/s [/q]]
rmdir [<Drive>:]<Path> [/s [/q]]
```

## <a name="parameters"></a>Parameters

|     参数     |                                                                 描述                                                                  |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive>:]<Path> |                      指定的位置和你想要删除的目录的名称。 *路径*是必需的。                       |
|        /s         |                     删除目录树 （指定的目录和所有的子目录，其中包括所有文件）。                      |
|        /q         | 指定安静模式。 删除目录树时不提示确认。 (请注意， **/q** works 才 **/s**指定。) |
|        /?         |                                                     在命令提示符下显示帮助。                                                     |

## <a name="remarks"></a>备注

-   无法删除目录包含文件，其中包括隐藏文件或系统文件。 如果您尝试执行此操作，将显示以下消息：

    `The directory is not empty`

    使用**dir /a**命令以列出所有文件 (包括隐藏和系统文件)。 然后，使用**attrib**命令 **-h**中删除隐藏的文件属性 **-s**中删除系统文件属性，或 **-h-s**删除同时隐藏和系统文件属性。 后隐藏和文件属性已被删除，则可以删除的文件。
-   如果您插入一个反斜杠 (\)的开头*路径*，*路径*将开始 （而不考虑当前目录） 的根目录。
-   不能使用**rd**若要删除当前目录。 如果你尝试删除当前目录，将出现以下错误消息：

    `The process cannot access the file because it is being used by another process.`

    如果收到此错误消息，必须将更改为不同的目录 （不是子目录的当前目录），然后使用**rd** (指定*路径*如有必要)。
-   **Rd**命令，使用不同的参数，可从恢复控制台。

## <a name="BKMK_examples"></a>示例

不能删除当前使用的目录。 必须更改为不在当前目录内的目录。 例如，若要更改的父目录，请键入：
```
cd ..
```
现在，您可以安全地删除所需的目录。

使用 **/s**选项以删除目录树。 例如，若要删除一个目录命名为测试 （和所有其子目录和文件） 从当前目录，类型：
```
rd /s test
```
若要以无提示模式运行前面的示例，请键入：
```
rd /s /q test
```

> [!CAUTION]
> 在运行时**rd /s**在静默模式下，整个目录树不经确认直接删除。 确保重要的文件已移动或备份之前使用 **/q**命令行选项。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)