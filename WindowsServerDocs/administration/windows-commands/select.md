---
title: 选择
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c3723dd414adca68c22011ef3f6be02eb6531d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889898"
---
# <a name="select"></a>选择



将焦点移到磁盘、 分区、 卷或虚拟硬盘 (VHD)。

## <a name="syntax"></a>语法

```
select disk
select partition
select volume
select vdisk
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[选择磁盘](select-disk.md)|将焦点移到磁盘。|
|[选择分区](select-partition.md)|将焦点移到分区。|
|[选择卷](select-volume.md)|将焦点转移到的卷。|
|[选择的虚拟磁盘](select-vdisk.md)|将焦点转移到的 VHD。|

## <a name="remarks"></a>备注

-   如果具有一个对应的分区选择卷，则将自动选择该分区。
-   如果使用相应的卷选择分区，则将自动选择该卷。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

