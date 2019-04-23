---
title: bitsadmin getowner
description: Windows 命令主题**bitsadmin getowner** -检索指定的作业的所有者。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1381bc1268b2b81e2bde18d0d8e17bd760345e0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886708"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

显示的显示名称或指定的作业的所有者的 GUID。

## <a name="syntax"></a>语法

```
bitsadmin /GetOwner <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例显示名为的作业的所有者 *myDownloadJob*。
```
C:\>bitsadmin /GetOwner myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)