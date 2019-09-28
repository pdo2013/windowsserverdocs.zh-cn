---
title: pnputil
description: 了解如何通过 pnputil 实用工具管理驱动程序存储区。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fab686b8-09d3-4f6c-afa2-630e6036f44c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: f20c60bfd9ae33497dd356c7797b9fb1d2b51d18
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372292"
---
# <a name="pnputil"></a>pnputil

Pnputil 是一个命令行实用工具，可用于管理驱动程序存储区。 你可以使用 Pnputil 来添加驱动程序包、删除驱动程序包，以及列出存储中的驱动程序包。

## <a name="syntax"></a>语法

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-a|指定添加标识的 INF 文件。|
|-d.ddd...e|指定删除标识的 INF 文件。|
|-e|指定枚举所有第三方 INF 文件。|
|-f|指定强制删除标识的 INF 文件。 不能与 **– i**参数一起使用。|
|-i|指定安装标识的 INF 文件。 不能与 **-f**参数一起使用。|
|/?|在命令提示符下显示帮助。|


## <a name="examples"></a>示例

-   pnputil-a a:\usbcam\USBCAM。INF 会添加 USBCAM 指定的 INF 文件。遵从
-   pnputil-c：\ drivers\*.inf 在 c:\drivers\ 中添加所有 INF 文件
-   pnputil-i-a a:\usbcam\USBCAM。INF 添加并安装指定的驱动程序。
-   pnputil – e 枚举所有第三方驱动程序。
-   pnputil-d 为 oem0.inf 删除指定的。
-   pnputil-f-d 为 oem0.inf 强制删除指定的 INF 文件。

## <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

[Popd](popd.md)