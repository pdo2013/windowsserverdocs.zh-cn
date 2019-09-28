---
title: prompt
description: 了解如何自定义命令提示符。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d98e965-02eb-46ad-9d0a-5dc44830373e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 2df80d3af6344644a68b1b2d01ba48fbf41f1581
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372017"
---
# <a name="prompt"></a>prompt



更改 Cmd.exe 命令提示符。 如果在没有参数的情况下使用，则**prompt**会将命令提示符重置为默认设置，该设置是当前驱动器号和目录后跟大于号（ **@no__t**）。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
prompt [<Text>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Text >|指定要包括在命令提示符中的文本和信息。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

你可以自定义命令提示符以显示你需要的任何文本，包括当前目录的名称、时间和日期以及 Microsoft Windows 版本号等信息。

下表列出了可以包含的字符组合，而不是*文本*参数中的一个或多个字符串。 此列表包括对每个字符组合添加到命令提示符的文本或信息的简短说明。  

| 字符 |                                 描述                                 |
|-----------|-----------------------------------------------------------------------------|
|    $q     |                               = （等号）                                |
|    $$     |                               $ （美元符号）                               |
|    $t     |                                当前时间                                 |
|    $d     |                                当前日期                                 |
|    $p     |                           当前驱动器和路径                            |
|    $v     |                           Windows 版本号                            |
|    $n     |                                当前驱动器                                |
|    $g     |                            > （大于号）                            |
|    $l     |                             < （小于号）                              |
|    $b     |                              \| （管道符号）                               |
|    $_     |                               回车-换行符                                |
|    $e     |                         ANSI 转义码（代码27）                          |
|    $h     | Backspace （删除已写入命令行的字符） |
|    $a     |                                & （与号）                                |
|    $c     |                            （（左括号）                             |
|    $f     |                            ）（右括号）                            |
|    $s     |                                    空间                                    |

启用命令扩展（即默认值）时， **prompt**命令支持以下格式字符：  

|字符|描述|
|---------|-----------|
|$+|零个或多个加号（ **+** ）个字符，具体取决于**pushd**目录堆栈的深度（每个推送级别一个字符）。|
|$m|与当前驱动器号关联的远程名称; 如果当前驱动器不是网络驱动器，则为空字符串。|

如果在 text 参数中包含 **$p**字符，则在输入每个命令后，将读取磁盘（以确定当前驱动器和路径）。 这可能需要额外的时间，特别是对于软盘驱动器。

## <a name="BKMK_examples"></a>示例

若要设置带有当前时间和日期的两行命令提示符，并在第一行上设置大于号，请键入：
```
prompt $d$s$s$t$_$g 
```
系统会按如下所示更改提示，其中日期和时间是最新的：
```
Fri 06/01/2007  13:53:28.91
>
```
若要将命令提示符设置为显示为箭头（`-->`），请键入：
```
prompt --$g
```
若要手动将命令提示符更改为默认设置（当前驱动器和路径后跟大于号），请键入：
```
prompt $p$g
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
