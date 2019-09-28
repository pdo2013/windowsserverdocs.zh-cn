---
title: 变化
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b076e12c833645833f53a06476e62bbf44f2690
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384491"
---
# <a name="retain"></a>变化



准备要用作启动卷或系统卷的现有动态简单卷。

## <a name="syntax"></a>语法

```
retain
```

## <a name="remarks"></a>备注

-   在主启动记录（MBR）动态磁盘上，此命令在主启动记录中创建分区条目。
-   在 GUID 分区表（GPT）动态磁盘上，此命令在 GUID 分区表中创建分区条目。

#### <a name="additional-references"></a>其他参考

