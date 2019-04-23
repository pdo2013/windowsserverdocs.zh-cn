---
title: bitsadmin reset
description: Windows 命令主题**bitsadmin 重置**-取消当前用户拥有的传输队列中的所有作业。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b7c29aac55393cd87145583814b3ffa8f0a2c3b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874248"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

取消当前用户拥有的传输队列中的所有作业。

**BITSAdmin 1.5 和更早版本**: 如果您具有管理员特权 **重置** 取消队列中的所有作业。 不支持 /AllUsers 选项。

## <a name="syntax"></a>语法

```
bitsadmin /Reset [/AllUsers]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|AllUsers|可选-取消队列中的所有作业。|

## <a name="remarks"></a>备注

必须拥有管理员特权才能使用**AllUsers**参数。

> [!NOTE]
> 管理员不能重置由本地系统创建的作业。 使用任务计划程序安排此命令作为使用本地系统凭据的任务。

## <a name="BKMK_examples"></a>示例

下面的示例将取消当前用户的传输队列中的所有作业。
```
C:\>bitsadmin /Reset
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)