---
title: bitsadmin getstate
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55be37a6b1b44b81ed9002e5e3b9eb1fd46bd0dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381231"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate



检索指定作业的状态。

## <a name="syntax"></a>语法

```
bitsadmin /GetState <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

可能的状态为：

|-----|-----| |排队 |作业正在等待运行。 ||正在连接 |BITS 正在联系服务器。 ||正在传输 |BITS 正在传输数据。 ||挂起 |作业已暂停。 ||错误 |发生了不可恢复的错误;将不会重试传输。 ||TRANSIENT_ERROR |发生了可恢复的错误;最小重试延迟到期时，将重试传输。 ||已确认 |作业已完成。 ||已取消 |作业已取消。 |

## <a name="BKMK_examples"></a>示例

下面的示例将检索名为*myDownloadJob*的作业的状态。
```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)