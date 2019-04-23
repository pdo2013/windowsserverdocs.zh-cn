---
title: verify
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dfe8bc91-d948-4e47-84ad-a79a60506ffa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5e99237c2bac93625dedec0254c274e4f8dbc9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827248"
---
# <a name="verify"></a>verify



告知**cmd**是否要验证你的文件正确写入到磁盘。 如果使用不带参数，**验证**显示当前设置。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
verify [on | off]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[上\|关闭]|开关**验证**设置打开或关闭。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_examples"></a>示例

若要显示当前**验证**设置中，键入：
```
verify
```
若要打开**验证**设置上，键入：
```
Verify on
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)