---
title: bitsadmin wrap
description: Windows 命令主题 for **bitsadmin wrap** -将超出命令窗口最右边边缘的任意行输出文本包装到下一行。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5609fb6f38716795a545e0c7fe3939f893a8c8d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380678"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

对输出进行包装以适应命令窗口。

## <a name="syntax"></a>语法

```
bitsadmin /Wrap Job
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

在其他开关之前指定。 默认情况下，除[bitsadmin 监视器](bitsadmin-monitor.md)开关外，所有交换机都将输出换行。

## <a name="BKMK_examples"></a>示例

下面的示例将检索名为*myDownloadJob*的作业的信息，并对输出进行包装。

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
