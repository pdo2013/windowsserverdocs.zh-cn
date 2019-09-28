---
title: bdehdcfg 重新启动
description: 用于 bdehdcfg restart 的 Windows 命令主题-通知 bdehdcfg 应在驱动器准备结束后重新启动计算机。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6c4e48b051f567c98ea679feaa22f995982a899
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382210"
---
# <a name="bdehdcfg-restart"></a>bdehdcfg：重新启动



通知 Bdehdcfg 命令行工具应在驱动器准备结束后重新启动计算机。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -restart
```

### <a name="parameters"></a>Parameters

此命令不使用其他参数。

## <a name="remarks"></a>备注

如果其他用户登录到计算机，但未指定**quiet**命令，则将显示一条提示，确认应重新启动计算机。

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用**restart**命令。
```
bdehdcfg -target default -restart
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)