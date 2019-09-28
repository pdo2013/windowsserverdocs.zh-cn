---
title: bitsadmin reset
description: 用于**bitsadmin reset**的 Windows 命令主题-取消当前用户拥有的传输队列中的所有作业。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: adc6b07a7b5d1414c733fe6a3ac05eba7cb3029e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380812"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

取消当前用户拥有的传输队列中的所有作业。

**BITSAdmin 1.5 及更早版本**： 如果你具有管理员权限，请 **重置** cancels 队列中的所有作业。 不支持/AllUsers 选项。

## <a name="syntax"></a>语法

```
bitsadmin /Reset [/AllUsers]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|AllUsers|可选-取消队列中的所有作业。|

## <a name="remarks"></a>备注

您必须具有管理员特权才能使用**AllUsers**参数。

> [!NOTE]
> 管理员不能重置本地系统创建的作业。 使用任务计划程序将此命令计划为使用本地系统凭据的任务。

## <a name="BKMK_examples"></a>示例

下面的示例取消当前用户在传输队列中的所有作业。
```
C:\>bitsadmin /Reset
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)