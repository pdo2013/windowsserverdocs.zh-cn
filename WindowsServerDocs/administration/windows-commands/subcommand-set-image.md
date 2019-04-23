---
title: 子命令设置图像
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ae03c86-7a13-4e38-9182-32e55fffd504
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e63c67210764de76edae18a1897a68d763f9d695
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856478"
---
# <a name="subcommand-set-image"></a>子命令： 设置图像

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

更改图像的特性。
## <a name="syntax"></a>语法
对于启动映像：
```
wdsutil /Set-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>] [/Name:<Name>] 
[/Description:<Description>] [/Enabled:{Yes | No}]
```
对于安装映像：
```
wdsutil /Set-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:InstallmediaGroup:<Image group name>]
     [/Filename:<File name>]
     [/Name:<Name>]
     [/Description:<Description>]
     [/UserFilter:<SDDL>]
     [/Enabled:{Yes | No}]
     [/UnattendFile:<Unattend file path>]
         [/OverwriteUnattend:{Yes | No}]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
媒体：<Image name>|指定映像的名称。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
媒体类型: {启动&#124;安装}|指定映像的类型。|
|/ 体系结构: {x86 &#124; ia64 &#124; x64}|指定映像的体系结构。 由于你可以在不同的体系结构中有不同的启动映像的同一映像名称，指定体系结构可确保修改正确的映像。|
|[/ Filename:<File name>]|如果名称不能唯一标识该映像，必须使用此选项以指定的文件的名称。|
|[/Name]|指定映像的名称。|
|[/ 说明：<Description>]|设置映像的说明。|
|[/ 已启用: {是&#124;No}]|启用或禁用该映像。|
|\mediaGroup:<Image group name>]|指定包含该映像的映像组。 如果未指定任何映像组名称和服务器上只有一个映像组存在，将使用该映像组。 如果在服务器上存在多个映像组，必须使用此选项来指定映像组。|
|[/ UserFilter:<SDDL>]|在映像上设置用户的筛选器。 筛选器字符串必须采用安全描述符定义语言 (SDDL) 格式。 请注意，与不同 **/安全**选项对于映像组，此选项仅用于限制谁可以查看映像定义并不实际的图像文件资源。 若要限制对文件资源的访问并因此访问映像组中的所有映像，你将需要设置安全映像组本身。|
|[/ UnattendFile:<Unattend file path>]|设置要与映像关联的无人参与文件的完整路径。 例如：**D:\Files\Unattend\Img1Unattend.xml**|
|[/OverwriteUnattend:{Yes &#124; No}]|您可以指定 **/overwrite**覆盖无人参与文件，如果已存在与图像关联无人参与文件。 请注意，默认设置是**否**。|
## <a name="BKMK_examples"></a>示例
若要设置的启动映像的值，请键入以下项之一：
```
wdsutil /Set-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86 /Description:"New description"
wdsutil /verbose /Set-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim 
/Name:"New Name" /Description:"New Description" /Enabled:Yes
```
若要为安装映像设置值，请键入以下项之一：
```
wdsutil /Set-Imagmedia:"Windows Vista with Officemediatype:Install /Description:"New description" 
wdsutil /verbose /Set-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /Name:"New name" /Description:"New description" /UserFilter:"O:BAG:DUD:AI(A;ID;FA;;;SY)(A;ID;FA;;;BA)(A;ID;0x1200a9;;;AU)" /Enabled:Yes /UnattendFile:\\server\share\unattend.xml /OverwriteUnattend:Yes
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加映像命令](using-the-add-image-command.md)
[使用复制图像命令](using-the-copy-image-command.md)
[使用导出映像命令](using-the-export-image-command.md)
[使用 get 映像命令](using-the-get-image-command.md)
[使用删除映像命令](using-the-remove-image-command.md)
 [使用替换映像命令](using-the-replace-image-command.md)
