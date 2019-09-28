---
title: bitsadmin setvalidationstate
description: 适用于**bitsadmin setvalidationstate**的 Windows 命令主题-设置作业中给定文件的内容验证状态。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37d7fa3a8a91abf1e7b6ac5a51b6cebd78984a91
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380397"
---
# <a name="bitsadmin-setvalidationstate"></a>bitsadmin setvalidationstate



设置作业中给定文件的内容验证状态。

## <a name="syntax"></a>语法

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

## <a name="parameters"></a>Parameters

| 参数  |          描述           |
|------------|--------------------------------|
|    作业     | 该作业的显示名称或 GUID |
| 文件索引 |         从0开始          |
|    True    |             False              |

## <a name="BKMK_examples"></a>示例

下面的示例将名为*myJob*的作业的文件2的内容验证状态设置为 TRUE。
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)