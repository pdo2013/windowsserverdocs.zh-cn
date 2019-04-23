---
title: bdehdcfg 目标
description: Windows 命令主题 bdehdcfg 目标-准备用于为系统驱动器的 BitLocker 和 Windows 恢复分区。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8d180974f480b4c40532dab529ad49dcc33540d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881528"
---
# <a name="bdehdcfg-target"></a>bdehdcfg: target



准备用于为系统驱动器的 BitLocker 和 Windows 恢复分区。 默认情况下，没有驱动器号的情况下创建此分区。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|default|指示命令行工具将遵循与 BitLocker 安装向导相同的进程。|
|未分配|在磁盘上创建系统分区超出可用的未分配空间。|
|\<驱动器号 > 收缩|减少了创建活动的系统分区所需的数量指定的驱动器。 若要使用此命令，指定的驱动器必须具有至少 5%的可用空间。|
|\<驱动器号 > 合并|使用指定为活动的系统分区的驱动器。 操作系统驱动器不能是合并的目标。|

## <a name="BKMK_Examples"></a>示例

以下示例展示了使用**目标**命令以将现有的驱动器 (P) 指定为系统驱动器。
```
bdehdcfg -target P: merge
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)