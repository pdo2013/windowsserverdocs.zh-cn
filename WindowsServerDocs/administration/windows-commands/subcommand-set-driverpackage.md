---
title: 子命令集 DriverPackage
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 11804bb6-ca29-4461-8c63-5131748cd742
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 24acd672184b8df235e8de843961ac4adb2bd412
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441131"
---
# <a name="subcommand-set-driverpackage"></a>Subcommand: set-DriverPackage



重命名和/或启用或禁用服务器上的驱动程序包。

## <a name="syntax"></a>语法

```
WDSUTIL /Set-DriverPackage [/Server:<Server name>] {/DriverPackage:<Name> | /PackageId:<ID>} [/Name:<New Name>] [/Enabled:{Yes | No}
```

## <a name="parameters"></a>Parameters

|        参数         |                                                                                                                                                                                                               描述                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/ 服务器：\<服务器名称 >] |                                                                                                                                                 指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果未指定服务器名称，则使用本地服务器。                                                                                                                                                 |
| [/DriverPackage:\<Name>] |                                                                                                                                                                                       指定要修改的驱动程序包的当前名称。                                                                                                                                                                                        |
|    [/PackageId:\<ID>]    | 指定驱动程序包的 Windows 部署服务 ID。 如果不能由名称唯一地标识驱动程序包，必须指定此选项。 若要查找此 ID 的包，请单击包中的驱动程序组 (或**的所有包**节点)，右键单击包，然后单击**属性**。 上列出包 ID**常规**选项卡。例如: {DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}。 |
|   [/Name:\<新名称 >]    |                                                                                                                                                                                              指定驱动程序包的新名称。                                                                                                                                                                                              |
|      [/ 已启用: {是      |                                                                                                                                                                                                                   无}                                                                                                                                                                                                                    |

## <a name="BKMK_examples"></a>示例

若要更改有关包的设置，请键入以下项之一：
```
WDSUTIL /Set-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318} /Name:MyDriverPackage
```
```
WDSUTIL /Set-DriverPackage /DriverPackage:MyDriverPackage /Name:NewName /Enabled:Yes
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)