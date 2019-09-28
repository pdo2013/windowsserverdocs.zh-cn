---
title: bitsadmin 缓存和 setexpirationtime
description: 适用于**bitsadmin 缓存和 setexpirationtime**的 Windows 命令主题-设置缓存过期时间。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 386c6659e4410b41669ade39d8af97829d81a1cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381922"
---
>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>bitsadmin 缓存和 setexpirationtime
设置缓存过期时间。
## <a name="syntax"></a>语法
```
bitsadmin /Cache /SetExpirationtime secs
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|秒钟|缓存过期前等待的秒数。|
## <a name="BKMK_examples"></a>示例
下面的示例在60秒内将缓存过期。
```
C:\>bitsadmin /Cache / SetExpirationtime 60
```
## <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)
