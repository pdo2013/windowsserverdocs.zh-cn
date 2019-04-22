---
title: bitsadmin suspend
description: Windows 命令主题**bitsadmin 挂起**-挂起指定的作业。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87e1bbd1b068d68fb60655043735c6c1aeb07707
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825918"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> 适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

挂起指定的作业。

## <a name="syntax"></a>语法

```
bitsadmin /Suspend <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

若要重启该作业，请使用[bitsadmin 恢复](bitsadmin-resume.md)切换。

## <a name="BKMK_examples"></a>示例

下面的示例将名为的作业挂起*myDownloadJob*。

```
C:\>bitsadmin /Suspend myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
