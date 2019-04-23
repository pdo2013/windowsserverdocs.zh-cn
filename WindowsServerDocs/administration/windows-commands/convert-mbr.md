---
title: convert mbr
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a635a4c0-af73-4330-b021-51d483424537
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da8d62567863bc38a5aa0b35a8f3fe4ee24cc888
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834608"
---
# <a name="convert-mbr"></a>convert mbr



将具有主启动记录 (MBR) 分区形式的基本磁盘转换为具有 GUID 分区表 (GPT) 分区形式的空白基本磁盘。

> [!IMPORTANT]
> 磁盘必须为空，将其转换为 MBR 磁盘。 备份你的数据，并在转换磁盘之前删除所有分区或卷。

有关如何使用此命令的说明，请参阅[GUID 分区表磁盘更改主启动记录磁盘](https://go.microsoft.com/fwlink/?LinkId=207050)(https://go.microsoft.com/fwlink/?LinkId=207050)。

## <a name="syntax"></a>语法

```
convert mbr [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|

## <a name="remarks"></a>备注

-   若要成功执行此操作，必须选择基本磁盘。 使用**选择的磁盘**命令选择基本磁盘，并将焦点移到它。

## <a name="BKMK_examples"></a>示例

若要将基本光盘从 GPT 分区形式转换为 MBR 分区形式，请键入 >:
```
convert mbr
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

