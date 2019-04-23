---
title: bitsadmin setpriority
description: Windows 命令主题**bitsadmin setpriority** -设置指定的作业的优先级。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 072f22ae8c928d427104062b8cbf0f8f42ac4416
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882208"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority



指定的作业的优先级设置。

## <a name="syntax"></a>语法

```
bitsadmin /SetPriority <Job> <Priority>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|Priority|以下值之一：</br>的前景色</br>-高</br>-正常</br>-中低|

## <a name="BKMK_examples"></a>示例

下面的示例设置名为的作业优先级*myDownloadJob*正常状态。
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)