---
title: bitsadmin setnotifyflags
description: 适用于**bitsadmin setnotifyflags**的 Windows 命令主题-设置指定作业的事件通知标志。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d9cfabf05610cbbe8fa65fd16b0d33e161dcef9b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380448"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

设置指定作业的事件通知标志。

## <a name="syntax"></a>语法

```
bitsadmin /SetNotifyFlags <Job> <NotifyFlags>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|NotifyFlags|请参阅 "备注"|

## <a name="remarks"></a>备注

**NotifyFlags**参数可以包含以下一个或多个通知标志。

|-----|-----| | 1 |当作业中的所有文件都已传输时生成事件。 || 2 |发生错误时生成事件。 || 4 |禁用通知。 |

## <a name="BKMK_examples"></a>示例

下面的示例为 "已传输" 和 "错误事件" 作业设置名为 " *myDownloadJob*" 的通知标志。
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)