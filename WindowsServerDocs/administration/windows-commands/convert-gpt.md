---
title: convert gpt
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 433e30efeecec4e4ec51d67c40c14cacf986d12e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434222"
---
# <a name="convert-gpt"></a>convert gpt



具有主启动记录 (MBR) 分区形式的空白基本磁盘转换为具有 GUID 分区表 (GPT) 分区形式的基本磁盘。

有关如何使用此命令的说明，请参阅[主启动记录磁盘更改为 GUID 分区表磁盘](https://go.microsoft.com/fwlink/?LinkId=207049)(https://go.microsoft.com/fwlink/?LinkId=207049)。

## <a name="syntax"></a>语法

```
convert gpt [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|

## <a name="remarks"></a>备注

> [!IMPORTANT]
> 磁盘必须为空，将其转换为 GPT 磁盘。 备份你的数据，并在转换磁盘之前删除所有分区或卷。
> -   转换为 GPT 的所需的最小磁盘大小为 128 兆字节。
> -   若要成功执行此操作，必须选择的基本 MBR 磁盘。 使用**选择的磁盘**命令选择基本磁盘，并将焦点移到它。

## <a name="BKMK_examples"></a>示例

若要将基本磁盘从 MBR 分区形式转换为 GPT 分区形式，请键入：
```
convert gpt
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

