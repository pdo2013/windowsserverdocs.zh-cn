---
title: bdehdcfg newdriveletter
description: Windows 命令主题 bdehdcfg newdriveletter-将新的驱动器号分配给用作系统驱动器的驱动器的部分。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd40942dfb724d46c0fa9a43c4646e1db09d2a76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887138"
---
# <a name="bdehdcfg-newdriveletter"></a>bdehdcfg: newdriveletter



将新的驱动器号分配给用作系统驱动器的驱动器的部分。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<DriveLetter>|定义将分配给指定的目标驱动器的驱动器号。|

## <a name="remarks"></a>备注

作为最佳做法，建议现在将驱动器号分配给您的系统驱动器。

## <a name="BKMK_Examples"></a>示例

下面的示例显示了要分配驱动器号 P.的默认驱动器
```
bdehdcfg -target default -newdriveletter P:
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)