---
title: echo
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 343d6327d262401b4be14e472a135062456890f1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377632"
---
# <a name="echo"></a>echo



显示消息或打开或关闭命令回显功能。 如果不使用参数， **echo**将显示当前的回显设置。

有关如何使用此命令的示例，请参阅[示例](#examples)。

## <a name="syntax"></a>语法

```
echo [<Message>]
echo [on | off]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[\| off]|打开或关闭命令回显功能。 默认情况下，命令回显处于启用状态。|
|\<Message >|指定要在屏幕上显示的文本。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   当**回响**关闭时， **echo** *Message*命令特别有用。 若要在不显示任何命令的情况下显示长达几行的消息，可以在批处理程序中的 " **echo off** " 命令后包含多个**echo** *消息*命令。
-   关闭**回响**后，命令提示符将不会显示在命令提示符窗口中。 若要显示命令提示符，请**在上键入 echo。**
-   如果在批处理文件中使用，**回显开启**和**关闭**会影响命令提示符下的设置。
-   若要防止在批处理文件中回显特定命令，请在命令前面插入一个 at 符号（@）。 若要防止在批处理文件中回显所有命令，请在文件开头包含**回响 off**命令。
-   若要在使用**echo**时显示管道（ **@no__t 1**）或重定向字符（ **@no__t 3**或 **>** ），请在管道或重定向字符之前立即使用插入符号（^）（例如， **^|** ， **|0**或**2**）。 若要显示插入符号，请连续键入两个插入符号（ **^^** ）。

## <a name="examples"></a>示例

若要显示当前**echo**设置，请键入：

```
echo
```

若要在屏幕上回显空白行，请键入：

```
echo.
```

> [!NOTE]
> 句点之前不能包含空格。 否则，将显示句点而不是空行。

若要在命令提示符处阻止回显命令，请键入：

```
echo off 
```

> [!NOTE]
> 关闭**回响**后，命令提示符将不会显示在命令提示符窗口中。 若要再次显示命令提示符，请**在上键入 echo**。

若要防止批处理文件中的所有命令（包括 "**回响 off** " 命令）显示在屏幕上，请在批处理文件类型的第一行：

```
@echo off
```

可以使用**echo**命令作为**if**语句的一部分。 例如，若要在当前目录中搜索文件扩展名为 rpt 的任何文件，并在找到此类文件时回显消息，请键入：

```
if exist *.rpt echo The report has arrived.
```

以下批处理文件将在当前目录中搜索 .txt 文件扩展名的文件，并显示一条消息，指出搜索结果：

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

如果在批处理文件运行时找不到 .txt 文件，将显示以下消息：

```
This directory contains no text files.
```

如果在运行批处理文件时找到 .txt 文件，则会显示以下输出（在本示例中，假定文件为 .txt、File2 和 File3）：

```
This directory contains the following text files:
File1.txt
File2.txt
File3.txt
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
