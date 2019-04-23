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
ms.openlocfilehash: ed9518fae07745502d01dc0084b7443a1332db83
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859798"
---
# <a name="using-the-get-driverpackagefile-command"></a>使用 get DriverPackageFile 命令



显示有关驱动程序包，包括驱动程序和其包含的文件的信息。

## <a name="syntax"></a>语法

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/ InfFile:\<Inf 文件路径 >|指定驱动程序包.inf 文件的完整路径和文件名。|
|[/ 体系结构: {x86 | ia64 | x64}]|指定驱动程序包的体系结构。|
|[/ 显示: {驱动程序 | 文件 | All}]|指示要显示的包信息。 如果 **/ 显示**未指定，默认值为返回包的元数据的只有驱动程序。 **驱动程序**显示包中的驱动程序的列表。 **文件**包中显示的文件列表。 **所有**显示驱动程序和文件。|

## <a name="BKMK_examples"></a>示例

若要查看有关驱动程序文件的信息，请键入：
```
WDSUTIL /Get-DriverPackageFile /InfFile:"C:\temp\1394.inf" /Architecture:x86
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)