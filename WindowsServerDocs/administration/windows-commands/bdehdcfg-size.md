---
title: bdehdcfg 大小
description: Windows 命令主题-在创建新的系统驱动器时指定的系统分区的大小。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d024bb4092f93782300d6afb9053cee1da32629a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817518"
---
# <a name="bdehdcfg-size"></a>bdehdcfg： 大小



在创建新的系统驱动器时，请指定系统分区的大小。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink} -size <SizeinMB>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<SizeinMB>|指示兆字节 (MB) 用于新的分区数。|

## <a name="remarks"></a>备注

如果不指定大小，该工具将使用默认值 300 MB。 系统驱动器的最小大小为 100 MB。 如果将系统分区上存储系统恢复或其他系统工具，应相应地增加大小。

> [!NOTE]
> **大小**命令不能结合**目标**\<驱动器号 >**合并**命令。

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用**大小**命令要分配给默认系统驱动器的 500 MB。
```
bdehdcfg -target default -size 500
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)