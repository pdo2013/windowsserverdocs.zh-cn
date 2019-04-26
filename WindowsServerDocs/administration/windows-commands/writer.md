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
ms.openlocfilehash: 87b10952c6a851b5536a1589b994b265e8699f59
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826398"
---
# <a name="writer"></a>编写器



验证编写器或组件包含或排除从备份或还原过程的编写器或组件。 如果使用不带参数，**编写器**在命令提示符下显示的帮助。

## <a name="syntax"></a>语法

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|verify|验证包含在备份或还原过程中指定的编写器或组件。 如果编写器或组件不包括，在备份或还原过程将失败。|
|exclude|从备份或还原过程中排除指定的编写器或组件。|
|[\<Writer> | <Component>]|指定要验证或排除的编写器或组件。 编写器指定的编写器 GUID 或通过编写器名称，例如"系统编写器。"|

## <a name="BKMK_examples"></a>示例

若要验证通过指定其 GUID （对于此示例中，4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f） 编写器，请键入：
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
若要排除的编写器具有名称"系统写入程序？ 类型：
```
writer exclude "System Writer"
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)