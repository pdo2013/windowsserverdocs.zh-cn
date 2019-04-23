---
title: more
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ded14f6a-d82f-4aeb-a2d8-7ec1c94dfb8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d4957f9455c6caed027331a939db0c2fefbe1961
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857568"
---
# <a name="more"></a>more



一次显示一个屏幕的输出。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
<Command> | more [/c] [/p] [/s] [/t<N>] [+<N>]
more [[/c] [/p] [/s] [/t<N>] [+<N>]] < [<Drive>:][<Path>]<FileName>
more [/c] [/p] [/s] [/t<N>] [+<N>] [<Files>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<命令 >|指定你想要显示的输出的命令。|
|/c|显示一个页面之前清除屏幕。|
|/p|将扩展窗体源字符。|
|/s|作为单个空白行显示多个空行。|
|/t\<N>|为指定的空格数显示选项卡*N*。|
|+\<N>|在指定的行中显示文件开头的第一个*N*。|
|[\<Drive>:] [<Path>]<FileName>|指定的位置和要显示的文件的名称。|
|\<文件 >|指定要显示文件的列表。 请用空格分隔文件的名称。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   在以下子命令会接受**更多**提示符 (`-- More --`)。  
    |键|操作|
    |---|------|
    |空格键|显示下一个页面。|
    |进入|显示下一步的行。|
    |f|显示下一个文件。|
    |q|退出**详细**命令。|
    |=|显示行号。|
    |p \<N>|显示下一步*N*行。|
    |s \<N>|将跳过下一步*N*行。|
    |?|将显示在可用的命令**详细**提示符。|
-当使用重定向字符 (**<**)，您必须指定文件名作为源。 使用管道时 (* *|*），您可以使用此类命令作为**dir**，**排序**，并**类型**。
-   **详细**命令，使用不同的参数，可从恢复控制台。

## <a name="BKMK_examples"></a>示例

若要查看名为 Clients.new 文件的信息的第一个屏幕，请键入以下命令之一：
```
more < clients.new
type clients.new | more
```
**详细**命令显示 Clients.new 中的信息的第一个屏幕，然后显示以下提示：
```
-- More --
```
然后可以按空格键后，若要查看的信息的下一个屏幕。

若要清除屏幕并显示文件 Clients.new 之前删除所有额外的空白的行，键入以下命令之一：
```
more /c /s < clients.new
type clients.new | more /c /s
```
**详细**命令显示 Clients.new 中的信息的第一个屏幕，然后显示以下提示：
```
-- More --
```

### <a name="using-more-subcommands"></a>使用更多的子命令

下面的示例可用于在**更多**提示符 (`-- More --`)。
-   若要一次显示文件的一行，按 ENTER**详细**提示符。
-   若要显示的下一个屏幕，请按在空格键**详细**提示符。
-   若要显示在命令行上列出的下一个文件，请键入**f**处**详细**提示符。
-   若要显示可用命令，请键入 **？** 在**详细**提示符。
-   若要退出**更多**，类型**q**处**详细**提示符。
-   若要显示当前行号，请键入**=** 处**详细**提示符。 当前行号添加到**详细**提示，如下所示：  
    ```
    -- More [Line: 24] --
    ```  
-   若要显示特定数目的行，请键入**p**处**详细**提示符。 **更多**将提示您输入要显示，如下所示的行数：  
    ```
    -- More -- Lines:
    ```  
    键入要显示的行数，然后按 ENTER。 **更多**显示指定的行数。
-   若要跳过指定的行数，请键入**s**处**详细**提示符。 **更多**将提示您输入要跳过，如下所示的行数：  
    ```
    -- More -- Lines:
    ```  
    键入要跳过的行数，然后按 ENTER。 **更多**跳过指定的行数并显示信息的下一个屏幕。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)