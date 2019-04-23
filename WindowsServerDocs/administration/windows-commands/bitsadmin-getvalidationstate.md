---
title: bitsadmin getvalidationstate
description: 'Windows 命令主题**bitsadmin getvalidationstate** -报告作业中的给定文件的内容验证状态。 '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8abff3fc9fddb9cff1758739fdc540a9c945efe2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879158"
---
# <a name="bitsadmin-getvalidationstate"></a>bitsadmin getvalidationstate



报告作业中的给定文件的内容验证状态。

## <a name="syntax"></a>语法

```
bitsadmin /GetValidationState <Job> <file index> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|文件索引|从 0 开始|

## <a name="BKMK_examples"></a>示例

下面的示例获取文件 2 中名为的作业的内容验证状态*myJob*。
```
C:\>bitsadmin /GetValidationState myJob 1
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)