---
title: bitsadmin monitor
description: Windows 命令主题**bitsadmin 监视器**-监视当前用户拥有的传输队列中的作业。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c4620d5c8e46cb8bfcb6b9c83261d57781abea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814588"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor



监视作业的当前用户拥有的传输队列中。

## <a name="syntax"></a>语法

```
bitsadmin /Monitor [/allusers] [/refresh <Seconds>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|Allusers|可选-监视对所有用户的作业。|
|刷新|可选 — 通过指定的时间间隔刷新数据*秒*。 默认刷新间隔为五秒。|

## <a name="remarks"></a>备注

必须拥有管理员特权才能使用**Allusers**参数。

使用 CTRL + C 来停止刷新。

## <a name="BKMK_examples"></a>示例

下面的示例监视由当前用户拥有的作业的传输队列，并刷新信息每隔 60 秒。
```
C:\>bitsadmin /Monitor /refesh 60
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)