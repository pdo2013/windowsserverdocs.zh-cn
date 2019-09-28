---
title: bitsadmin setpriority
description: 适用于**bitsadmin setpriority**的 Windows 命令主题-设置指定作业的优先级。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60564350928f917ca1861684e042304d5d380426
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380439"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority



设置指定作业的优先级。

## <a name="syntax"></a>语法

```
bitsadmin /SetPriority <Job> <Priority>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|Priority|以下值之一：</br>-前景</br>-高</br>-正常</br>-低|

## <a name="BKMK_examples"></a>示例

下面的示例将名为*myDownloadJob*的作业的优先级设置为 normal。
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)