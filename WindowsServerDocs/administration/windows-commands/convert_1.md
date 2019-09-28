---
title: convert
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae151297-af21-4701-bd69-21d775518e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d78f7adbc26acf9787ad39019e1450542a6acda2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379031"
---
# <a name="convert"></a>convert



将磁盘从一种磁盘类型转换为另一种类型。

## <a name="syntax"></a>语法

```
convert basic
convert dynamic
convert gpt
convert mbr
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[转换基本](convert-basic.md)|将空动态磁盘转换为基本磁盘。|
|[转换动态](convert-dynamic.md)|将基本磁盘转换为动态磁盘。|
|[转换 gpt](convert-gpt.md)|将具有主启动记录（MBR）分区形式的空白基本磁盘转换为具有 GUID 分区表（GPT）分区形式的基本磁盘。|
|[转换 mbr](convert-mbr.md)|将具有 GUID 分区表（GPT）分区形式的空白基本磁盘转换为具有主启动记录（MBR）分区形式的基本磁盘。|

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

