---
title: bitsadmin 缓存和 deleteurl
description: Windows 命令主题**bitsadmin 缓存和 deleteurl** -对于给定的 URL 中删除所有缓存条目。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a831c49e1461761cb7466b46e7a5ad8e037f4ec9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816648"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>bitsadmin 缓存和 deleteurl



对于给定的 URL 中删除所有缓存条目。

## <a name="syntax"></a>语法

```
bitsadmin /DeleteURL url
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|url|统一资源定位符，用于标识远程文件。|

## <a name="BKMK_examples"></a>示例

以下示例将删除所有缓存条目 https://www.microsoft.com/en/us/default.aspx
```
C:\>bitsadmin /DeleteURL https://www.microsoft.com/en/us/default.aspx 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)