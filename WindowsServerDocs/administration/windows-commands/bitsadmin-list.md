---
title: bitsadmin list
description: Windows 命令主题**bitsadmin 列表**-列出了由当前用户拥有的传输作业。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0b88001b9c4ae01b57006ffeef66dec0348ca77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873858"
---
# <a name="bitsadmin-list"></a>bitsadmin list



列出了由当前用户拥有的传输作业。

## <a name="syntax"></a>语法

```
bitsadmin /List [/allusers][/verbose]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/Allusers|可选-列出所有用户的作业|
|/Verbose|可选 — 提供了为每个作业的详细的信息。|

## <a name="remarks"></a>备注

必须拥有管理员特权才能使用 /allusers 参数

## <a name="BKMK_examples"></a>示例

下面的示例检索有关当前用户拥有的作业的信息。
```
C:\>bitsadmin /List 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)