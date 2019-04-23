---
title: bitsadmin 缓存和删除
description: Windows 命令主题**bitsadmin 缓存并删除**-删除某个特定缓存项。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 63b82cbbadebf2c4e36f2c76076b329787d7b1b5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852508"
---
# <a name="bitsadmin-cache-and-delete"></a>bitsadmin 缓存和删除



删除某个特定缓存项。

## <a name="syntax"></a>语法

```
bitsadmin /Cache /Delete RecordID 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|RecordID|与缓存项关联的 GUID。|

## <a name="BKMK_examples"></a>示例

以下示例将删除具有 {6511FB02-E195-40A2-B595-E8E2F8F47702} RecordID 的缓存条目。
```
C:\>bitsadmin /Cache /Delete {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)