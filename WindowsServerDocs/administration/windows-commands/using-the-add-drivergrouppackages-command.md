---
title: 使用添加 DriverGroupPackages 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29022f53-ce14-4b2d-a81a-679c18e022b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad0f8c7ed202f0d6fccc11307b17b742c70c3e49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842788"
---
# <a name="using-the-add-drivergrouppackages-command"></a>使用添加 DriverGroupPackages 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
## <a name="syntax"></a>语法
```
wdsutil /add-DriverGroupPackages /DriverGroup:<Group Name> [/Server:<Server Name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value>
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/DriverGroup:<Group Name>|指定的驱动程序组的名称。|
|/ 服务器：<Server name>|指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果指定没有服务器名称，则使用本地服务器。|
|/ 筛选器类型：<Filter type>|指定要搜索的驱动程序包的特性。 您可以在单个命令中指定多个属性。 此外必须使用此选项指定 /Operator 和 /Value。<br /><br /><Filter type> 可以是以下值之一：<br /><br />**PackageId**<br /><br />**PackageName**<br /><br />**PackageEnabled**<br /><br />**Packagedateadded**<br /><br />**PackageInfFilename**<br /><br />**PackageClass**<br /><br />**PackageProvider**<br /><br />**PackageArchitecture**<br /><br />**PackageLocale**<br /><br />**PackageSigned**<br /><br />**PackagedatePublished**<br /><br />**Packageversion**<br /><br />**Driverdescription**<br /><br />**DriverManufacturer**<br /><br />**DriverHardwareId**<br /><br />**DrivercompatibleId**<br /><br />**DriverExcludeId**<br /><br />**DriverGroupId**<br /><br />**DriverGroupName**|
|/Operator:{Equal &#124; NotEqual &#124; GreaterOrEqual &#124; LessOrEqual &#124; Contains}|指定的属性和值之间的关系。 仅可以指定**Contains**字符的字符串属性。 仅可以指定**相等**， **NotEqual**， **GreaterOrEqual**并**LessOrEqual**具有日期和版本属性。|
|/ 值：<Value>|指定与对应的客户端值 **/Filtertype**。 可以指定多个值的单个 **/Filtertype**。 以下列表概述了可以为每个筛选器指定的值。 有关这些值的详细信息，请参阅[驱动程序和包属性](https://go.microsoft.com/fwlink/?LinkId=166895)(https://go.microsoft.com/fwlink/?LinkId=166895)。<br /><br />-PackageId-指定有效的 GUID。 例如: {4d36e972-e325-11ce-bfc1-08002be10318}。<br />-PackageName 指定任意字符串值。<br />-PackageEnabled-指定**是**或**否**。<br />-Packagedateadded-采用以下格式指定日期：YYYY/MM/DD<br />-PackageInfFilename 指定任意字符串值。<br />-PackageClass-指定有效的类名称或类 GUID。 例如：**磁盘驱动器**， **Net**，或 {4d36e972-e325-11ce-bfc1-08002be10318}。<br />-PackageProvider 指定任意字符串值。<br />-PackageArchitecture-指定**x86**， **x64**，或**ia64**。<br />-PackageLoale-指定有效的语言标识符。 例如： **EN-US**或**ES-ES**。<br />-PackageSigned-指定**是**或**否**。<br />-PackagedatePublished-采用以下格式指定日期：YYYY/MM/DD<br />-Packageversion-采用以下格式指定的版本： a.b.x.y. 例如：6.1.0.0<br />-Driverdescription 指定任意字符串值。<br />-DriverManufacturer 指定任意字符串值。<br />-DriverHardwareId-指定任何字符串值。<br />-DrivercompatibleId-指定任何字符串值。<br />-DriverExcludeId-指定任何字符串值。<br />-DriverGroupId-指定有效的 GUID。 例如: {4d36e972-e325-11ce-bfc1-08002be10318}。<br />-DriverGroupName 指定任意字符串值。|
## <a name="BKMK_examples"></a>示例
若要添加驱动程序包，请键入以下项之一：
```
wdsutil /verbose /add-DriverGroupPackages /DriverGroup:printerdrivers /Filtertype:PackageClass /Operator:Equal /Value:printer /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2
```
```
wdsutil /verbose /add-DriverGroupPackages /DriverGroup:DisplayDriversX86 /Filtertype:PackageClass /Operator:Equal /Value:Display /Filtertype:PackageArchitecture /Operator:Equal /Value:x86 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加 DriverGroupPackage 命令](using-the-add-drivergrouppackage-command.md)
[使用添加 DriverPackage 命令](using-the-add-driverpackage-command.md)
 [使用添加 AllDriverPackages 子命令](using-the-add-alldriverpackages-subcommand.md)
