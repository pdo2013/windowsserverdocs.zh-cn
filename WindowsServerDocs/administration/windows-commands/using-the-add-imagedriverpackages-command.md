---
title: 使用添加 ImageDriverPackages 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9dc78909-a4d1-42a2-af8f-21ebcbfe8302
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55b26acf9c4006db3d64e27be8a10e158876ddc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817358"
---
# <a name="using-the-add-imagedriverpackages-command"></a>使用添加 ImageDriverPackages 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将驱动程序包从驱动程序存储区添加到启动映像。 映像版本必须是 Windows 7 或 Windows Server 2008 R2 或更高版本。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
## <a name="syntax"></a>语法
```
wdsutil /add-ImageDriverPackages [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} 
[/Filename:<File name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value> ...]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/ 服务器：<Server name>|指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果指定没有服务器名称，则使用本地服务器。|
媒体：<Image name>|指定要添加到驱动程序的图像的名称。|
mediatype:Boot|指定图像添加到驱动程序的类型。 可以添加驱动程序包，只到启动映像。|
|/ 体系结构: {x86 &#124; ia64 &#124; x64}|指定启动映像的体系结构。 因为它是可以在不同的体系结构中包含的启动映像的映像名称，则应指定体系结构，以确保使用正确的映像。|
|/ 文件名：<File name>|指定的文件的名称。 如果名称不能唯一标识该映像，必须指定的文件的名称。|
|/ 筛选器类型：<Filter type>|指定要搜索的驱动程序包的特性。 您可以在单个命令中指定多个属性。 此外必须指定 **/Operator**并 **/值**使用此选项。<br /><br /><Filter type> 可以是以下值之一：<br /><br />**PackageId**<br /><br />**PackageName**<br /><br />**PackageEnabled**<br /><br />**Packagedateadded**<br /><br />**PackageInfFilename**<br /><br />**PackageClass**<br /><br />**PackageProvider**<br /><br />**PackageArchitecture**<br /><br />**PackageLocale**<br /><br />**PackageSigned**<br /><br />**PackagedatePublished**<br /><br />**Packageversion**<br /><br />**Driverdescription**<br /><br />**DriverManufacturer**<br /><br />**DriverHardwareId**<br /><br />**DrivercompatibleId**<br /><br />**DriverExcludeId**<br /><br />**DriverGroupId**<br /><br />**DriverGroupName**|
|/Operator:{Equal &#124; NotEqual &#124; GreaterOrEqual &#124; LessOrEqual &#124; Contains}|该属性和值之间的关系。 仅可以指定**Contains**字符的字符串属性。 仅可以指定**GreaterOrEqual**并**LessOrEqual**具有日期和版本属性。|
|/ 值：<Value>|指定要搜索的相对于指定的值<attribute>。 可以指定多个值的单个 **/Filtertype**。 下表列出了可以为每个筛选器指定的属性。 有关这些属性的详细信息，请参阅[驱动程序和包属性](https://go.microsoft.com/fwlink/?LinkId=166895)(https://go.microsoft.com/fwlink/?LinkId=166895)。<br /><br />-PackageId-指定有效的 GUID。 例如: {4d36e972-e325-11ce-bfc1-08002be10318}。<br />-PackageName 指定任意字符串值。<br />-PackageEnabled-指定**是**或**否**。<br />-Packagedateadded-采用以下格式指定日期：YYYY/MM/DD<br />-PackageInfFilename 指定任意字符串值。<br />-PackageClass-指定有效的类名称或类 GUID。 例如：**磁盘驱动器**， **Net**，或 {4d36e972-e325-11ce-bfc1-08002be10318}。<br />-PackageProvider 指定任意字符串值。<br />-PackageArchitecture-指定**x86**， **x64**，或**ia64**。<b />-PackageLocale-指定有效的语言标识符。 例如： **EN-US**或**ES-ES**。<br />-PackageSigned-指定**是**或**否**。<br />-PackagedatePublished-采用以下格式指定日期：YYYY/MM/DD<br />-Packageversion-采用以下格式指定的版本： a.b.x.y. 例如：6.1.0.0<br />-Driverdescription 指定任意字符串值。<br />-DriverManufacturer 指定任意字符串值。<br />-DriverHardwareId-指定任何字符串值。<br />-DrivercompatibleId-指定任何字符串值。<br />-DriverExcludeId-指定任何字符串值。<br />-DriverGroupId-指定有效的 GUID。 例如: {4d36e972-e325-11ce-bfc1-08002be10318}。<br />-DriverGroupName 指定任意字符串值。|
## <a name="BKMK_examples"></a>示例
若要将驱动程序包添加到启动映像中，键入以下项之一：
```
wdsutil /add-ImageDriverPackagemedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /Filtertype:DriverGroupName /Operator:Equal /Value:x86Bus /Filtertype:PackageProvider /Operator:Contains /Value:Provider1 /Filtertype:Packageversion /Operator:GreaterOrEqual /Value:6.1.0.0
```
```
wdsutil /verbose /add-ImageDriverPackagemedia: "WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```
```
wdsutil /verbose /add-ImageDriverPackagemedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Value:System /Value:DiskDrive /Value:HDC /Value:SCSIAdapter
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加 ImageDriverPackage 命令](using-the-add-imagedriverpackage-command.md)
[使用添加 AllDriverPackages 子命令](using-the-add-alldriverpackages-subcommand.md)
