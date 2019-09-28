---
title: 删除分区
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 46a214f26e7c21f6ae08eb16d95fd898bd949b0f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378662"
---
# <a name="delete-partition"></a>删除分区



删除具有焦点的分区。

## <a name="syntax"></a>语法

```
delete partition [noerr] [override]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|忽略|允许 DiskPart 删除任何分区，而不考虑类型。 通常，DiskPart 只允许删除已知的数据分区。|
|noerr|仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。|

## <a name="remarks"></a>备注

> [!CAUTION]
> 删除动态磁盘上的分区可能会删除该磁盘上的所有动态卷，因而会销毁所有数据，并使磁盘处于损坏状态。 若要删除动态卷，请始终改用**delete volume**命令。 可以从动态磁盘中删除分区，但不应创建分区。 例如，可以在动态 GPT 磁盘上删除无法识别的 GUID 分区表（GPT）分区。 删除此类分区不会导致生成的可用空间可用。 此命令旨在允许在不能使用 DiskPart 中的**clean**命令的紧急情况下，reclame 损坏的脱机动态磁盘上的空间。
> -   不能删除系统分区、启动分区或任何包含活动页面文件或故障转储信息的分区。
> -   必须选择分区，此操作才能成功。 使用 "**选择分区**" 命令可选择分区，并将焦点移动到该分区。

## <a name="BKMK_examples"></a>示例

若要删除具有焦点的分区，请键入：
```
delete partition
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

