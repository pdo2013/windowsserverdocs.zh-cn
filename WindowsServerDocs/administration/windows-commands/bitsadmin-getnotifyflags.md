---
title: bitsadmin getnotifyflags
description: 适用于**bitsadmin getnotifyflags**的 Windows 命令主题-检索指定作业的通知标志。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 56ee3a30050b6cc934b35bab24e9508911ea250e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381476"
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

|-----|-----| | 0x001 |当作业中的所有文件都已传输时生成事件。 || 0x002 |发生错误时生成事件。 || 0x004 |禁用通知。 || 0x008 |修改作业或传输进度时生成事件。 |

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的通知标志 *myDownloadJob*。
```
C:\>bitsadmin /GetNotifyFlags myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)