---
title: bitsadmin 缓存和 setlimit
description: Windows 命令主题**bitsadmin 缓存和 setlimit** -设置缓存大小限制。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7d0b72c5ec6c779fa4ce3fa038352836cd9456ac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852588"
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
|百分比|定义的硬盘总空间百分比的缓存限制...|

## <a name="BKMK_examples"></a>示例

下面的示例限制为 50%的缓存大小。
```
C:\>bitsadmin /Cache /SetLimit 50 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)