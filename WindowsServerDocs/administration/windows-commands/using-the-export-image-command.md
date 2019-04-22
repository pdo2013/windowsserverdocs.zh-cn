---
title: 使用导出映像命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a9b8b467-0f2d-4754-8998-55503a262778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: efa9b2d09c37a383a91883ee02c995eedb2f235e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823138"
---
# <a name="using-the-export-image-command"></a>使用导出映像命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将现有映像从映像存储区导出到另一个 Windows 映像 (.wim) 文件。
## <a name="syntax"></a>语法
对于启动映像：
```
wdsutil [Options] /Export-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No}]
```
对于安装映像：
```
wdsutil [Options] /Export-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:InstallmediaGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No | append}]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
媒体：<Image name>|指定要导出的图像的名称。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
媒体类型: {启动&#124;安装}|指定要导出的图像的类型。|
|\mediaGroup:<Image group name>]|指定包含图像的图像组要导出。 如果未指定任何映像组名称和服务器上只有一个映像组存在，则将默认使用该映像组。 如果在服务器上存在多个映像组，必须指定映像组。|
|/ 体系结构: {x86 &#124; ia64 &#124; x64}|指定要导出的图像的体系结构。 因为它是可以在不同的体系结构中包含的启动映像的映像名称，指定体系结构值可确保将返回正确的映像。|
|[/ Filename:<Filename>]|如果名称不能唯一标识该映像，必须指定的文件的名称。|
|/DestinationImage|指定目标图像的设置。 您可以指定这些设置，请使用以下选项：<br /><br />-/Filepath:<File path and name> -指定新的映像的完整文件路径。<br />-[/Name:<Name>]-设置图像的显示名称。 如果未不指定任何名称，将使用源映像的显示名称。<br />-[/ 说明： <Description>]-设置映像的说明。|
|[/Overwrite: {是&#124;否&#124;追加}]|确定指定的文件是否 **/DestinationImage**将覆盖选项，如果在 /Filepath 已存在具有该名称的现有文件。<br /><br />-   **是**会导致覆盖现有文件。<br />-   **不**（默认选项） 后，如果已存在具有相同名称的文件会出现错误。<br />-   **追加**会导致生成的映像作为现有的.wim 文件中的新映像追加。|
## <a name="BKMK_examples"></a>示例
若要导出的启动映像，请键入以下项之一：
```
wdsutil /Export-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86 /DestinationImage /Filepath:"C:\temp\boot.wim"
wdsutil /verbose /Progress /Export-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/DestinationImage /Filepath:"\\Server\Share\ExportImage.wim" /Name:"Exported WinPE image" /Description:"WinPE Image from WDS server" /Overwrite:Yes
```
若要导出安装映像，请键入以下项之一：
```
wdsutil /Export-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Filepath:"C:\Temp\Install.wim"
wdsutil /verbose /Progress /Export-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Filepath:\\server\share\export.wim /Name:"Exported Windows image" /Description:"Windows Vista image from WDS server" /Overwrite:append
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加映像命令](using-the-add-image-command.md)
[使用复制图像命令](using-the-copy-image-command.md)
[使用 get-映像命令](using-the-get-image-command.md)
[使用删除映像命令](using-the-remove-image-command.md)
[使用替换映像命令](using-the-replace-image-command.md)
[子命令： 设置图像](subcommand-set-image.md)
