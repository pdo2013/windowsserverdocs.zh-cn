---
title: bitsadmin getfilestransferred
description: Windows 命令主题**bitsadmin getfilestransferred** -检索为指定的作业传输的文件数。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5df7f2abfdad6780878b1f00da44c772eecf9fba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822258"
---
# <a name="bitsadmin-getfilestransferred"></a>bitsadmin getfilestransferred



检索指定的作业传输的文件数。

## <a name="syntax"></a>语法

```
bitsadmin /GetFilesTransferred <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为作业中传输的文件数 *myDownloadJob*。
```
C:\>bitsadmin /GetFilesTransferred myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)