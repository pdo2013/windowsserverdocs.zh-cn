---
title: 使用添加映像命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d5b6f4da-90ba-4b0e-9423-66c8ef5172e2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0433e0775bd2088170ae17fcfe432cdaee0bf99d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817458"
---
# <a name="using-the-add-image-command"></a>使用添加映像命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将图像添加到 Windows 部署服务服务器。 有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
## <a name="syntax"></a>语法
对于启动映像，使用以下语法：
```
wdsutil /add-ImagmediaFile:<wim file path> [/Server:<Server name>mediatype:Boot [/Skipverify] [/Name:<Image name>] [/Description:<Image description>] 
[/Filename:<New wim file name>]
```
对于安装映像，使用以下语法：
```
wdsutil /add-ImagmediaFile:<wim file path>
     [/Server:<Server name>]
   mediatype:Install
     [/Skipverify]
    mediaGroup:<Image group name>]
     [/SingleImage:<Single image name>]
         [/Name:<Name>]
         [/Description:<Description>]
     [/Filename:<File name>]
     [/UnattendFile:<Unattend file path>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
媒体文件： <.wim 文件路径 >|指定包含要添加的映像的 Windows 映像 (.wim) 文件的完整路径和文件名。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果未指定服务器名称，将使用本地服务器。|
mediatype:{Boot&#124;Install}|指定要添加的映像的类型。|
|[/Skipverify]|指定之前添加图像后，不将完整性验证对源图像文件中执行。|
|[/Name:<Name>]|设置图像的显示名称。|
|[/ 说明：<Description>]|设置映像的说明。|
|[/ Filename:<Filename>]|指定的.wim 文件的新文件名称。 这使您要添加映像时更改文件名称的.wim 文件。 如果不指定任何文件名称，将使用源映像文件的名称。 在所有情况下，Windows 部署服务进行检查以确定目标计算机的启动映像存储区中的文件的名称是否唯一。|
|\mediaGroup:<Image group name>]|指定在其中的映像是要添加的映像组的名称。 如果在服务器上存在多个映像组，必须指定映像组。 如果未指定此映像组尚不存在，将创建新的映像组。 否则，将使用现有的映像组。|
|[/ Singleimage:<Single image name>] [/name:<Name>] [/ 说明：<Description>]|复制带.wim 文件中，指定的单一映像，并设置图像的显示名称和说明。|
|[/ UnattendFile:<Unattend file path>]|指定要与所添加的映像关联的无人参与的安装文件的完整路径。 如果 **/SingleImage**未指定，则相同的无人参与文件将与所有.wim 文件中的图像相关联。|
## <a name="BKMK_examples"></a>示例
若要添加启动映像，请键入：
```
wdsutil /add-ImagmediaFile:"C:\MyFolder\Boot.wimmediatype:Boot
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share\Boot.wim /Server:MyWDSServemediatype:Boot /Name:"My WinPE Image" 
/Description:"WinPE Image containing the WDS Client" /Filename:WDSBoot.wim
```
若要添加安装映像，请键入以下项之一：
```
wdsutil /add-ImagmediaFile:"C:\MyFolder\Install.wimmediatype:Install
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share \Install.wim /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/SingleImage:"Windows Pro" /Name:"My WDS Image"
/Description:"Windows Pro image with Microsoft Office" /Filename:"Win Pro.wim" /UnattendFile:"\\server\share\unattend.xml"
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用复制图像命令](using-the-copy-image-command.md)
[使用导出映像命令](using-the-export-image-command.md)
[使用获取映像命令](using-the-get-image-command.md)
[使用删除映像命令](using-the-remove-image-command.md)
[使用替换映像命令](using-the-replace-image-command.md)
 [子命令： 设置图像](subcommand-set-image.md)
