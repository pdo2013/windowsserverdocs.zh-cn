---
title: bdehdcfg 目标
description: Bdehdcfg target 的 Windows 命令主题-通过 BitLocker 和 Windows 恢复将分区准备用作系统驱动器。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2fb0a1daa257ef2c9f1cd77b88e5ef14f84a0dfa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382177"
---
# <a name="bdehdcfg-target"></a>bdehdcfg：目标



准备要由 BitLocker 和 Windows 恢复用作系统驱动器的分区。 默认情况下，创建此分区时没有驱动器号。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|default|指示命令行工具将遵循与 BitLocker 安装向导相同的过程。|
|分配|使用磁盘上的未分配空间创建系统分区。|
|@no__t > 收缩|减少创建活动系统分区所需的驱动器数量。 若要使用此命令，指定的驱动器必须至少有 5% 的可用空间。|
|\<DriveLetter > merge|使用指定为活动系统分区的驱动器。 操作系统驱动器不能是合并目标。|

## <a name="BKMK_Examples"></a>示例

以下示例描述了如何使用**target**命令将现有驱动器（P）指定为系统驱动器。
```
bdehdcfg -target P: merge
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)