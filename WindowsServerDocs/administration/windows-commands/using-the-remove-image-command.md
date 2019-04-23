---
title: 使用删除映像命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce5e2384-2264-4b22-92af-74eec8c10ae0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2aaf7d63d858045f9dd5df399c5f3f92b038bc2e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846698"
---
# <a name="using-the-remove-image-command"></a>使用删除映像命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

从服务器中删除映像。
## <a name="syntax"></a>语法
对于启动映像：
```
wdsutil [Options] /remove-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<Filename>]
```
对于安装映像：
```
wdsutil [Options] /remove-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<Filename>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
媒体：<Image name>|指定映像的名称。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
媒体类型: {启动&#124;安装}|指定映像的类型。|
|/ 体系结构: {x86 &#124; ia64 &#124; x64}|指定映像的体系结构。 因为它是可以在不同的体系结构中包含不同的启动映像的同一映像名称，指定体系结构值可确保正确的映像将被删除。|
|\mediaGroup:<Image group name>]|指定包含该映像的映像组。 如果未指定任何映像组名称和服务器上只有一个映像组存在，将使用该映像组。 如果存在多个映像组，必须使用此选项来指定映像组。|
|[/ Filename:<File name>]|如果名称不能唯一标识该映像，必须使用此选项以指定的文件的名称。|
## <a name="BKMK_examples"></a>示例
若要删除的启动映像，请键入：
```
wdsutil /remove-Imagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86
```
```
wdsutil /verbose /remove-Imagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim
```
若要删除安装映像，请键入：
```
wdsutil /remove-Imagmedia:"Windows Vista with Officemediatype:Install
```
```
wdsutil /verbose /remove-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加映像命令](using-the-add-image-command.md)
[使用复制图像命令](using-the-copy-image-command.md)
[使用导出映像命令](using-the-export-image-command.md)
[使用 get 映像命令](using-the-get-image-command.md)
[使用替换映像命令](using-the-replace-image-command.md)
 [子命令： 设置图像](subcommand-set-image.md)
