---
title: echo
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fb9fcd0f-5e73-4504-aa95-78204e1a79d3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eb5c9650b95703f1316e6f5f179b910d22574f68
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222963"
---
# <a name="echo"></a>echo



显示消息，或打开或关闭回显功能的命令。 如果使用不带参数， **echo**将显示当前回显设置。

有关如何使用此命令的示例，请参阅[示例](#examples)。

## <a name="syntax"></a>语法

```
echo [<Message>]
echo [on | off]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[上\|关闭]|打开或关闭回显功能的命令。 回显命令是在默认情况下。|
|\<消息 >|指定要在屏幕上显示的文本。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **Echo** *消息*时，命令将特别有用**echo**处于关闭状态。 若要显示的消息是多个行长而不显示任何命令，你可以包含多个**echo** *消息*命令后**关闭回显**命令，在你批处理程序。
-   当**echo**处于关闭状态，命令提示符将不会出现在命令提示符窗口中。 若要显示命令提示符处，键入**上回显。**
-   如果在批处理文件中，使用**上回显**并**回显关闭**不会影响在命令提示符下设置。
-   若要防止发送的批处理文件中的特定命令，将插入符号 (@) 命令的前面。 若要防止发送的批处理文件中的所有命令，包括**回显关闭**命令文件的开头。
-   若要显示一个管道 ( **|** ) 或重定向字符 (**<** 或**>**) 使用时**echo**，立即在管道或重定向字符之前使用插入符号 (^) (例如， **^|**， **^>**，或 **^<** ). 若要显示一个插入符号，连续键入两个插入符号 ( **^^** )。

## <a name="examples"></a>示例

若要显示当前**echo**设置中，键入：
```
echo
```
若要回显空行在屏幕上的，键入：
```
echo.
```

> [!NOTE]
> 期之前不包括空格。 否则，该期间将显示而不是一个空行。

若要防止回显命令在命令提示符处，键入：
```
echo off 
```

> [!NOTE]
> 当**echo**处于关闭状态，命令提示符将不会出现在命令提示符窗口中。 若要再次显示命令提示符处，键入**回响的**。

若要防止在批处理文件中的所有命令 (包括**回显关闭**命令) 显示在屏幕上，在批处理文件类型的第一行：
```
@echo off
```
可以使用**echo**命令的一部分**如果**语句。 例如，若要搜索扩展名.rpt 的文件，并回显一条消息的任何文件的当前目录，如果找到此类文件，请键入：
```
if exist *.rpt echo The report has arrived.
```
下面的批处理文件会搜索当前目录具有.txt 文件扩展名的文件，并显示一条消息，指示搜索的结果：
```
@echo off
if not exist *.txt (
echo This directory contains no text files.
) else (
   echo This directory contains the following text files:
   echo.
   dir /b *.txt
   )
```
如果运行批处理文件时，未不找到任何.txt 文件，会显示以下消息：
```
This directory contains no text files.
```
如果未找到.txt 文件的批处理文件运行时显示以下输出 （对于此示例中，假定 File1.txt 和 File2.txt、 File3.txt 存在的文件）：
```
This directory contains the following text files:
File1.txt
File2.txt
File3.txt
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
