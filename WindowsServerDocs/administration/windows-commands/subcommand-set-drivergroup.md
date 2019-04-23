---
title: 子命令集 DriverGroup
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4ba9b1c-8c52-4fd5-969b-f7905611b364
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e645e16a3d78dd91bad98fedbb04896025b0eaf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852698"
---
# <a name="subcommand-set-drivergroup"></a>子命令： 集 DriverGroup

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在服务器上设置的现有驱动程序组的属性。
## <a name="syntax"></a>语法
```
wdsutil /Set-DriverGroup /DriverGroup:<Group Name> [/Server:<Server Name>] [/Name:<New Group Name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/DriverGroup:<Group Name>|指定的驱动程序组的名称。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果未指定服务器名称，则使用本地服务器。|
|[/Name:<New Group Name>]|指定驱动程序组的新名称。|
|[/ 已启用: {是&#124;无}|启用或禁用驱动程序组。|
|[/ 适用性: {匹配&#124;所有}]|指定要满足的筛选器条件安装的程序包。 **匹配**方法安装客户端的硬件匹配的驱动程序包。 **所有**意味着所有程序包都安装到客户端而不考虑其硬件。|
## <a name="BKMK_examples"></a>示例
若要设置的驱动程序组的属性，键入以下项之一：
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Name:colorprinterdrivers /Applicability:All
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[子命令： 集 DriverGroupFilter](subcommand-set-drivergroupfilter.md)
