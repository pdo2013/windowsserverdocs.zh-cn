---
title: active
description: Windows 命令主题对于**活动**的基本磁盘，将焦点标记为活动分区。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c926bf9b7a583cf7eaa23166e09e6f0a1599e625
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382846"
---
# <a name="active"></a>active



在基本磁盘上，将具有焦点的分区标记为活动分区。

> [!CAUTION]
> DiskPart 只验证分区是否能够包含操作系统启动文件。 DiskPart 不检查分区的内容。 如果错误地将某个分区标记为活动，并且它不包含操作系统启动文件，则您的计算机可能无法启动。

## <a name="syntax"></a>语法

```
active
```

## <a name="remarks"></a>备注

-   这会通知基本输入/输出系统（BIOS）或可扩展固件接口（EFI）分区或卷是有效的系统分区或系统卷。
-   只能将分区标记为活动分区。
-   必须选择分区，此操作才能成功。 使用 "**选择分区**" 命令可选择分区，并将焦点移动到该分区。

## <a name="BKMK_examples"></a>示例

若要将具有焦点的分区标记为活动分区，请键入：
```
active
```

#### <a name="additional-references"></a>其他参考

