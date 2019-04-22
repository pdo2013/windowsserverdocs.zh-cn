---
title: 使用 /get-multicasttransmission 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b733737b-1e81-43d4-a058-d6985a613bef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdbb9283285bcf56cd83c18ea076e3d36a51b966
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823228"
---
# <a name="using-the-get-multicasttransmission-command"></a>使用 /get-multicasttransmission 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示有关指定映像的多播传输的信息。
## <a name="syntax"></a>语法
**Windows Server 2008**
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] 
[/Filename:<File name>] [/Show:Clients]
```
**Windows Server 2008 R2**的启动映像传输：
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name>
    [/Server:<Server name>]
    [/details:Clients]
  mediatype:Boot
    /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
对于安装映像传输：
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name>
         [/Server:<Server name>]
         [/details:Clients]
       mediatype:Install
    mediaGroup:<Image Group>]
     [/Filename:<File name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
媒体：<Image name>|显示与此图像关联的多播的传输。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则使用本地服务器。|
mediatype:Install|指定图像类型。 请注意，此选项必须设置为**安装**。|
|\mediaGroup:<Image group name>]|指定包含该映像的映像组。 如果未指定任何映像组名称和服务器上只有一个映像组存在，则使用该映像组。 如果在服务器上存在多个映像组，必须使用此选项来指定映像组。|
|/ 体系结构: {x86 &#124; ia64 &#124; x64}|指定与传输相关联的启动映像的体系结构。 因为它是可以在不同的体系结构中包含的启动映像的映像名称，则应指定体系结构，以确保使用正确的映像。|
|[/ Filename:<File name>]|指定包含图像的文件。 如果名称不能唯一标识该映像，必须使用此选项以指定的文件的名称。|
|[/Show:Clients]<br /><br />或<br /><br />[/details:Clients]|显示有关客户端计算机连接到多播传输的信息。|
## <a name="BKMK_examples"></a>示例
**Windows Server 2008**若要查看名为 Vista Office 的映像，传输有关的信息，请键入以下值之一：
```
wdsutil /Get-MulticastTransmissiomedia:"Vista with Officemediatype:Install
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /Show:Clients
```
**Windows Server 2008 R2**若要查看名为 Vista Office 的映像，传输有关的信息，请键入以下值之一：
```
wdsutil /Get-MulticastTransmissiomedia:"Vista with Office"
 /Imagetype:Install
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /details:Clients
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:"X64 Boot Imagemediatype:Boot /Architecture:x64 /Filename:boot.wim /details:Clients
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用 get AllMulticastTransmissions 命令](using-the-get-allmulticasttransmissions-command.md)
[使用新 MulticastTransmission 命令](using-the-new-multicasttransmission-command.md)
[使用 /remove-multicasttransmission 命令](using-the-remove-multicasttransmission-command.md)
[子命令： /start-multicasttransmission](subcommand-start-multicasttransmission.md)
