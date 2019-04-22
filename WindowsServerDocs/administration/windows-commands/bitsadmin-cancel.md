---
title: bitsadmin cancel
description: Windows 命令主题**bitsadmin 取消**-从传输队列中删除作业并删除与作业关联的所有临时文件。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a4d1e2d6e4fd66cb525316f236d070fcd72d73f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814068"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

从传输队列中删除该作业并删除与作业关联的所有临时文件。

## <a name="syntax"></a>语法

```
bitsadmin /cancel <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例移除 *myDownloadJob* 从传输队列的作业。
```
C:\>bitsadmin /cancel myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)