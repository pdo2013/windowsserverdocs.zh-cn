---
title: bdehdcfg 重新启动
description: Windows 命令主题 bdehdcfg 重新启动-告知 bdehdcfg 的驱动器准备已结束后，，应重新启动计算机。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0f361db8fdf33bd414556575de75241f7dbd9327
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879458"
---
# <a name="bdehdcfg-restart"></a>bdehdcfg: restart



通知 Bdehdcfg 命令行工具的驱动器准备已结束后，，应重新启动计算机。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -restart
```

### <a name="parameters"></a>Parameters

此命令不含有其他参数。

## <a name="remarks"></a>备注

如果其他用户登录到计算机并**安静**命令未指定，将显示一条提示来确认应该重新启动计算机。

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用**重新启动**命令。
```
bdehdcfg -target default -restart
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)