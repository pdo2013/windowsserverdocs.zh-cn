---
title: 转换动态
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 353c1e4558ab2b0c948ec78c0cd87b579c738ec8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841608"
---
# <a name="convert-dynamic"></a>转换动态



基本磁盘转换为动态磁盘。

有关如何使用此命令的说明，请参阅[将基本磁盘转换为动态磁盘](https://go.microsoft.com/fwlink/?LinkId=207047)(https://go.microsoft.com/fwlink/?LinkId=207047)。

## <a name="syntax"></a>语法

```
convert dynamic [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|

## <a name="remarks"></a>备注

-   基本磁盘上的任何现有分区成为简单卷。
-   若要成功执行此操作，必须选择基本磁盘。 使用**选择的磁盘**命令选择基本磁盘，并将焦点移到它。

## <a name="BKMK_examples"></a>示例

若要将基本磁盘转换为动态磁盘，请键入：
```
convert dynamic
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

