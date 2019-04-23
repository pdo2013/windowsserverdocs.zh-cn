---
title: 删除磁盘
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e401133854118e82a45fd5e01466288ae41f67da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863908"
---
# <a name="delete-disk"></a>删除磁盘



从磁盘列表删除丢失的动态磁盘。

有关如何使用此命令的说明，请参阅[删除缺少的动态磁盘](https://go.microsoft.com/fwlink/?LinkId=207055)(https://go.microsoft.com/fwlink/?LinkId=207055)。

## <a name="syntax"></a>语法

```
delete disk [noerr] [override]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|
|重写|允许 DiskPart 删除磁盘上的所有简单卷。 如果磁盘包含半个镜像卷，请删除磁盘上的镜像的一半。 如果磁盘是 raid-5 卷的成员将删除磁盘重写命令失败。|

## <a name="BKMK_examples"></a>示例

若要从磁盘列表删除丢失的动态磁盘，请键入：
```
delete disk
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

