---
title: bdehdcfg quiet
description: Windows 命令主题 bdehdcfg quiet-会告知 bdehdcfg，则不显示所有操作和错误。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0f0d98f6ae76e9bf6357689c97e091766b9645c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865748"
---
# <a name="bdehdcfg-quiet"></a>bdehdcfg: quiet



通知所有操作和错误都将不会显示在命令行界面的 Bdehdcfg 命令行工具。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

### <a name="parameters"></a>Parameters

此命令不含有其他参数。

## <a name="remarks"></a>备注

如果任何是 / 在驱动器准备过程中，将已显示没有 （是/否） 提示，则假定答案"是"。 若要查看驱动器准备过程中发生任何错误，请查看下的系统事件日志**Microsoft Windows BitLocker DrivePreparationTool**事件提供程序。

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用**安静**命令。
```
bdehdcfg -target default -quiet
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)