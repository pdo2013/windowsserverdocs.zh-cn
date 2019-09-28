---
title: verify
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 840fd3609ed3aded1c9cfebd4e395ddcc6d5588b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363098"
---
# <a name="verify"></a>verify



告诉**cmd**是否验证文件是否已正确写入磁盘。 如果不使用参数，则**验证**将显示当前设置。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
verify [on | off]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[\| off]|打开或关闭**验证**设置。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_examples"></a>示例

若要显示当前**验证**设置，请键入：
```
verify
```
若要启用**验证**设置，请键入：
```
Verify on
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)