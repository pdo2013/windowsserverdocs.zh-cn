---
title: 使用 DriverPackage 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 923a86805134c4162b36cdade98c2122b3cb7ccd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362807"
---
# <a name="using-the-remove-driverpackage-command"></a>使用 DriverPackage 命令

> 适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012
> 
> 
> 适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

从服务器中删除驱动程序包。
## <a name="syntax"></a>语法
```
wdsutil /remove-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Parameters

|        参数        |                                                                            描述                                                                             |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server： @no__t] |              指定服务器的名称。 此名称可以是 NetBIOS 名称或 FQDN。 如果未指定服务器名称，则使用本地服务器。              |
| [/DriverPackage： @no__t] |                                                        指定要删除的驱动程序包的名称。                                                         |
|    [/PackageId： @no__t]    | 指定要删除的驱动程序包的 Windows 部署服务 ID。 如果无法按名称唯一地标识驱动程序包，则必须指定 ID。 |

## <a name="BKMK_examples"></a>示例
若要查看有关图像的信息，请键入下列内容之一：
```
wdsutil /remove-DriverPackage /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /remove-DriverPackage /Server:MyWdsServer /DriverPackage:MyDriverPackage
```
#### <a name="additional-references"></a>其他参考
[使用 DriverPackages 命令](using-the-remove-driverpackages-command.md)
 的[命令行语法键](command-line-syntax-key.md)
