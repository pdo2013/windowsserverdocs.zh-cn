---
title: 不用
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ce91b6a024c165e3aa63148b9ad6dfcc4db7a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375370"
---
# <a name="inactive"></a>不用



在基本主启动记录（MBR）磁盘上，将具有焦点的系统分区或启动分区标记为非活动。

## <a name="syntax"></a>语法

```
inactive
```

## <a name="remarks"></a>备注

> [!CAUTION]
> 如果没有活动分区，计算机可能无法启动。 请勿将系统分区或启动分区标记为非活动，除非你是一个全面了解 Windows 系列操作系统的经验丰富的用户。</br>> 如果将系统或启动分区标记为非活动状态后无法启动计算机，请将 Windows 安装程序 CD 插入 cd-rom 驱动器中，重新启动计算机，然后使用中的**fixmbr**和**fixboot**命令修复分区恢复控制台。
> -   将系统分区或启动分区标记为非活动后，计算机将从 BIOS 中指定的下一个选项启动，如 CD-ROM 驱动器或预启动执行环境（PXE）。
> -   必须选择一个活动系统分区或启动分区，此操作才能成功。 使用 "**选择分区**" 命令选择活动分区，并将焦点移动到该分区。

## <a name="BKMK_examples"></a>示例

```
inactive
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

