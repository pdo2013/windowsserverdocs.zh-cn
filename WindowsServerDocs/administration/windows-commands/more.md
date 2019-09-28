---
title: more
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ded14f6a-d82f-4aeb-a2d8-7ec1c94dfb8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/26/2019
ms.openlocfilehash: d505f99511d8702f11ac0c70edba3d62c8cf7996
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373915"
---
# <a name="more"></a>more



一次显示输出的一个屏幕。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
<Command> | more [/c] [/p] [/s] [/t<N>] [+<N>]
more [[/c] [/p] [/s] [/t<N>] [+<N>]] < [<Drive>:][<Path>]<FileName>
more [/c] [/p] [/s] [/t<N>] [+<N>] [<Files>]
```

## <a name="parameters"></a>Parameters

|           参数            |                               描述                               |
|--------------------------------|-------------------------------------------------------------------------|
|           \<Command >           |      指定要显示其输出的命令。      |
|               /c               |               在显示页面之前清除屏幕。               |
|               /p               |                      展开换页符。                      |
|               /s               |          以单个空行的形式显示多个空行。          |
|             /t @ no__t-0N >             |         将选项卡显示为*N*指定的空格数。         |
|             + @ NO__T-1N >              |     显示从*N*指定的行开始的第一个文件。     |
| [@no__t >：][@no__t >] \<FileName > |          指定要显示的文件的位置和名称。          |
|            \<Files >            | 指定要显示的文件的列表。 用空格分隔文件名。 |
|               /?               |                  在命令提示符下显示帮助。                   |

## <a name="remarks"></a>备注

-   在**更多**提示（`-- More --`）上接受以下子命令。 

    | Key | 操作 |
    | --- | ------ |
    | 空格键 | 显示下一页。 |
    | 进入 | 显示下一行。 |
    | f | 显示下一个文件。 |
    | q | 退出**更多**命令。 |
    | = | 显示行号。 |
    | p \<N > | 显示接下来的*N*行。 |
    | s \<N > |Kips 接下来的*N*行。 |
    | ? | 显示在 "**更多**" 提示符下可用的命令。| 
    
-   使用重定向字符（ **<** ）时，必须指定一个文件名作为源。 使用管道（ **\|** ）时，可以使用类似于**dir**、 **sort**和**type**的命令。
-   可从恢复控制台获取**更多**命令（具有不同的参数）。

## <a name="BKMK_examples"></a>示例

若要查看名为 "客户端" 的文件的第一个屏幕，请键入以下命令之一：
```
more < clients.new
type clients.new | more
```
"**更多**" 命令显示客户端中的第一个屏幕信息。新，然后显示以下提示：
```
-- More --
```
然后，可以按空格键查看下一个屏幕信息。

若要清除屏幕并在显示文件客户端之前删除所有额外的空白行，请键入以下命令之一：
```
more /c /s < clients.new
type clients.new | more /c /s
```
"**更多**" 命令显示客户端中的第一个屏幕信息。新，然后显示以下提示：
```
-- More --
```

### <a name="using-more-subcommands"></a>使用更多子命令

以下示例可用于**更多**提示（`-- More --`）。
- 若要每次显示一行文件，**请在命令提示符下**按 enter。
- 若要显示下一个屏幕，请在 **"提示" 下按**空格键。
- 若要显示命令行上列出的下一个文件，请在 "**更多**" 提示符下键入**f** 。
- 若要显示可用的命令，请键入 **？** **再**提示。
- 若要退出**详细信息**，请在**命令提示符处键入** **q** 。
- 若要显示当前行号，请在 "**更多**" 提示符下键入 **=** 。 将当前行号添加到**更多**提示，如下所示：  
  ```
  -- More [Line: 24] --
  ```  
- 若要显示特定数量的行，**请在 "提示"** 中键入**p** 。 还**会提示你输入要**显示的行数，如下所示：  
  ```
  -- More -- Lines:
  ```  
  键入要显示的行数，然后按 ENTER。 **显示指定**的行数。
- 若要跳过特定数量的行，请在 "**更多**" 提示符下键入**s** 。 **更多**提示您输入要跳过的行数，如下所示：  
  ```
  -- More -- Lines:
  ```  
  键入要跳过的行数，然后按 ENTER。 **更多**跳过指定的行数，并显示下一屏信息。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
