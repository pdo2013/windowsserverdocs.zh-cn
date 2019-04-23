---
title: bitsadmin getnotifyflags
description: Windows 命令主题**bitsadmin getnotifyflags** -检索指定的作业的通知标志。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 690e94805c5e61d96603e4ade102fb3a4bda409e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889278"
---
# <a name="bitsadmin-getnotifyflags"></a>bitsadmin getnotifyflags



检索指定的作业的通知标志。

## <a name="syntax"></a>语法

```
bitsadmin /GetNotifyFlags <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

作业可以包含一个或多个以下的通知标志。

|---|---| | 0x001 |生成事件时已传输作业中的所有文件。 || 为 0x002 |生成一个事件发生错误时。 || 0x004 |禁用通知。 || 0x008 |生成时修改作业或使传输进度事件。 |

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的通知标志 *myDownloadJob*。
```
C:\>bitsadmin /GetNotifyFlags myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)