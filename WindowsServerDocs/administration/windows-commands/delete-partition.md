---
title: 删除分区
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b47338b74cf71a4754b7320d6b3842f342d324d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436141"
---
# <a name="delete-partition"></a>删除分区



删除选中的分区。

## <a name="syntax"></a>语法

```
delete partition [noerr] [override]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|重写|允许 DiskPart 删除任何类型的分区。 通常情况下，DiskPart 只允许删除已知的数据分区。|
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|

## <a name="remarks"></a>备注

> [!CAUTION]
> 删除动态磁盘上的分区会删除所有动态卷上的磁盘，从而破坏所有数据并使磁盘处于损坏状态。 若要删除动态卷，请始终使用**删除卷**命令。 可以从动态磁盘删除分区，但不是会创建。 例如，就可以删除动态 GPT 磁盘上应用无法识别的 GUID 分区表 (GPT) 分区。 删除这样的分区不会导致生成的可用空间变得可用。 此命令为了让您到 reclame 的紧急情况下已损坏的脱机动态磁盘上的空间位置**干净**不能使用 DiskPart 中的命令。
> -   无法删除系统分区、 启动分区或任何包含的活动的分页文件或故障转储信息分区。
> -   若要成功执行此操作，必须选择分区。 使用**选择分区**命令选择分区，并将焦点移到它。

## <a name="BKMK_examples"></a>示例

若要删除选中的分区，请键入：
```
delete partition
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

