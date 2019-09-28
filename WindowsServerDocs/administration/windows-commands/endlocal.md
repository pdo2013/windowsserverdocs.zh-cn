---
title: endlocal
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 16d2b7b445a2220a10f88f21029948ed10ee96e4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377575"
---
# <a name="endlocal"></a>endlocal



结束批处理文件中环境更改的本地化，并在运行相应的**setlocal**命令之前将环境变量还原到其值。

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

-   **Endlocal**命令在脚本或批处理文件外不起作用。
-   批处理文件的末尾有一个隐式**endlocal**命令。
-   如果启用了命令扩展（默认情况下启用命令扩展）， **endlocal**命令会将命令扩展（即启用或禁用）的状态还原为运行相应的**setlocal**命令之前的状态。

> [!NOTE]
> 有关启用和禁用命令扩展的详细信息，请参阅[Cmd](cmd.md)。

## <a name="BKMK_examples"></a>示例

可以在批处理文件中本地化环境变量。 例如，以下程序启动网络上的 superapp 批处理程序，将输出定向到某个文件，并在记事本中显示该文件：
```
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)