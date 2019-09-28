---
title: 转换动态
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 15c15d14aeb440c5d7862f0a304f223988f52bbe
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379096"
---
# <a name="convert-dynamic"></a>转换动态



将基本磁盘转换为动态磁盘。

有关如何使用此命令的说明，请参阅[将基本磁盘更改为动态磁盘](https://go.microsoft.com/fwlink/?LinkId=207047)（ https://go.microsoft.com/fwlink/?LinkId=207047) 。

## <a name="syntax"></a>语法

```
convert dynamic [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。|

## <a name="remarks"></a>备注

-   基本磁盘上的任何现有分区都将成为简单卷。
-   若要成功执行此操作，必须选择基本磁盘。 使用 "**选择磁盘**" 命令选择基本磁盘，并将焦点移动到该磁盘。

## <a name="BKMK_examples"></a>示例

若要将基本磁盘转换为动态磁盘，请键入：
```
convert dynamic
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

