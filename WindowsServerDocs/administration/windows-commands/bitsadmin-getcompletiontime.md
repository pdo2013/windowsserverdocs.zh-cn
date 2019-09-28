---
title: bitsadmin getcompletiontime
description: 适用于**bitsadmin getcompletiontime**的 Windows 命令主题-检索作业传输数据完成的时间。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 190467d5f3a7b7244ed0d7ab3b75d4cbbf56c8d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381744"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime



检索作业传输数据完成的时间。

## <a name="syntax"></a>语法

```
bitsadmin /GetCompletionTime <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例将检索名为*myDownloadJob*的作业完成传输数据的时间。
```
C:\>bitsadmin /GetCompletionTime myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)