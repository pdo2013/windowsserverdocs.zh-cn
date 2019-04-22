---
title: 列表
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57b6c8d0-872e-4dba-9715-1db8ab892e98
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aacc93e1c7a16a7327ddbd17515f19cf41a5b458
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825538"
---
# <a name="list"></a>列表



列出了编写器、 卷影副本或在系统上的当前已注册的卷影复制提供程序。 如果使用不带参数，**列表**在命令提示符下显示的帮助。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
list writers [metadata | detailed | status]
list shadows {all | set <SetID> | id <ShadowID>}
list providers
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|编写器|列出了编写器。 请参阅[List](list-writers.md)语法和参数。|
|阴影|列出了持久性和现有的非永久性卷影副本。 请参阅[列出阴影](list-shadows.md)语法和参数。|
|提供程序|列表当前注册的卷影复制提供程序。 请参阅[列出提供程序](list-providers.md)语法和参数。|

## <a name="BKMK_examples"></a>示例

若要列出所有卷影副本，请键入：
```
list shadows all
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)