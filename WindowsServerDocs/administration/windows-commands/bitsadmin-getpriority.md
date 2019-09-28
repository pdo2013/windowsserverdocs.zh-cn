---
title: bitsadmin getpriority
description: 适用于**bitsadmin getpriority**的 Windows 命令主题-检索指定作业的优先级。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 0b8914f27c690aa9bb9cbf30430b3edf55f2eb92
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381428"
---
# <a name="bitsadmin-getpriority"></a>bitsadmin getpriority

检索指定的作业的优先级。

## <a name="syntax"></a>语法

```
bitsadmin /GetPriority <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

优先级为 "**前台**"、"**高**"、"**正常**"、"**低**" 或 "**未知**"。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的优先级 *myDownloadJob*。
```
C:\>bitsadmin /GetPriority myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
