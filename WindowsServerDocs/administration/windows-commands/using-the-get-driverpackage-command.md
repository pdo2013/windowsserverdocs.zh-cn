---
title: 使用 get DriverPackage 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0f123d281625140b3c4ba46316cb9b773bf5fee
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440515"
---
# <a name="using-the-get-driverpackage-command"></a>使用 get DriverPackage 命令



在服务器上将显示有关驱动程序包的信息。

## <a name="syntax"></a>语法

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parameters

|        参数         |                                                                           描述                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/ 服务器：\<服务器名称 >] |              指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果指定没有服务器名称，则使用本地服务器。               |
| [/DriverPackage:\<Name>] |                                                        指定要显示的驱动程序包的名称。                                                         |
|    [/PackageId:\<ID>]    | 指定 Windows 部署服务包的 ID 驱动程序以显示。 如果不能由名称唯一地标识驱动程序包，则必须指定 ID。 |
|     [/ 显示: {驱动程序     |                                                                              文件                                                                               |

## <a name="BKMK_examples"></a>示例

若要查看有关驱动程序包的信息，请键入以下项之一：
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)