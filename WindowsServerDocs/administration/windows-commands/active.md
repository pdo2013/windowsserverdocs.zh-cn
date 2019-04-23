---
title: active
description: Windows 命令主题**active** -基本磁盘上将标记为活动状态的选中的分区。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a039e0200fb84d446739ac7017556b6c302f4af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868758"
---
# <a name="active"></a>active



在基本磁盘上将标记为活动状态的选中的分区。

> [!CAUTION]
> DiskPart 仅验证分区才可以包含操作系统启动文件。 DiskPart 不检查分区的内容。 如果错误地标记为活动分区，并且它不包含操作系统启动文件，计算机可能无法启动。

## <a name="syntax"></a>语法

```
active
```

## <a name="remarks"></a>备注

-   这会通知的基本输入/输出系统 (BIOS) 或可扩展固件接口 (EFI) 的分区或卷是有效的系统分区或系统卷。
-   只有分区才可以将标记为活动状态。
-   若要成功执行此操作，必须选择分区。 使用**选择分区**命令选择分区，并将焦点移到它。

## <a name="BKMK_examples"></a>示例

若要将标记为活动分区选中的分区，请键入：
```
active
```

#### <a name="additional-references"></a>其他参考

