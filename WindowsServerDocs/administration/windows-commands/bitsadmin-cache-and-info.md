---
title: bitsadmin 缓存和信息
description: Windows 命令主题**bitsadmin 缓存和信息**-转储指定缓存项。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 61ff57b33e575921f2032d4e13a2d9b74accae60
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813318"
---
# <a name="bitsadmin-cache-and-info"></a>bitsadmin 缓存和信息



转储指定缓存项。

## <a name="syntax"></a>语法

```
bitsadmin /Cache /Info RecordID [/Verbose] 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|RecordID|与缓存项关联的 GUID。|

## <a name="BKMK_examples"></a>示例

下面的示例将转储具有 {6511FB02-E195-40A2-B595-E8E2F8F47702} RecordID 的缓存条目。
```
C:\>bitsadmin /Cache /Info {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)