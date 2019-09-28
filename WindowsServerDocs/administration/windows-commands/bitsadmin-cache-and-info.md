---
title: bitsadmin 缓存和信息
description: '**Bitsadmin 缓存和信息**的 Windows 命令主题-转储特定的缓存条目。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 11963ff5640ef30e597e5e802778aff121c0efb3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381970"
---
# <a name="bitsadmin-cache-and-info"></a>bitsadmin 缓存和信息



转储特定的缓存项。

## <a name="syntax"></a>语法

```
bitsadmin /Cache /Info RecordID [/Verbose] 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|RecordID|与缓存项关联的 GUID。|

## <a name="BKMK_examples"></a>示例

下面的示例将缓存项转储为 RecordID 的 {6511FB02-E195-40A2-B595-E8E2F8F47702}。
```
C:\>bitsadmin /Cache /Info {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)