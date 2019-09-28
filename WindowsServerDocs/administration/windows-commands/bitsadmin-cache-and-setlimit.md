---
title: bitsadmin 缓存和 setlimit
description: 适用于**bitsadmin 缓存和 setlimit**的 Windows 命令主题-设置缓存大小限制。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88a10ce8599202e237daa6822cf62806d3c21429
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381936"
---
# <a name="bitsadmin-cache-and-setlimit"></a>bitsadmin 缓存和 setlimit



设置缓存大小限制。

## <a name="syntax"></a>语法

```
bitsadmin /Cache /SetLimit Percent
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|Percent|缓存限制定义为总硬盘空间的百分比。|

## <a name="BKMK_examples"></a>示例

下面的示例将缓存大小限制为 50%。
```
C:\>bitsadmin /Cache /SetLimit 50 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)