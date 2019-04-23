---
title: bitsadmin getmodificationtime
description: Windows 命令主题**bitsadmin getmodificationtime** -检索上次修改作业的时间或数据已成功传送。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4257f0ae4868b2f18221ab99268384f778c4bbbe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837018"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime



已成功传送检索上次修改作业的时间或数据。

## <a name="syntax"></a>语法

```
bitsadmin /GetModificationTime <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的上次修改时间 *myDownloadJob*。
```
C:\>bitsadmin /GetModificationTime myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)