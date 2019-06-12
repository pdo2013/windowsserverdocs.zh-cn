---
title: 非活动状态
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9c8ded732d984830c7892720f75938979f1abb67
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438162"
---
# <a name="inactive"></a>非活动状态



在基本主启动记录 (MBR) 磁盘上将系统分区或启动分区为非活动状态的选中标记。

## <a name="syntax"></a>语法

```
inactive
```

## <a name="remarks"></a>备注

> [!CAUTION]
> 没有活动分区，您的计算机可能不会启动。 除非您是经验丰富的用户有深入了解的 Windows 操作系统系列，不要将标记为非活动状态的系统或启动分区。</br>> 如果您不能标记为非活动状态的系统或启动分区后启动你的计算机，Windows 安装程序 CD 插入 CD-ROM 驱动器，重新启动计算机，并修复分区使用**fixmbr** 和**fixboot**恢复控制台中的命令。
> -   标记的系统分区或启动分区为非活动状态后，计算机可启动从 BIOS，如 CD-ROM 驱动器或预启动执行环境 (PXE) 中指定的下一个选项。
> -   若要成功执行此操作，必须选择活动的系统或启动分区。 使用**选择分区**命令选择活动分区，并将焦点移到它。

## <a name="BKMK_examples"></a>示例

```
inactive
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

