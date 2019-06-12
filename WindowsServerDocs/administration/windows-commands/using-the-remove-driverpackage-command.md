---
title: 使用删除 DriverPackage 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b201e91-0d44-4e4a-8252-8b0235df1002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 217ff23b8724464670520d0b2d5b196df5a4af47
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440298"
---
# <a name="using-the-remove-driverpackage-command"></a>使用删除 DriverPackage 命令

> 适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012
> 
> 
> 适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

从服务器中删除驱动程序包。
## <a name="syntax"></a>语法
```
wdsutil /remove-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Parameters

|        参数        |                                                                            描述                                                                             |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/ 服务器：<Server name>] |              指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果未指定服务器名称，则使用本地服务器。              |
| [/DriverPackage:<Name>] |                                                        指定要删除的驱动程序包的名称。                                                         |
|    [/PackageId:<ID>]    | 指定 Windows 部署服务驱动程序包的要删除的 ID。 如果不能由名称唯一地标识驱动程序包，则必须指定 ID。 |

## <a name="BKMK_examples"></a>示例
若要查看有关映像的信息，请键入以下项之一：
```
wdsutil /remove-DriverPackage /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /remove-DriverPackage /Server:MyWdsServer /DriverPackage:MyDriverPackage
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用删除 DriverPackages 命令](using-the-remove-driverpackages-command.md)
