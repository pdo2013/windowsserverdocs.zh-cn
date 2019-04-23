---
title: bitsadmin getcompletiontime
description: Windows 命令主题**bitsadmin getcompletiontime** -检索该作业已完成传输数据的时间。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3790a91c4b347b982c0f0a023d5977a8d6cd1f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857378"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime



检索作业已完成传输数据的时间。

## <a name="syntax"></a>语法

```
bitsadmin /GetCompletionTime <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为作业的时间*myDownloadJob*已完成传输数据。
```
C:\>bitsadmin /GetCompletionTime myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)