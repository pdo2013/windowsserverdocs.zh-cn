---
title: 编写器
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8aee4ecca85c7d5f46ee79f3ad928b746c02e7bb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439985"
---
# <a name="writer"></a>编写器



验证编写器或组件包含或排除从备份或还原过程的编写器或组件。 如果使用不带参数，**编写器**在命令提示符下显示的帮助。

## <a name="syntax"></a>语法

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

## <a name="parameters"></a>Parameters

| 参数  |                                                                                      描述                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   verify   | 验证包含在备份或还原过程中指定的编写器或组件。 如果编写器或组件不包括，在备份或还原过程将失败。 |
|  exclude   |                                                   从备份或还原过程中排除指定的编写器或组件。                                                    |
| [\<Writer> |                                                                                     <Component>]                                                                                      |

## <a name="BKMK_examples"></a>示例

若要验证通过指定其 GUID （对于此示例中，4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f） 编写器，请键入：
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
若要排除具有名称"系统写入程序"的编写器，请键入：
```
writer exclude "System Writer"
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)