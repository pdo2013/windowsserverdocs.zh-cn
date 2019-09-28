---
title: 使用 get Image 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ecaa999-72ad-4191-adb5-a418de42a001
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73b25fd70c1512f99b5097b2317c3f0403f3526c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363115"
---
# <a name="using-the-get-image-command"></a>使用 get Image 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

检索有关映像的信息。
## <a name="syntax"></a>语法
对于启动映像：
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
```
对于安装映像：
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
媒体： <Image name>|指定映像的名称。|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|
媒体： {Boot &#124; Install}|指定图像的类型。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定图像的体系结构。 由于不同体系结构中的启动映像可能具有相同的映像名称，因此指定体系结构值可确保返回正确的映像。|
|[/Filename： @no__t]|如果无法按名称唯一地标识映像，则必须使用此选项指定文件名。|
|\mediaGroup： <Image group name>]|指定包含图像的映像组。 如果未指定映像组并且服务器上只存在一个映像组，则将使用该组。 如果服务器上存在多个映像组，则必须使用此参数来指定映像组。|
## <a name="BKMK_examples"></a>示例
若要检索有关启动映像的信息，请键入下列内容之一：
```
wdsutil /Get-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86
wdsutil /verbose /Get-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim
```
若要检索有关安装映像的信息，请键入下列内容之一：
```
wdsutil /Get-Imagmedia:"Windows Vista with Officemediatype:Install
wdsutil /verbose /Get-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
#### <a name="additional-references"></a>其他参考
[命令行语法关键字](command-line-syntax-key.md)
 使用 "[添加图像](using-the-add-image-command.md)" 命令 
 使用 "[复制映像](using-the-copy-image-command.md)" 命令 
 使用 "[导出](using-the-export-image-command.md)-映像" 命令 @no__t-@no__t[7 使用](using-the-remove-image-command.md)[替换图像命令](using-the-replace-image-command.md)1[子命令：设置-图像](subcommand-set-image.md)
