---
title: ntcmdprompt
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 583f56c294e66542a75efca09e97d57ae54a8cea
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436422"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

运行命令解释器**Cmd.exe**，而非**Command.com**前后运行的是终止和保持居民 (TSR) 或启动命令提示符下从 MS-DOS 应用程序中后。
## <a name="syntax"></a>语法
```
ntcmdprompt
```
### <a name="parameters"></a>Parameters

| 参数 |             描述              |
|-----------|--------------------------------------|
|    /?     | 在命令提示符下显示帮助。 |

## <a name="remarks"></a>备注
- 当**Command.com**正在运行的某些功能**Cmd.exe**，如**doskey**显示的命令历史记录，请将不可用。 如果你想要运行**Cmd.exe**命令解释器启动终止和保持居民 (TSR) 或启动命令提示符下从基于 MS-DOS 的应用程序中之后，可以使用**ntcmdprompt**命令。 但是，请记住要在运行时不可能可供使用 TSR **Cmd.exe**。 可以包括**ntcmdprompt**命令，在您**Config.nt**文件或应用程序的程序信息文件 (Pif) 中的等效的自定义启动文件。
  ## <a name="examples"></a>示例
  若要包括**ntcmdprompt**在你**Config.nt**文件或 Pif，类型中指定的配置启动文件： **ntcmdprompt**
  ## <a name="additional-references"></a>其他参考
- [命令行语法项](command-line-syntax-key.md)

