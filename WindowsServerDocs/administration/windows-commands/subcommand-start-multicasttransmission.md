---
title: 子命令开始-MulticastTransmission
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1b2d459-1ece-49d4-997c-9d206c463b61
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c0e05a1d625e560d85f0af6ae1d76ef8116ddfd8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383831"
---
# <a name="subcommand-start-multicasttransmission"></a>子命令： MulticastTransmission

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

启动映像的计划强制转换。
## <a name="syntax"></a>语法
**Windows Server 2008**
```
wdsutil /start-MulticastTransmissiomedia:<Image name> [/Server:<Server namemediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
用于启动映像的**Windows Server 2008 R2** ：
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
对于安装映像：
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group>]
        [/Filename:<File name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
媒体： <Image name>|指定映像的名称。|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|
媒体： {安装&#124;启动}|指定映像类型。 请注意，必须将此选项设置为 "为 Windows Server 2008**安装**"。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|与要启动的传输关联的启动映像的体系结构。 由于不同体系结构中的启动映像可能具有相同的映像名称，因此应指定体系结构以确保使用正确的传输。|
|\mediaGroup： <Image group name>]|指定图像的图像组。 如果未指定映像组名称，并且服务器上只存在一个映像组，则将使用该映像组。 如果服务器上存在多个映像组，则必须使用此选项来指定映像组名称。|
|[/Filename： @no__t]|指定包含图像的文件的名称。 如果无法按名称唯一地标识映像，则必须使用此选项指定文件名。|
## <a name="BKMK_examples"></a>示例
若要开始多播传输，请键入下列内容之一：
```
wdsutil /start-MulticastTransmissiomedia:"Vista with Office"
/Imagetype:Install
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
若要为 Windows Server 2008 R2 启动启动映像多播传输，请键入：
```
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:"X64 Boot Imagemediatype:Boot /Architecture:x64
/Filename:boot.wim\n\
```
#### <a name="additional-references"></a>其他参考
[命令行语法键](command-line-syntax-key.md)
 使用[AllMulticastTransmissions 命令](using-the-get-allmulticasttransmissions-command.md)
 使用[MulticastTransmission 命令](using-the-get-multicasttransmission-command.md)，使用 MulticastTransmission 命令 
，使用[@no__t 命令](using-the-new-multicasttransmission-command.md)[MulticastTransmission 命令](using-the-remove-multicasttransmission-command.md)
