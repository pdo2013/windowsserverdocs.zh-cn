---
title: title
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42094e0f1231fee5ac9ef0ec9184ba685c8846b1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385795"
---
# <a name="title"></a>title



创建 "命令提示符" 窗口的标题。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
title [<String>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<String >|指定 "命令提示符" 窗口的标题。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   若要为批处理程序创建窗口标题，请在批处理程序的开头包含**title**命令。
-   设置窗口标题后，只能使用**title**命令对其进行重置。

## <a name="BKMK_examples"></a>示例

在下面的示例脚本中，命令提示符窗口的标题将更改为 "正在更新文件"，同时批处理文件执行**复制**命令。 执行命令后，将显示文本 `Files Updated`，并且命令提示符窗口的标题将更改回 "命令提示符"。
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)