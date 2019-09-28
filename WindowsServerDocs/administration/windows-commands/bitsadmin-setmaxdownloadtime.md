---
title: bitsadmin setmaxdownloadtime
description: '**Bitsadmin setmaxdownloadtime**的 Windows 命令主题-设置下载超时（以秒为单位）。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 985453de5bd2f4a06b5635ae5b0a9794d30175b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380557"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>bitsadmin setmaxdownloadtime



设置下载超时（以秒为单位）。

## <a name="syntax"></a>语法

```
bitsadmin /SetMaxDownloadTime <Job> <Timeout>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|Timeout|超时（秒）|

## <a name="remarks"></a>备注

-   不可用

## <a name="BKMK_examples"></a>示例

下面的示例将名为*myDownloadJob*的作业的超时值设置为10秒。
```
C:\>bitsadmin /SetMaxDownloadTime myDownloadJob 10
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)