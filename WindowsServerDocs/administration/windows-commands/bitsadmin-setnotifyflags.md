---
title: bitsadmin setnotifyflags
description: Windows 命令主题**bitsadmin setnotifyflags** -将事件设置为指定的作业的通知标志。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc817e03e0f1916ea392830d14985a7a1377d69a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868788"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

将事件设置为指定的作业的通知标志。

## <a name="syntax"></a>语法

```
bitsadmin /SetNotifyFlags <Job> <NotifyFlags>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|NotifyFlags|请参阅备注|

## <a name="remarks"></a>备注

**NotifyFlags**参数可以包含一个或多个以下的通知标志。

|---|---| | 1 |生成事件时已传输作业中的所有文件。 || 2 |生成一个事件发生错误时。 || 4 |禁用通知。 |

## <a name="BKMK_examples"></a>示例

下面的示例设置的通知标志，用于传输和错误事件作业名为作业*myDownloadJob*。
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)