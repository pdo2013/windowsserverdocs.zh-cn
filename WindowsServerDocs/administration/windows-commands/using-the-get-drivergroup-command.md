---
title: 使用 DriverGroup 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cfe10c3-a63f-48e7-bef9-f6b474b4ddbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fd8e4dd22e32722bbfe0dcdcfc79168ab7e3b72
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363183"
---
# <a name="using-the-get-drivergroup-command"></a>使用 DriverGroup 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示有关服务器上的驱动程序组的信息。
## <a name="syntax"></a>语法
```
wdsutil /Get-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/DriverGroup： <Group Name>|指定驱动程序组的名称。|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或 FQDN。  如果未指定服务器名称，则使用本地服务器。|
|[/Show： {PackageMetaData &#124; Filters &#124; All}]|显示指定组中所有驱动程序包的元数据。 **PackageMetaData**显示有关驱动程序组的所有筛选器的信息。 **筛选器**显示组的所有驱动程序包和筛选器的元数据。|
## <a name="BKMK_examples"></a>示例
若要查看有关驱动程序文件的信息，请键入：
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Show:PackageMetaData
```
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Server:MyWdsServer /Show:Filters
```
#### <a name="additional-references"></a>其他参考
[使用 AllDriverGroups 命令](using-the-get-alldrivergroups-command.md)
 的[命令行语法键](command-line-syntax-key.md)
