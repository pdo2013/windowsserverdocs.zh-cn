---
title: setlocal
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4e4b6d3-3f1a-4851-a782-25ee2470e16e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 70e58e3c3a7c3de594c620f7530816b57727d4c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868858"
---
# <a name="setlocal"></a>setlocal



启动批处理文件中的环境变量的本地化。 本地化将继续，直到匹配**endlocal**遇到命令或批处理文件结束。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
setlocal [enableextensions | disableextensions] [enabledelayedexpansion | disabledelayedexpansion]
```

## <a name="arguments"></a>参数

|参数|描述|
|--------|-----------|
|enableextensions|使命令扩展，直至进行匹配**endlocal**遇到命令，而不考虑之前设置**setlocal**运行命令。|
|disableextensions|禁用命令扩展，直至进行匹配**endlocal**遇到命令，而不考虑之前设置**setlocal**运行命令。|
|enabledelayedexpansion|启用延迟的环境变量扩展直到匹配**endlocal**遇到命令，而不考虑之前设置**setlocal**运行命令。|
|disabledelayedexpansion|禁用延迟的环境变量扩展直到匹配**endlocal**遇到命令，而不考虑之前设置**setlocal**运行命令。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   使用**setlocal**

    当你使用**setlocal**之外的脚本或批处理文件，它不起作用。
-   更改环境变量

    使用**setlocal**来运行批处理文件时更改环境变量。 在运行后所做的环境更改**setlocal**是本地的批处理文件。 Cmd.exe 程序还原以前的设置，当它遇到**endlocal**命令或达到批处理文件的末尾。
-   嵌套命令

    您可以有多个**setlocal**或**endlocal**命令中的批处理程序 （即，嵌套命令）。
-   批处理文件中的命令扩展测试

    **Setlocal**命令将设置 ERRORLEVEL 变量。 如果您通过 {**enableextensions** | **disableextensions**} 或 {**enabledelayedexpansion**  |  **disabledelayedexpansion**}，ERRORLEVEL 变量设置为**0** （零）。 否则，设置为**1**。 可以在批处理脚本中使用此信息来确定是否提供了扩展，如下面的示例中所示：  
    ```
    setlocal enableextensions
    verify other 2>nul
    if errorlevel 1 echo Unable to enable extensions
    ```  
    因为**cmd**禁用命令扩展后，未设置 ERRORLEVEL 变量**验证**命令将 ERRORLEVEL 变量初始化为一个非零值时使用了无效自变量。 此外，如果您使用**setlocal**命令和参数 {**enableextensions** | **disableextensions**} 或 {**enabledelayedexpansion**  |  **disabledelayedexpansion**} 和不设置 ERRORLEVEL 变量**1**，命令扩展不可用。

## <a name="BKMK_examples"></a>示例

可以本地化批处理文件中的环境变量，如下面的示例脚本中所示：
```
rem *******Begin Comment**************
rem This program starts the superapp batch program on the network,
rem directs the output to a file, and displays the file
rem in Notepad.
rem *******End Comment**************
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)