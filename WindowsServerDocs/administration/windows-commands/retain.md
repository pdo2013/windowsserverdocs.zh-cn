---
title: 保留
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4b437e9f0c8d671e4378311d450aa0ac7639219f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852168"
---
# <a name="retain"></a>保留



准备现有动态简单卷，要用作启动卷或系统卷。

## <a name="syntax"></a>语法

```
retain
```

## <a name="remarks"></a>备注

-   在主启动记录 (MBR) 动态磁盘上，此命令在主启动记录中创建分区项。
-   在 GUID 分区表 (GPT) 动态磁盘上，此命令中的 GUID 分区表创建分区项。

#### <a name="additional-references"></a>其他参考

