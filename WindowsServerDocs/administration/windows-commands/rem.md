---
title: rem
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2da0b6e42858582c1485659f3bf8f59e8e2ed97e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384574"
---
# <a name="rem"></a>rem



在批处理文件或配置中记录注释（备注）。系统. 如果未指定任何注释，则**rem**将增加垂直间距。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
rem [<Comment>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Comment >|指定要包含为注释的字符串。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **Rem**命令不在屏幕上显示注释。 你必须在批处理或配置中使用**echo on**命令。SYS 文件，用于在屏幕上显示注释。
-   不能在批处理文件注释中使用重定向字符（ **<** 或 **>** ）或管道（ **|** ）。
-   尽管可以使用不带注释的**rem**来向批处理文件添加垂直间距，但也可以使用空行。 处理批处理程序时，将忽略空白行。

## <a name="BKMK_examples"></a>示例

下面的示例演示一个批处理文件，该文件使用备注和垂直间距：
```
@echo off
rem  This batch program formats and checks new disks.
rem  It is named Checknew.bat.
rem
rem echo Insert new disk in Drive B.
pause 
format b: /v chkdsk b: 
```
在配置中的**prompt**命令之前包含解释性注释。SYS 文件中，将以下行添加到 CONFIG。系统
```
rem Set prompt to indicate current directory
prompt $p$g
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)