---
title: bdehdcfg 大小
description: Windows 命令主题-指定在创建新的系统驱动器时的系统分区大小。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6ec42cdb5716c63c7210ea6cfde8ce8884833b45
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382199"
---
# <a name="bdehdcfg-size"></a>bdehdcfg： size



指定在创建新的系统驱动器时的系统分区大小。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink} -size <SizeinMB>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<SizeinMB >|指示用于新分区的兆字节数（MB）。|

## <a name="remarks"></a>备注

如果未指定大小，则该工具将使用默认值 300 MB。 系统驱动器的最小大小为 100 MB。 如果要将系统恢复或其他系统工具存储在系统分区上，则应相应地增加大小。

> [!NOTE]
> **Size**命令不能与**目标**@no__t > **merge**命令组合。

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用**size**命令将 500 MB 分配给默认系统驱动器。
```
bdehdcfg -target default -size 500
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)