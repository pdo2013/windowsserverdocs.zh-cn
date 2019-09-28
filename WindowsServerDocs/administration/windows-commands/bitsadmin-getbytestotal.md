---
title: bitsadmin getbytestotal
description: 适用于**bitsadmin getbytestotal**的 Windows 命令主题-检索指定作业的大小。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38b0f09e13919e0c75d2b7429dd66f765b434de5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381754"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal



检索指定的作业的大小

## <a name="syntax"></a>语法

```
bitsadmin /GetBytesTotal <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的大小 *myDownloadJob*。
```
C:\>bitsadmin /GetBytesTotal myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)