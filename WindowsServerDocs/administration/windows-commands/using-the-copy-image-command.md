---
title: 使用复制映像命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bea41cf4-36e6-4181-afa5-00170ebd4fdc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f269884e9c1f96151c9e1220242fad917b76831
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363564"
---
# <a name="using-the-copy-image-command"></a>使用复制映像命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

复制同一映像组中的映像。 若要在映像组之间复制图像，请使用 "[导出图像](using-the-export-image-command.md)" 命令命令，然后使用 "[添加图像](using-the-add-image-command.md)命令" 命令。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
## <a name="syntax"></a>语法
```
wdsutil [Options] /copy-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Install
    mediaGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Name:<Name>
         /Filename:<File name>
         [/Description:<Description>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
媒体： <Image name>|指定要复制的映像的名称。|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|
媒体媒体：安装|指定要复制的映像类型。 必须将此选项设置为 "**安装**"。|
|\mediaGroup： <Image group name>]|指定包含要复制的映像的映像组。 如果未指定映像组并且服务器上只存在一个组，则默认情况下将使用该映像组。 如果服务器上存在多个映像组，则必须指定映像组。|
|[/Filename： @no__t]|指定要复制的映像的文件名。 如果无法按名称唯一地标识源映像，则必须指定文件名。|
|/DestinationImage|指定目标映像的设置，如下表所述。<br /><br />-/Name： @no__t-设置要复制的图像的显示名称。<br />-/Filename： @no__t-设置将包含图像副本的目标映像文件的名称。<br />-[/Description： <Description>]-设置图像副本的说明。|
## <a name="BKMK_examples"></a>示例
若要创建指定映像的副本并将其命名为 WindowsVista，请键入：
```
wdsutil /copy-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim"
```
若要创建指定映像的副本，应用指定的设置，并将副本命名为 WindowsVista，请键入：
```
wdsutil /verbose /Progress /copy-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim" /Description:"This is a copy of the original Windows image with Office installed"
```
#### <a name="additional-references"></a>其他参考
[命令行语法关键字](command-line-syntax-key.md)
 使用 "[添加图像"](using-the-add-image-command.md)命令 
 使用 "[导出](using-the-export-image-command.md)-映像" 命令 
 使用 "[获取](using-the-get-image-command.md)映像" 命令 @no__t-@no__t[7 使用](using-the-remove-image-command.md)[替换图像命令](using-the-replace-image-command.md)1[子命令：设置-图像](subcommand-set-image.md)
