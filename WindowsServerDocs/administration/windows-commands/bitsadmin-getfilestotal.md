---
title: bitsadmin getfilestotal
description: 适用于**bitsadmin getfilestotal**的 Windows 命令主题-检索指定作业中的文件数。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27cf04e8745aeab5cd1f2ce379c8506be642fea2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381615"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal



检索指定的作业中的文件数。

## <a name="syntax"></a>语法

```
bitsadmin /GetFilesTotal <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为作业中包含的文件数 *myDownloadJob*。
```
C:\>bitsadmin /GetFilesTotal myDownloadJob
```

# #

[命令行语法关键字](command-line-syntax-key.md)另请参阅