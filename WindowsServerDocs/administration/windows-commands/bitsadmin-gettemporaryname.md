---
title: bitsadmin gettemporaryname
description: 适用于**bitsadmin gettemporaryname**的 Windows 命令主题-报告作业中给定文件的临时文件名。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b665fae4c0bfdd5ea04b929be49f9590430b358
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381298"
---
# <a name="bitsadmin-gettemporaryname"></a>bitsadmin gettemporaryname



报告作业中给定文件的临时文件名。

## <a name="syntax"></a>语法

```
bitsadmin /GetTemporaryName <Job> <file index> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|文件索引|从0开始|

## <a name="BKMK_examples"></a>示例

下面的示例报告名为*myJob*的作业的文件2的临时文件名。
```
C:\>bitsadmin /GetTemporaryName myJob 1 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)