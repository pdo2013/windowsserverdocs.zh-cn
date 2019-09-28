---
title: bitsadmin suspend
description: 用于**bitsadmin 挂起**的 Windows 命令主题-挂起指定的作业。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a3a484df2b50cdc8893512020b835f913793d2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380379"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> 适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

挂起指定的作业。

## <a name="syntax"></a>语法

```
bitsadmin /Suspend <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

若要重新启动作业，请使用[bitsadmin resume](bitsadmin-resume.md)开关。

## <a name="BKMK_examples"></a>示例

以下示例挂起名为*myDownloadJob*的作业。

```
C:\>bitsadmin /Suspend myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
