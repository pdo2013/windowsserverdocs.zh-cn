---
title: goto
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e0de1458-1f78-48ff-a746-c285a945a510
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1ad0190519d58bd879ae391f378d800760c204f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857528"
---
# <a name="goto"></a>goto



中的批处理程序定向到带标签的行的 cmd.exe。 在批处理程序， **goto**将定向到由标志来标识行的命令处理。 当找到标签时，继续进行处理，开头的下一步的行开始的命令。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
goto <Label> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Label>|指定用作批处理程序中的标签的文本字符串。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   使用命令扩展

    如果命令扩展是启用 （默认值），并使用**goto**命令的目标标签 **: EOF**，将控件传输到当前的批处理脚本文件的末尾并退出批处理脚本文件而无需定义一个标签。 当你使用**goto**与 **: EOF**标签，必须插入一个冒号之前的标签。 例如：  
    ```
    goto:EOF
    ```  
-   使用有效*标签*值

    可以使用中的空格*标签*参数，但您不能包含其他分隔符 （例如，用分号或等号）。
-   匹配*标签*与批处理程序中的标签

    *标签*您指定的值必须匹配的批处理程序中的标签。 批处理程序中的标签必须以冒号 （:） 开头。 如果行以冒号开头，它将被视为一个标签，并忽略该行上的任何命令。 如果你的批处理程序不包含在指定的标签*标签*，批处理程序停止，并显示以下消息：  
    ```
    Label not found
    ```  
-   使用**goto**执行条件操作

    可以使用**goto**与其他命令执行条件操作。 有关使用详细信息**goto**条件的操作，请参阅[如果](if.md)命令参考。

## <a name="BKMK_examples"></a>示例

下面的批处理程序作为系统磁盘格式化驱动器 A 中的磁盘。 如果操作成功， **goto**命令将定向到处理 **： 结束**标签：
```
echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program. 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

[Cmd](cmd.md)

[If](if.md)