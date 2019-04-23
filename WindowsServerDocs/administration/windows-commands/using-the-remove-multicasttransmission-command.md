---
title: 使用 /remove-multicasttransmission 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9a7f5c31-bfbf-425d-9129-a6f9173fe83d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc3ba385644ef9da9b5d592142091ff087cd7545
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839678"
---
# <a name="using-the-remove-multicasttransmission-command"></a>使用 /remove-multicasttransmission 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

禁用映像的多播传输。 除非指定，否则 **/force**，现有客户端将完成图像传输，但新的客户端才能加入。
## <a name="syntax"></a>语法
**Windows Server 2008**
```
wdsutil /remove-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image Group>] [/Filename:<File name>] [/force]
```
**Windows Server 2008 R2**的启动映像：
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
\x20    [/Server:<Server name>]
\x20  mediatype:Boot
\x20    /Architecture:{x86 | ia64 | x64}
\x20    [/Filename:<File name>]
```
对于安装映像：
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group
        [/Filename:<File name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
媒体：<Image name>|指定映像的名称。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则使用本地服务器。|
媒体类型: {安装&#124;启动}|指定图像类型。 请注意，此选项必须设置为**安装**对于 Windows Server 2008。|
|/ 体系结构: {x86 &#124; ia64 &#124; x64}|指定与要启动的传输相关联的启动映像的体系结构。 因为它是可以在不同的体系结构中包含的启动映像的映像名称，则应指定体系结构，以确保使用正确的传输。|
|\mediaGroup:<Image group name>]|指定包含该映像的映像组。 如果未指定任何映像组名称和服务器上只有一个映像组存在，则使用该映像组。 如果在服务器上存在多个映像组，必须使用此选项来指定映像组名称。|
|[/ Filename:<File name>]|指定的文件的名称。 如果名称不能唯一标识源映像，必须使用此选项以指定的文件的名称。|
|[/force]|删除传输，并终止所有客户端。 除非您指定的值 **/force**选项，现有客户端可以完成图像传输，但不能加入新客户端。|
## <a name="BKMK_examples"></a>示例
若要停止一个命名空间 （当前客户端将在完成传输，但新的客户端将不能联接），类型：
```
wdsutil /remove-MulticastTransmissiomedia:"Vista with Office"
/Imagetype:Install
```
```
wdsutil /remove-MulticastTransmissiomedia:"x64 Boot Image"
/Imagetype:Boot /Architecture:x64
```
若要强制终止的所有客户端，请键入：
```
wdsutil /remove-MulticastTransmission /Server:MyWDSServer
/Image:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /force
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用 get AllMulticastTransmissions 命令](using-the-get-allmulticasttransmissions-command.md)
[使用 /get-multicasttransmission 命令](using-the-get-multicasttransmission-command.md)
[使用新 MulticastTransmission 命令](using-the-new-multicasttransmission-command.md)
[子命令： /start-multicasttransmission](subcommand-start-multicasttransmission.md)
