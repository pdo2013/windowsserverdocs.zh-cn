---
title: 使用替换映像命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68ded3df-e309-420f-9f5d-caeb609385a5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e396ee678e22885a50c02800d77ecea1cc5ef8ba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819648"
---
# <a name="using-the-replace-image-command"></a>使用替换映像命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

使用该映像的新版本替换现有的映像。
## <a name="syntax"></a>语法
对于启动映像：
```
wdsutil [Options] /replace-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Boot
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /replacementImage
       mediaFile:<wim file path>
         [/Name:<Image name>]
         [/Description:<Image description>]
```
对于安装映像：
```
wdsutil [Options] /replace-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Install
    mediaGroup:<Image group name>]
     [/Filename:<File name>]
     /replacementImage
       mediaFile:<wim file path>
         [/SourceImage:<Source image name>]
         [/Name:<Image name>]
         [/Description:<Image description>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
媒体：<Image name>|指定要替换的图像的名称。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
媒体类型: {启动&#124;安装}|指定图像要替换的类型。|
|/ 体系结构: {x86 &#124; ia64 &#124; x64}|指定要替换的图像的体系结构。 因为它是可以在不同的体系结构中包含不同的启动映像的同一映像名称，指定体系结构可确保替换正确的映像。|
|[/ Filename:<File name>]|如果名称不能唯一标识该映像，必须使用此选项以指定的文件的名称。|
|/replacementImage|指定替换图像的设置。 设置这些设置，请使用以下选项：<br /><br />-媒体文件： <file path> -指定的名称和新的.wim 文件的位置 （完整路径）。<br />-[/ SourceImage: <image name>]-指定.wim 文件包含多个映像时要使用的图像。 此选项仅适用于安装映像。<br />-[/Name:<Image name>] 设置图像的显示名称。<br />-[/ 说明：<Image description>]-设置映像的说明。|
## <a name="BKMK_examples"></a>示例
若要替换的启动映像，请键入以下项之一：
```
wdsutil /replace-Imagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /replacementImagmediaFile:"C:\MyFolder\Boot.wim"
wdsutil /verbose /Progress /replace-Imagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/replacementImagmediaFile:\\MyServer\Share\Boot.wim /Name:"My WinPE Image" /Description:"WinPE Image with drivers"
```
若要替换的安装映像，请键入以下项之一：
```
wdsutil /replace-Imagmedia:"Windows Vista Homemediatype:Install /replacementImagmediaFile:"C:\MyFolder\Install.wim"
wdsutil /verbose /Progress /replace-Imagmedia:"Windows Vista Pro" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:Install.wim /replacementImagmediaFile:\\MyServer\Share \Install.wim /SourceImage:"Windows Vista Ultimate" /Name:"Windows Vista Desktop" /Description:"Windows Vista Ultimate with standard business applications."
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加映像命令](using-the-add-image-command.md)
[使用复制图像命令](using-the-copy-image-command.md)
[使用导出映像命令](using-the-export-image-command.md)
[使用 get 映像命令](using-the-get-image-command.md)
[使用替换映像命令](using-the-replace-image-command.md)
 [子命令： 设置图像](subcommand-set-image.md)
