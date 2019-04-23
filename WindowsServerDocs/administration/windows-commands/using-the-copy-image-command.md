---
title: 使用复制映像命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 52c9c0bb45e60e76077bf90534e93f2c6fc1df18
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884038"
---
# <a name="using-the-copy-image-command"></a>使用复制映像命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

相同的映像组内的副本映像。 若要将映像映像组之间的复制，请使用[使用导出映像命令](using-the-export-image-command.md)命令，然后[使用添加映像命令](using-the-add-image-command.md)命令。
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
媒体：<Image name>|指定要复制的图像的名称。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
mediatype:Install|指定要复制的图像的类型。 此选项必须设置为**安装**。|
|\mediaGroup:<Image group name>]|指定包含要复制的映像的映像组。 如果指定没有映像组，并且在服务器上存在一个组，将默认使用该映像组。 如果在服务器上存在多个映像组，则必须指定映像组。|
|[/ Filename:<Filename>]|指定要复制的图像的文件名。 如果名称不能唯一标识源映像，必须指定的文件的名称。|
|/DestinationImage|下表中所述，请指定目标图像的设置。<br /><br />-/Name:<Name> -设置要复制的图像的显示名称。<br />-必需：<Filename> -设置将包含映像副本的目标图像文件的名称。<br />-[/ 说明： <Description>]-设置映像副本的说明。|
## <a name="BKMK_examples"></a>示例
若要创建指定的图像的副本并将其命名 WindowsVista.wim，键入：
```
wdsutil /copy-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim"
```
若要创建指定的图像的副本，将应用指定的设置，并命名副本 WindowsVista.wim，类型：
```
wdsutil /verbose /Progress /copy-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim" /Description:"This is a copy of the original Windows image with Office installed"
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加映像命令](using-the-add-image-command.md)
[使用导出映像命令](using-the-export-image-command.md)
[使用获取映像命令](using-the-get-image-command.md)
[使用删除映像命令](using-the-remove-image-command.md)
[使用替换映像命令](using-the-replace-image-command.md)
 [子命令： 设置图像](subcommand-set-image.md)
