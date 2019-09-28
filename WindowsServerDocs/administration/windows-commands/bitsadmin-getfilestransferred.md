---
title: bitsadmin getfilestransferred
description: 适用于**bitsadmin getfilestransferred**的 Windows 命令主题-检索为指定作业传输的文件数。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d02d9d7bc216a5ad7ca922e716c368f64c4b9a44
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381599"
---
# <a name="bitsadmin-getfilestransferred"></a>bitsadmin getfilestransferred



检索指定的作业传输的文件数。

## <a name="syntax"></a>语法

```
bitsadmin /GetFilesTransferred <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为作业中传输的文件数 *myDownloadJob*。
```
C:\>bitsadmin /GetFilesTransferred myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)