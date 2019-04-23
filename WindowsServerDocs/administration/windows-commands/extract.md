---
title: extract
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9113c34b61b98fb738bc0aff03193ab73b1abbd7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882308"
---
# <a name="extract"></a>extract



## <a name="syntax"></a>语法

```
EXTRACT [/Y] [/A] [/D | /E] [/L dir] cabinet [filename ...]
EXTRACT [/Y] source [newname]
EXTRACT [/Y] /C source destination
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|cab 文件|文件包含两个或多个文件。|
|filename|要从该 cab 文件中提取的文件的名称。 可以使用通配符和多个文件名 （由空格分隔）。|
|源|压缩的文件 （只有一个文件是 cabinet 文件）。|
|新名称|新的文件名以便提取的文件。 如果未提供，则使用原始名称。|
|/A|处理所有的 cabinet 文件。 遵循 cabinet 链中所述的第一个 cab 文件开始。|
|/C|将源文件复制到目标 （若要从 DMF 磁盘复制）。|
|/D|显示 cab 文件的目录 （与文件名以避免提取一起使用）。|
|/E|提取 (而不是使用 *。* 若要提取的所有文件）。|
|/L dir|提取的文件 （默认值为当前目录） 的放置位置。|
|/Y|覆盖现有文件之前进行提示。|

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)