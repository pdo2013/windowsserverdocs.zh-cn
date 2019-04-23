---
title: bitsadmin gettemporaryname
description: Windows 命令主题**bitsadmin gettemporaryname** -报告作业中的给定文件的临时文件名。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 762a2a5943202b38e94a245b74745e6631e0792d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876708"
---
# <a name="bitsadmin-gettemporaryname"></a>bitsadmin gettemporaryname



报告作业中的给定文件的临时文件名。

## <a name="syntax"></a>语法

```
bitsadmin /GetTemporaryName <Job> <file index> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|文件索引|从 0 开始|

## <a name="BKMK_examples"></a>示例

下面的示例报告文件 2 名为的作业的临时文件名*myJob*。
```
C:\>bitsadmin /GetTemporaryName myJob 1 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)