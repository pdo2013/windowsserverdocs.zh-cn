---
title: bitsadmin getvalidationstate
description: '适用于**bitsadmin getvalidationstate**的 Windows 命令主题-报告作业中给定文件的内容验证状态。 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca4269a596010258edd0479f5a7e9844bc9c98df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381256"
---
# <a name="bitsadmin-getvalidationstate"></a>bitsadmin getvalidationstate



报告作业中给定文件的内容验证状态。

## <a name="syntax"></a>语法

```
bitsadmin /GetValidationState <Job> <file index> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|文件索引|从0开始|

## <a name="BKMK_examples"></a>示例

下面的示例获取名为*myJob*的作业中的文件2的内容验证状态。
```
C:\>bitsadmin /GetValidationState myJob 1
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)