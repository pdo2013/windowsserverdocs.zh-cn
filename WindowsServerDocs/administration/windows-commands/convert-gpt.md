---
title: convert gpt
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9a6392cbcff618c642b9d0f168fe555e8be9e759
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379092"
---
# <a name="convert-gpt"></a>convert gpt



将具有主启动记录（MBR）分区形式的空白基本磁盘转换为具有 GUID 分区表（GPT）分区形式的基本磁盘。

有关如何使用此命令的说明，请参阅[将主启动记录磁盘更改为 GUID 分区表磁盘](https://go.microsoft.com/fwlink/?LinkId=207049)（ https://go.microsoft.com/fwlink/?LinkId=207049) 。

## <a name="syntax"></a>语法

```
convert gpt [noerr]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|noerr|仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。|

## <a name="remarks"></a>备注

> [!IMPORTANT]
> 磁盘必须为空，才能将其转换为 GPT 磁盘。 在转换磁盘之前，请备份数据，然后删除所有分区或卷。
> -   转换为 GPT 所需的最小磁盘大小为 128 mb。
> -   必须选择基本 MBR 磁盘，此操作才能成功。 使用 "**选择磁盘**" 命令选择基本磁盘，并将焦点移动到该磁盘。

## <a name="BKMK_examples"></a>示例

若要将基本光盘从 MBR 分区形式转换为 GPT 分区形式，请键入：
```
convert gpt
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

