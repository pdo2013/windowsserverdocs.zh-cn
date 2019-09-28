---
title: 删除磁盘
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 886e84edf80227d42ad77e36aed388b9e853ca62
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378666"
---
# <a name="delete-disk"></a>删除磁盘



从磁盘列表中删除丢失的动态磁盘。

有关如何使用此命令的说明，请参阅[删除丢失的动态磁盘](https://go.microsoft.com/fwlink/?LinkId=207055)（ https://go.microsoft.com/fwlink/?LinkId=207055) 。

## <a name="syntax"></a>语法

```
delete disk [noerr] [override]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。|
|忽略|允许 DiskPart 删除磁盘上的所有简单卷。 如果磁盘包含镜像卷的一半，则会删除磁盘上的一半镜像。 如果磁盘是 RAID-5 卷的成员，则删除磁盘替代命令将失败。|

## <a name="BKMK_examples"></a>示例

若要从磁盘列表中删除丢失的动态磁盘，请键入：
```
delete disk
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

