---
title: break
description: 'Windows 命令主题**break_1** -设置或清除对 MS-DOS 系统的扩展 CTRL + C 检查。 如果在没有参数的情况下使用，则**break**将显示当前设置。 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c89b7357-d69e-4141-826e-73c9ba0fc630
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73afdac29efbfd9efec88d297cf4185ca1b92d62
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380487"
---
# <a name="break"></a>break



设置或清除对 MS-DOS 系统的扩展 CTRL + C 检查。 如果在没有参数的情况下使用，则**break**将显示当前设置。

> [!NOTE]
> 此命令已不再使用。 包括该命令仅用于保持与现有 MS-DOS 文件的兼容性，但由于该命令的功能是自动的，因此在命令行上不起作用。

## <a name="syntax"></a>语法

```
break=[on|off]
```

## <a name="remarks"></a>备注

如果命令扩展已在 Windows 平台上启用并运行，则在批处理文件中插入**break**命令将进入硬编码的断点，前提是调试器正在调试该断点。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)