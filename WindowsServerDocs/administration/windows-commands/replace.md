---
title: replace
description: 了解如何使用替换命令来替换文件。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6143661e-d90f-4812-b265-6669b567dd1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: bce4622983271bde06614ebc04418871490cff91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818888"
---
# <a name="replace"></a>replace



替换文件。 如果用于 **/a**选项，**替换为**而不是替换现有文件目录中添加新文件。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/a] [/p] [/r] [/w] 
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/p] [/r] [/s] [/w] [/u] 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[\<Drive1>:][\<Path1>]\<FileName>|指定的位置和源文件的名称或一组文件。 *文件名*是必需的并且可以包括通配符 (**&#42;** 并 **？**)。|
|[\<Drive2>:][\<Path2>]|指定目标文件的位置。 不能指定该文件的文件替换名称。 如果未指定驱动器或路径，**替换为**作为目标将使用当前驱动器和目录。|
|/a|将新文件添加到目标目录，而不是替换现有文件。 不能使用此命令行选项与 **/s**或 **/u**命令行选项。|
|/p|将提示您在替换的目标文件或添加源文件前进行确认。|
|/r|替代只读的和未受保护文件。 如果在尝试更换一个只读的文件，但不是指定 **/r**，发生错误并停止替换操作。|
|/w|您可以插入一张磁盘，源文件搜索开始之前等待。 如果未指定 **/w**，**替换为**按 ENTER 后立即开始，并替换或添加文件。|
|/s|目标目录中搜索所有子目录和替换匹配的文件。 不能使用 **/s**与 **/a**命令行选项。 **替换为**命令不会搜索中指定的子目录*Path1*。|
|/u|替换只是那些早于源目录中的目标目录文件。 不能使用 **/u**与 **/a**命令行选项。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   作为**替换为**添加或替换文件，名称显示在屏幕的文件。 之后**替换为**已完成，摘要行显示采用以下格式之一：  
    ```
    nnn files added
    nnn files replaced
    no file added
    no file replaced
    ```  
-   如果您正在使用软盘并需要切换期间磁盘**替换为**操作，可以指定 **/w**命令行选项，以便**替换**将等待你为切换磁盘。
-   不能使用**替换为**更新隐藏的文件或系统文件。
-   下表显示了每个退出代码以及其含义的简要描述：  
    |退出代码|描述|
    |---------|-----------|
    |0|**替换为**命令已成功替换或添加文件。|
    |1|**替换为**命令遇到的 MS-DOS 版本不正确。|
    |2|**替换为**命令找不到源文件。|
    |3|**替换为**命令找不到源或目标路径。|
    |5|用户没有访问你想要替换的文件。|
    |8|没有足够的系统内存来执行的命令。|
    |11|用户在命令行上使用错误的语法。|

> [!NOTE]
> 可以在使用 ERRORLEVEL 参数**如果**中的批处理程序进程退出代码，以返回到命令行**替换为**。

## <a name="BKMK_examples"></a>示例

若要更新的名 Phones.cli （它们会出现在多个目录位于驱动器 C 上），为软盘驱动器 A 中，从 Phones.cli 文件的最新版本的文件的所有版本键入：

`replace a:\phones.cli c:\ /s`

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)