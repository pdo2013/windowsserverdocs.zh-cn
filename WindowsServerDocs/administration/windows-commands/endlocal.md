---
title: endlocal
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 765fae3c-0c0a-4639-99a4-cf613489b949
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e516b2bf9e8a45ada910dfbd93e3ed5e7d86c14
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862138"
---
# <a name="endlocal"></a>endlocal



结束本地化环境中的更改的批处理文件，并将环境变量为其值还原之前相对应**setlocal**运行命令。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
endlocal
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **Endlocal**命令不起作用外部脚本或批处理文件。
-   没有一种隐式**endlocal**命令末尾的批处理文件。
-   如果启用了命令扩展 （命令扩展会启用默认情况下）， **endlocal**命令将命令扩展 （即，启用或禁用） 的状态还原到前的相应**setlocal**运行命令。

> [!NOTE]
> 有关启用和禁用命令扩展的详细信息，请参阅[Cmd](cmd.md)。

## <a name="BKMK_examples"></a>示例

可以本地化的批处理文件中的环境变量。 例如，以下程序在网络上启动 superapp 批处理程序、 将输出定向到一个文件，并在记事本中显示的文件：
```
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)