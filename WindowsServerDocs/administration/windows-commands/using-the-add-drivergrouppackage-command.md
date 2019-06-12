---
title: 使用添加 DriverGroupPackage 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cd323ae-9049-448e-a460-6c7d6462d4c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad9d180e2cf2110d36ebc82211af3a495a0e0b6b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440728"
---
# <a name="using-the-add-drivergrouppackage-command"></a>使用添加 DriverGroupPackage 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将驱动程序包添加到驱动程序组。
## <a name="syntax"></a>语法
```
wdsutil /add-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Parameters

|         参数         |                                                                                                                                               描述                                                                                                                                               |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup:<Group Name> |                                                                                                                                 指定的驱动程序组的名称。                                                                                                                                 |
|   / 服务器：<Server name>   |                                                                                  指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果指定没有服务器名称，则使用本地服务器。                                                                                  |
|   /DriverPackage:<Name>   |                                                                      指定要添加到组的驱动程序包的名称。 如果不能由名称唯一地标识驱动程序包，必须指定此选项。                                                                       |
|      /PackageId:<ID>      | 指定包的 ID。 若要查找程序包 ID，请单击包中的驱动程序组 (或**的所有包**节点)，右键单击包，然后单击**属性**。 上列出包 ID**常规**选项卡中，例如： **{DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}** 。 |

## <a name="BKMK_examples"></a>示例
若要添加驱动程序包，请键入以下项之一：
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /DriverPackage:XYZ
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加 DriverGroupPackages 命令](using-the-add-drivergrouppackages-command.md)
[使用添加 DriverPackage 命令](using-the-add-driverpackage-command.md)
 [使用添加 AllDriverPackages 子命令](using-the-add-alldriverpackages-subcommand.md)
