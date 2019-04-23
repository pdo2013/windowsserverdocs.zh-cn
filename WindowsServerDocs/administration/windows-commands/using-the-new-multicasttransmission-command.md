---
title: 使用新 MulticastTransmission 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c1f1dc46-dd50-4eb9-9f72-cf0e5d71bd3d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84f5aa69f2d4a875995ac6c18fa43bd68518fba3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875138"
---
# <a name="using-the-new-multicasttransmission-command"></a>使用新 MulticastTransmission 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建映像的新多播的传输。 此命令相当于使用 Windows 部署服务 mmc 管理单元创建传输 (右键单击**多播传输**节点，并单击**创建多播传输**). 部署服务器角色服务和传输服务器角色服务安装 （这是默认安装） 时，应使用此命令。 如果只安装传输服务器角色服务，使用[使用新 Namespace 命令](using-the-new-namespace-command.md)。
## <a name="syntax"></a>语法
对于安装映像传输：
```
wdsutil [Options] /New-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
        /FriendlyName:<Friendly name>
        [/Description:<Description>]
        /Transmissiontype: {AutoCast | ScheduledCast}
            [/time:<YYYY/MM/DD:hh:mm>]
            [/Clients:<Num of Clients>]
      mediatype:Install
       mediaGroup:<Image Group>]
        [/Filename:<File name>]
```
有关启动映像传输 （仅支持 Windows Server 2008 R2）：
```
wdsutil [Options] /New-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
        /FriendlyName:<Friendly name>
        [/Description:<Description>]
        /Transmissiontype: {AutoCast | ScheduledCast}
            [/time:<YYYY/MM/DD:hh:mm>]
            [/Clients:<Num of Clients>]
      mediatype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
媒体：<Image name>|指定要使用多播传输的图像的名称。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
|/Friendlyname:<Friendly name>|指定传输的友好名称。|
|[/ 说明：<Description>]|指定传输的说明。|
mediatype:{Boot&#124;Install}|指定要使用多播传输图像的类型。 请注意**启动**只能用于 Windows Server 2008 R2。|
|\mediaGroup:<Image group name>]|指定包含该映像的映像组。 如果未指定任何映像组名称和服务器上只有一个映像组存在，则使用该映像组。 如果在服务器上存在多个映像组，必须使用此选项来指定映像组名称。|
|[/ Filename:<File name>]|指定的文件的名称。 如果名称不能唯一标识源映像，必须使用此选项以指定的文件的名称。|
|/Transmissiontype:{AutoCast &#124; ScheduledCast}|指定是否自动启动传输 （自动广播） 或基于指定的开始条件 (ScheduledCast)。<br /><br /><ul><li>**自动强制转换**。 此传输类型表示只要适合的客户端请求安装映像时，所选映像的多播的传输开始。 如其他客户端请求同一映像时，会加入已启动的传输。</li><li>**计划强制**。 此传输类型设置基于客户端映像和/或特定的日期和时间所请求的数量，传输的启动条件。 您可以指定以下选项：<br /><br /><ul><li>[/time: <time>]-设置传输应首先使用以下格式的时间：YYYY/MM/DD:hh:mm。</li><li>[/ 客户端： <Number of clients>]-设置客户端传输开始前等待的最小数量。</li></ul></li></ul>|
|/ 体系结构: {x86 &#124; ia64 &#124; x64}|指定用于传输使用多播的启动映像的体系结构。 因为它是可能有不同的体系结构的启动映像相同的名称，则应指定体系结构，以确保使用正确的映像。|
|[/ Filename:<File name>]|指定的文件的名称。 如果名称不能唯一标识源映像，必须指定的文件的名称。|
## <a name="BKMK_examples"></a>示例
若要在 Windows Server 2008 R2 中创建的启动映像的 Auto-cast 传输，请键入：
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS Boot Transmission"
/Image:"X64 Boot Imagemediatype:Boot /Architecture:x64 /Transmissiontype:AutoCast
```
若要创建安装映像 Auto-cast 传输，请键入：
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS AutoCast Transmission"
/Image:"Vista with Officemediatype:Install /Transmissiontype:AutoCast
```
若要创建 Scheduled-cast 传输的安装映像，请键入：
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS SchedCast Transmission" /Server:MyWDSServemedia:"Vista with Officemediatype:Install 
/Transmissiontype:ScheduledCast /time:"2006/11/20:17:00" /Clients:100
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用 get AllMulticastTransmissions 命令](using-the-get-allmulticasttransmissions-command.md)
[使用 /get-multicasttransmission 命令](using-the-get-multicasttransmission-command.md)
[使用 /remove-multicasttransmission 命令](using-the-remove-multicasttransmission-command.md)
[子命令： /start-multicasttransmission](subcommand-start-multicasttransmission.md)
