---
title: bitsadmin list
description: '**Bitsadmin 列表**的 Windows 命令主题-列出当前用户拥有的传输作业。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bd4787f51dc2a7843ff6cf5c4f786658e530ad8f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381105"
---
# <a name="bitsadmin-list"></a>bitsadmin list



列出当前用户拥有的传输作业。

## <a name="syntax"></a>语法

```
bitsadmin /List [/allusers][/verbose]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/Allusers|可选-列出所有用户的作业|
|/Verbose|可选-提供每个作业的详细信息。|

## <a name="remarks"></a>备注

您必须具有管理员特权才能使用/allusers 参数

## <a name="BKMK_examples"></a>示例

下面的示例检索有关当前用户拥有的作业的信息。
```
C:\>bitsadmin /List 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)