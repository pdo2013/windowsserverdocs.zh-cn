---
title: bitsadmin 缓存和 deleteurl
description: '**Bitsadmin 缓存和 deleteurl**的 Windows 命令主题-删除给定 URL 的所有缓存条目。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 869d3bc0f011cc82aaea9b7468667964051e1c00
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382065"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>bitsadmin 缓存和 deleteurl



删除给定 URL 的所有缓存条目。

## <a name="syntax"></a>语法

```
bitsadmin /DeleteURL url
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|url|标识远程文件的统一资源定位器。|

## <a name="BKMK_examples"></a>示例

下面的示例将删除 @no__t 的所有缓存项
```
C:\>bitsadmin /DeleteURL https://www.microsoft.com/en/us/default.aspx 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)