---
title: 磁盘详细信息
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c7a5063edf3cb2e190e8aec957e1b571c1f15bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819098"
---
# <a name="detail-disk"></a>磁盘详细信息



显示所选磁盘的属性和该磁盘上的卷。

## <a name="syntax"></a>语法

```
detail disk
```

## <a name="remarks"></a>备注

-   成功执行此操作，必须选择一个磁盘。 使用**选择的磁盘**命令选择某一磁盘，并将焦点移到它。
-   如果选择的磁盘是虚拟硬盘 (VHD)**详细信息磁盘**报告作为虚拟磁盘的总线类型。

## <a name="BKMK_examples"></a>示例

若要查看所选的磁盘和卷的磁盘中的信息的属性，请键入：
```
detail disk
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

