---
title: exec
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ecdfd05b8abefb35946b783daaa3220a6713a38d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882918"
---
# <a name="exec"></a>exec



执行本地计算机上的文件。 该文件可以是**cmd**脚本。

## <a name="syntax"></a>语法

```
exec <ScriptFile.cmd>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<ScriptFile.cmd>|指定要执行的脚本文件。|

## <a name="remarks"></a>备注

-   此命令用于复制或还原的备份一部分的数据或还原顺序。
-   如果脚本失败，返回错误，DiskShadow 退出。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)