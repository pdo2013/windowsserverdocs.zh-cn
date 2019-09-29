---
title: bdehdcfg newdriveletter
description: 适用于 bdehdcfg newdriveletter 的 Windows 命令主题-将新的驱动器号分配给用作系统驱动器的驱动器部分。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e2abd4a686f358b5dd844514735edb3ffaa13845
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382232"
---
# <a name="bdehdcfg-newdriveletter"></a>bdehdcfg： newdriveletter



将新的驱动器号分配给用作系统驱动器的驱动器部分。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<DriveLetter >|定义将分配给指定的目标驱动器的驱动器号。|

## <a name="remarks"></a>备注

最佳做法是，建议不要将驱动器号分配给系统驱动器。

## <a name="BKMK_Examples"></a>示例

以下示例显示了向默认驱动器分配驱动器号 P。
```
bdehdcfg -target default -newdriveletter P:
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)