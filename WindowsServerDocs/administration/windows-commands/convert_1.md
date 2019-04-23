---
title: convert
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6a77e1fca9605c7e5cc4ff059db08ffbfcc81f81
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859278"
---
# <a name="convert"></a>convert



将磁盘从一个磁盘类型转换为另一个。

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
|[将基本转换](convert-basic.md)|将空动态磁盘转换为基本磁盘。|
|[转换动态](convert-dynamic.md)|基本磁盘转换为动态磁盘。|
|[转换 gpt](convert-gpt.md)|具有主启动记录 (MBR) 分区形式的空白基本磁盘转换为具有 GUID 分区表 (GPT) 分区形式的基本磁盘。|
|[将 mbr 转换](convert-mbr.md)|将具有主启动记录 (MBR) 分区形式的基本磁盘转换为具有 GUID 分区表 (GPT) 分区形式的空白基本磁盘。|

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

