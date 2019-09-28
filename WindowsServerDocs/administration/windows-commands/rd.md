---
title: rd
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 029935bcd8773e41adefcd6ca916d75edcea3065
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371808"
---
# <a name="rd"></a>rd



删除目录。 此命令与**rmdir**命令相同。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
rd [<Drive>:]<Path> [/s [/q]]
rmdir [<Drive>:]<Path> [/s [/q]]
```

## <a name="parameters"></a>Parameters

|     参数     |                                                                 描述                                                                  |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| [@no__t >：] <Path> |                      指定要删除的目录的位置和名称。 *路径*是必需的。                       |
|        /s         |                     删除目录树（指定的目录及其所有子目录，包括所有文件）。                      |
|        /q         | 指定安静模式。 删除目录树时不提示进行确认。 （请注意， **/q**仅在指定 **/s**时才起作用。） |
|        /?         |                                                     在命令提示符下显示帮助。                                                     |

## <a name="remarks"></a>备注

-   不能删除包含文件（包括隐藏文件或系统文件）的目录。 如果尝试这样做，将显示以下消息：

    `The directory is not empty`

    使用**dir/a**命令列出所有文件（包括隐藏文件和系统文件）。 然后，使用带有 **-h**的**attrib**命令删除隐藏的文件属性，使用- **s**删除系统文件属性，或使用 **-h-s**删除隐藏文件和系统文件属性。 删除隐藏属性和文件属性后，可以删除这些文件。
-   如果在*路径*开头插入反斜杠（@no__t 0，*路径*将从根目录开始（无论当前目录如何）。
-   你无法使用**rd**删除当前目录。 如果尝试删除当前目录，将显示以下错误消息：

    `The process cannot access the file because it is being used by another process.`

    如果收到此错误消息，则必须更改为其他目录（而不是当前目录的子目录），然后使用**rd** （如有必要，请指定*路径*）。
-   可从恢复控制台获取带有不同参数的**rd**命令。

## <a name="BKMK_examples"></a>示例

不能删除当前正在使用的目录。 必须更改为不在当前目录中的目录。 例如，要更改为父目录，请键入：
```
cd ..
```
现在，可以安全地删除所需的目录。

使用 **/s**选项来删除目录树。 例如，若要从当前目录中删除名为 Test 的目录及其所有子目录和文件，请键入：
```
rd /s test
```
若要在安静模式下运行前面的示例，请键入：
```
rd /s /q test
```

> [!CAUTION]
> 在安静模式下运行**rd/s**时，将删除整个目录树而不进行确认。 请确保在使用 **/q**命令行选项之前移动或备份重要文件。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)