---
title: bitsadmin setvalidationstate
description: Windows 命令主题**bitsadmin setvalidationstate** -设置在作业中的给定文件的内容验证状态。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 647389aaac06d1eb109052548c1b24f7579bde2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851238"
---
# <a name="bitsadmin-setvalidationstate"></a>bitsadmin setvalidationstate



设置作业中的给定文件的内容验证状态。

## <a name="syntax"></a>语法

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|文件索引|从 0 开始|
|True|False|设置为 TRUE 的文件内容是否有效，否则设置为 FALSE|

## <a name="BKMK_examples"></a>示例

下面的示例将文件 2 的内容验证状态设置为 TRUE 名为的作业*myJob*。
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)