---
title: bitsadmin getstate
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f7ed7529fda264efaceb6b4b36e36e728c211f3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889618"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate



检索指定的作业的状态。

## <a name="syntax"></a>语法

```
bitsadmin /GetState <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

可能的状态包括：

|-----|-----| |排入队列 |作业正在等待运行。 ||连接 |BITS 联系服务器。 ||传输 |BITS 将数据传输。 ||挂起 |作业已暂停。 ||错误 |发生不可恢复的错误;将不会重新传输。 ||TRANSIENT_ERROR |出现可恢复错误则传输重试的最小重试延迟时间过期时。 ||确认 |完成作业。 ||取消 |已取消该作业。 |

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业状态*myDownloadJob*。
```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)