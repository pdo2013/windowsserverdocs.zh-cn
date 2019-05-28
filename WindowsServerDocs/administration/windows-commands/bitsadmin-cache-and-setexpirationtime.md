---
title: bitsadmin 缓存和 setexpirationtime
description: Windows 命令主题**bitsadmin 缓存和 setexpirationtime** -设置的缓存过期时间。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8e896df0a88c0cfc4eec07aba4807f184e7abe32
ms.sourcegitcommit: b190fac4bfa5599751a60d3fc3b4c4a64dd9afd7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2019
ms.locfileid: "66008938"
---
>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>bitsadmin 缓存和 setexpirationtime
设置缓存过期时间。
## <a name="syntax"></a>语法
```
bitsadmin /Cache /SetExpirationtime secs
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|秒|缓存过期之前的秒数。|
## <a name="BKMK_examples"></a>示例
下面的示例将缓存在 60 秒后过期。
```
C:\>bitsadmin /Cache / SetExpirationtime 60
```
## <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)
