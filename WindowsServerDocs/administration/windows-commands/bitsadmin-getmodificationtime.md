---
title: bitsadmin getmodificationtime
description: 适用于**bitsadmin getmodificationtime**的 Windows 命令主题-检索上次修改作业或成功传输数据的时间。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 48b4d252ce6161b288726190f41f08c64efdbcf2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381529"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime



已成功传送检索上次修改作业的时间或数据。

## <a name="syntax"></a>语法

```
bitsadmin /GetModificationTime <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的上次修改时间 *myDownloadJob*。
```
C:\>bitsadmin /GetModificationTime myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)