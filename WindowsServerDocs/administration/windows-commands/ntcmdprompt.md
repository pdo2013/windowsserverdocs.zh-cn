---
title: ntcmdprompt
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5fef1641bf1b48bd1fe4aaf284ed309ab4d4d5f1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372668"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

运行 "终止并保持驻留" （TSR）后，或在从 MS-DOS 应用程序中启动命令提示符之后运行命令解释器**cmd.exe**，而不是**Command.com**。
## <a name="syntax"></a>语法
```
ntcmdprompt
```
### <a name="parameters"></a>Parameters

| 参数 |             描述              |
|-----------|--------------------------------------|
|    /?     | 在命令提示符下显示帮助。 |

## <a name="remarks"></a>备注
- 当**Command.com**运行时， **cmd.exe**的某些功能（例如命令历史记录的**doskey**显示）将不可用。 如果你希望在启动终止和保持驻留（TSR）或从基于 MS-DOS 的应用程序中启动命令提示符之后运行**cmd.exe**命令解释器，则可以使用**ntcmdprompt**命令。 但请记住，在运行**cmd.exe**时 TSR 可能不可用。 可以在**配置**文件中包含**ntcmdprompt**命令，也可以在应用程序的程序信息文件（Pif）中包含等效的自定义启动文件。
  ## <a name="examples"></a>示例
  若要在**配置**文件中包含**ntcmdprompt** ，或在 Pif 中指定配置启动文件，请键入： **ntcmdprompt**
  ## <a name="additional-references"></a>其他参考
- [命令行语法项](command-line-syntax-key.md)

