---
title: 使用 DriverPackageFile 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 21bbe17e56177da5cd2c1bf83c712d256cc794c8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363146"
---
# <a name="using-the-get-driverpackagefile-command"></a>使用 DriverPackageFile 命令



显示有关驱动程序包的信息，包括其包含的驱动程序和文件。

## <a name="syntax"></a>语法

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parameters

|         参数         |                              描述                               |
|---------------------------|------------------------------------------------------------------------|
| /InfFile： \<Inf 文件路径 > | 指定驱动程序包 .inf 文件的完整路径和文件名。 |
|    [/Architecture： {x86    |                                  ia64                                  |
|     [/Show： {驱动程序      |                                 文件                                  |

## <a name="BKMK_examples"></a>示例

若要查看有关驱动程序文件的信息，请键入：
```
WDSUTIL /Get-DriverPackageFile /InfFile:"C:\temp\1394.inf" /Architecture:x86
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)