---
title: exec
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 514503e4920e16ba6778185af32f925541805223
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377431"
---
# <a name="exec"></a>exec



在本地计算机上执行文件。 该文件可以是**cmd**脚本。

## <a name="syntax"></a>语法

```
exec <ScriptFile.cmd>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<ScriptFile >|指定要执行的脚本文件。|

## <a name="remarks"></a>备注

-   此命令用于在备份或还原顺序中复制或还原数据。
-   如果脚本失败，将返回错误，并且 DiskShadow 将退出。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)