---
title: convert basic
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: df1f999499154366304d59e0573ba921ab1af83d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434238"
---
# <a name="convert-basic"></a>convert basic



将空的动态磁盘转换为基本磁盘。

有关如何使用此命令的说明，请参阅[改为基本磁盘的动态磁盘重新](https://go.microsoft.com/fwlink/?LinkId=207048)(https://go.microsoft.com/fwlink/?LinkId=207048)。

## <a name="syntax"></a>语法

```
convert basic [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|

## <a name="remarks"></a>备注

> [!IMPORTANT]
> 磁盘必须为空，将其转换为基本磁盘。 备份你的数据，并在转换磁盘之前删除所有分区或卷。
> -   若要成功执行此操作，必须选择动态磁盘。 使用**选择的磁盘**命令选择动态磁盘，并将焦点移到它。

## <a name="BKMK_examples"></a>示例

若要将所选的动态磁盘转换为 basic，请键入：
```
convert basic
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

