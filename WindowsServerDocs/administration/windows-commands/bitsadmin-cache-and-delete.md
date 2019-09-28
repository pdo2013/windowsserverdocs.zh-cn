---
title: bitsadmin 缓存和删除
description: '**Bitsadmin 缓存和删除**的 Windows 命令主题-删除特定的缓存条目。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87c3ffd7e0c9c43e8e2eb6e5d5a1d98610a4d9ad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382071"
---
# <a name="bitsadmin-cache-and-delete"></a>bitsadmin 缓存和删除



删除特定的缓存条目。

## <a name="syntax"></a>语法

```
bitsadmin /Cache /Delete RecordID 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|RecordID|与缓存项关联的 GUID。|

## <a name="BKMK_examples"></a>示例

下面的示例删除 RecordID 为 {6511FB02-E195-40A2-B595-E8E2F8F47702} 的缓存条目。
```
C:\>bitsadmin /Cache /Delete {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)