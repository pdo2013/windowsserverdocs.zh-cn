---
title: rem
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 85c8a69bf21a386cd36e45bbca6dacd35aef2509
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846998"
---
# <a name="rem"></a>rem



批处理文件或配置中的记录注释 （备注)。SYS。 如果指定无注释，则**rem**添加垂直间距。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
rem [<Comment>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<注释 >|指定要包括为注释的字符的字符串。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **Rem**命令不显示在屏幕上的注释。 必须使用**回响的**命令，在批处理或配置。若要在屏幕上显示注释的 SYS 文件。
-   不能使用重定向字符 (**<** 或**>**) 或管道 (**|**) 批处理文件注释中。
-   尽管可以使用**rem**而无需将垂直间距添加到批处理文件的注释，也可以使用空行。 处理的批处理程序时，将忽略空行。

## <a name="BKMK_examples"></a>示例

下面的示例显示了使用备注的注释和垂直间距的批处理文件：
```
@echo off
rem  This batch program formats and checks new disks.
rem  It is named Checknew.bat.
rem
rem echo Insert new disk in Drive B.
pause 
format b: /v chkdsk b: 
```
若要包括在之前的解释性备注**提示符**命令配置中。SYS 文件中，将以下行添加到配置。SYS:
```
rem Set prompt to indicate current directory
prompt $p$g
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)