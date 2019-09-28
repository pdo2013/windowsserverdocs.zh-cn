---
title: bitsadmin getcustomheaders
description: 适用于**bitsadmin getcustomheaders**的 Windows 命令主题-从作业检索自定义 HTTP 标头。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 039669fca42803ff22eb4e3d13dfdef5f0a06f93
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381657"
---
# <a name="bitsadmin-getcustomheaders"></a>bitsadmin getcustomheaders



从作业检索自定义 HTTP 标头。

## <a name="syntax"></a>语法

```
bitsadmin /GetCustomHeaders <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例获取名为*myDownloadJob*的作业的自定义标头。
```
C:\>bitsadmin /GetCustomHeaders myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)