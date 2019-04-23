---
title: pnputil
description: 了解如何管理驱动程序存储区使用 pnputil.exe 实用程序。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5bde78d97be8def9f8594572869c34ef213db480
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862538"
---
# <a name="pnputil"></a>pnputil

Pnputil.exe 是一个命令行实用工具，可用于管理驱动程序存储区。 可以使用 Pnputil 将驱动程序包添加、 删除驱动程序包和应用商店中的列表驱动程序包。

## <a name="syntax"></a>语法

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-a|指定要添加标识的 INF 文件。|
|-d|指定要删除所标识的 INF 文件。|
|-e|指定要枚举所有第三方 INF 文件。|
|-f|指定要强制删除的标识的 INF 文件。 不能与结合使用 **– i**参数。|
|-i|指定在安装标识的 INF 文件。 不能与结合使用 **-f**参数。|
|/?|在命令提示符下显示帮助。|


## <a name="examples"></a>示例

-   -a pnputil.exe a:\usbcam\USBCAM。INF 添加指定的 USBCAM 的 INF 文件。INF
-   pnputil.exe-a c:\drivers\*.inf c:\drivers\ 中添加所有 INF 文件
-   pnputil.exe-i-a a:\usbcam\USBCAM。添加了 INF 和安装指定的驱动程序。
-   pnputil.exe – e 枚举所有第三方驱动程序。
-   pnputil.exe-d 为 oem0.inf 删除指定。
-   pnputil.exe-f-d 为 oem0.inf 强制删除指定的 INF 文件。

## <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

[Popd](popd.md)