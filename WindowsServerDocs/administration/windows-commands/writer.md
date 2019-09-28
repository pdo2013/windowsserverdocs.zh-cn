---
title: 编写器
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c00f6067cd5f6cf741cddbd6d62c5bcbb1f37a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361856"
---
# <a name="writer"></a>编写器



验证是否包括了写入器或组件，或者是否从备份或还原过程中排除了写入器或组件。 如果在没有参数的情况下使用，则**编写器**会在命令提示符下显示帮助。

## <a name="syntax"></a>语法

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

## <a name="parameters"></a>Parameters

| 参数  |                                                                                      描述                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   verify   | 验证指定的编写器或组件是否包含在备份或还原过程中。 如果未包括写入器或组件，备份或还原过程将失败。 |
|  exclude   |                                                   从备份或还原过程中排除指定的编写器或组件。                                                    |
| [\<Writer > |                                                                                     <Component>]                                                                                      |

## <a name="BKMK_examples"></a>示例

若要通过指定写入器的 GUID 来验证写入器（对于此示例，为4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f），请键入：
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
若要排除名称为 "System Writer" 的编写器，请键入：
```
writer exclude "System Writer"
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)