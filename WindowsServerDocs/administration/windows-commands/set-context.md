---
title: 设置上下文
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 16f71d831f374f495abf2239cb8e694eee69efdf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370976"
---
# <a name="set-contex"></a>设置上下文



设置用于创建卷影副本的上下文。 如果使用时没有参数，则**设置上下文将**在命令提示符下显示帮助。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|clientaccessible|指定卷影副本可供 Windows 的客户端版本使用。|
|式|指定卷影副本在程序退出、重置或重新启动时保持不变。|
|失效|退出或重置时删除卷影副本。|
|nowriters|指定排除所有编写器。|

## <a name="remarks"></a>备注

-   默认情况下， *clientaccessible*上下文是永久性的。

## <a name="BKMK_examples"></a>示例

若要防止在退出 DiskShadow 时删除卷影副本，请键入：
```
set context persistent
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)