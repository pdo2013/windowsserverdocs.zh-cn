---
title: title
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d1d1ea70849c3beb4503edfdaa5116384c14a2fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848498"
---
# <a name="title"></a>title



创建命令提示符窗口的标题。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
title [<String>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<String>|指定命令提示符窗口的标题。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   若要创建的批处理程序窗口标题，包括**标题**命令的批处理程序开头。
-   设置窗口标题后，您可以仅通过使用重置**标题**命令。

## <a name="BKMK_examples"></a>示例

下面的示例脚本，在命令提示符窗口的标题已更改为"更新文件"的批处理文件执行的同时**复制**命令。 执行此命令后，文本`Files Updated`显示，并在命令提示符窗口的标题更改回"命令提示符"。
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)