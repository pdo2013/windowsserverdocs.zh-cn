---
title: bitsadmin monitor
description: '**Bitsadmin monitor**的 Windows 命令主题-监视当前用户拥有的传输队列中的作业。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fe4963349c7e17fc77500b5adfceafc48a20ac5f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381219"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor



监视当前用户拥有的传输队列中的作业。

## <a name="syntax"></a>语法

```
bitsadmin /Monitor [/allusers] [/refresh <Seconds>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|Allusers|可选-监视所有用户的作业。|
|刷新|可选—按*秒*指定的间隔刷新数据。 默认刷新间隔为5秒。|

## <a name="remarks"></a>备注

您必须具有管理员特权才能使用**Allusers**参数。

使用 CTRL + C 停止刷新。

## <a name="BKMK_examples"></a>示例

以下示例监视当前用户拥有的作业的传输队列，每60秒刷新一次信息。
```
C:\>bitsadmin /Monitor /refesh 60
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)