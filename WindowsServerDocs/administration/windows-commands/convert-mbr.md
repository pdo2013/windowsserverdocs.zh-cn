---
title: convert mbr
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 47001415158b3bdb0b06af9114b995a6f0634da1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379072"
---
# <a name="convert-mbr"></a>convert mbr



将具有 GUID 分区表（GPT）分区形式的空白基本磁盘转换为具有主启动记录（MBR）分区形式的基本磁盘。

> [!IMPORTANT]
> 磁盘必须为空，才能将其转换为 MBR 磁盘。 在转换磁盘之前，请备份数据，然后删除所有分区或卷。

有关如何使用此命令的说明，请参阅[将 GUID 分区表磁盘更改为主启动记录磁盘](https://go.microsoft.com/fwlink/?LinkId=207050)（ https://go.microsoft.com/fwlink/?LinkId=207050) 。

## <a name="syntax"></a>语法

```
convert mbr [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。|

## <a name="remarks"></a>备注

-   若要成功执行此操作，必须选择基本磁盘。 使用 "**选择磁盘**" 命令选择基本磁盘，并将焦点移动到该磁盘。

## <a name="BKMK_examples"></a>示例

若要将基本光盘从 GPT 分区形式转换为 MBR 分区形式，请键入 >：
```
convert mbr
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

