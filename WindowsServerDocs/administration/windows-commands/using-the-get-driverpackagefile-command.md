---
title: 使用 get DriverPackageFile 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 264bdb6d51622e6323be00b44014b86cd9662e61
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440500"
---
# <a name="using-the-get-driverpackagefile-command"></a>使用 get DriverPackageFile 命令



显示有关驱动程序包，包括驱动程序和其包含的文件的信息。

## <a name="syntax"></a>语法

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parameters

|         参数         |                              描述                               |
|---------------------------|------------------------------------------------------------------------|
| / InfFile:\<Inf 文件路径 > | 指定驱动程序包.inf 文件的完整路径和文件名。 |
|    [/ 体系结构: {x86    |                                  ia64                                  |
|     [/ 显示: {驱动程序      |                                 文件                                  |

## <a name="BKMK_examples"></a>示例

若要查看有关驱动程序文件的信息，请键入：
```
WDSUTIL /Get-DriverPackageFile /InfFile:"C:\temp\1394.inf" /Architecture:x86
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)