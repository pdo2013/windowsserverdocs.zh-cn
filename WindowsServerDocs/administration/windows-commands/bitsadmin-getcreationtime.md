---
title: bitsadmin getcreationtime
description: 适用于**bitsadmin getcreationtime**的 Windows 命令主题-检索指定作业的创建时间。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: be409cb5-ce72-41d9-aafa-edd4e230fd14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ea92133c90e20e37e5d281116e91bf1f109e83f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381690"
---
# <a name="bitsadmin-getcreationtime"></a>bitsadmin getcreationtime



检索指定作业的创建时间。

## <a name="syntax"></a>语法

```
bitsadmin /GetCreationTime <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为*myDownloadJob*的作业的创建时间。
```
C:\>bitsadmin /GetCreationTime myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)