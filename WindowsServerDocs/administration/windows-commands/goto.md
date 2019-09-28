---
title: goto
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1caf3da3e8b873150af5be7ed8316cfcb526db83
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375690"
---
# <a name="goto"></a>goto



将 cmd.exe 定向到批处理程序中带标签的行。 在批处理程序中， **goto**将命令处理定向到由标签标识的行。 找到标签后，处理将从下一行开始的命令开始。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
goto <Label> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Label >|指定一个文本字符串，该字符串用作批处理程序中的标签。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   使用命令扩展

    如果启用了命令扩展（默认设置），并使用目标标签为 **： EOF**的**goto**命令，则可以将控制转移到当前批处理脚本文件的末尾，并退出批处理脚本文件而不定义标签。 将**goto**与 **： EOF**标签一起使用时，必须在标签之前插入一个冒号。 例如：  
    ```
    goto:EOF
    ```  
-   使用有效的*标签*值

    可以在*标签*参数中使用空格，但不能包含其他分隔符（例如，分号或等号）。
-   与批处理程序中的标签匹配的*标签*

    指定的*标签*值必须与批处理程序中的标签相匹配。 批处理程序内的标签必须以冒号（:) 开头。 如果行以冒号开头，则将其视为标签，并忽略该行上的所有命令。 如果批处理程序不包含在*标签*中指定的标签，批处理程序将停止并显示以下消息：  
    ```
    Label not found
    ```  
-   使用**goto**进行条件运算

    可以将**goto**与其他命令结合使用来执行条件操作。 有关对条件运算使用**goto**的详细信息，请参阅[If](if.md)命令参考。

## <a name="BKMK_examples"></a>示例

以下批处理程序将驱动器 A 中的磁盘格式化为系统磁盘。 如果操作成功， **goto**命令会将处理定向到 **：结束**标签：
```
echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program. 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

[Cmd](cmd.md)

[如果](if.md)