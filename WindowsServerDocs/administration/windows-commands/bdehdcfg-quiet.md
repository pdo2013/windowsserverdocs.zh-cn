---
title: bdehdcfg quiet
description: 适用于 bdehdcfg quiet 的 Windows 命令主题-通知 bdehdcfg 不显示所有操作和错误。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d59a14e34200e3fa8e18e36e166ef62ceca1afe7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382217"
---
# <a name="bdehdcfg-quiet"></a>bdehdcfg： quiet



通知 Bdehdcfg 命令行工具所有操作和错误不会显示在命令行界面中。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

### <a name="parameters"></a>Parameters

此命令不使用其他参数。

## <a name="remarks"></a>备注

如果在驱动器准备过程中显示 "是/否（Y/N）" 提示，则假定为 "是" 答案。 若要查看驱动器准备过程中出现的任何错误，请查看**DrivePreparationTool**事件提供程序下的系统事件日志。

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用**quiet**命令。
```
bdehdcfg -target default -quiet
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)