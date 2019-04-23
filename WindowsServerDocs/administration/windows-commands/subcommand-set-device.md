---
title: 子命令来设置设备
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 401567f8-eaeb-4a2d-b811-140bb007028d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80bb9144936cf493784603bcbdb8a0d1e5c870bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848058"
---
# <a name="subcommand-set-device"></a>子命令： 设置设备

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

更改为预留计算机的属性。 为预留的计算机是已链接到 active directory 域服务器 (AD DS) 中的计算机帐户对象的计算机。 预留客户端也称为已知的计算机。 你可以以控制客户端安装在计算机帐户上配置属性。 例如，可以配置网络启动程序和客户端应接收，无人参与文件，以及客户端应下载的网络启动程序的服务器。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Set-Device /Device:<Device name> [/ID:<UUID | MAC address>] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] 
[/WdsClientUnattend:<Relative path>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/Domain:<Domain>] [/resetAccount]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/ 设备：<computer name>|指定的计算机 （SAM 帐户名称） 的名称。|
|[/ID:<UUID &#124; MAC address>]|指定 GUID/UUID 或的计算机的 MAC 地址。 此值必须采用以下三种格式之一：<br /><br />-二进制字符串： **/ID:ACEFA3E81F20694E953EB2DAA1E8B1B6**<br />-GUID/UUID 字符串: / ID:**E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br />-MAC 地址：**00B056882FDC** （无短划线） 或**00-B0-56-88-2F-DC** （带短划线）|
|[/ReferralServer:<Server name>]|指定要联系来下载使用普通文件传输协议 (tftp) 的网络启动程序和启动映像的服务器的名称。|
|[/BootProgram:<Relative path>]|指定从 remoteInstall 文件夹的相对路径指定的计算机将收到的网络启动程序。 例如： **boot\x86\pxeboot.com**|
|[/WdsClientUnattend:<Relative path>]|指定可安装屏幕的 Windows 部署服务客户端自动执行无人参与文件的相对路径从 remoteInstall 文件夹的路径。|
|[/ 用户： < 域 \ 用户&#124; User@Domain>]|要为指定的用户提供足够的权限来将计算机加入到域的计算机帐户对象上设置权限。|
|[/ JoinRights: {JoinOnly&#124;完整}]|指定要分配给用户的权限类型。<br /><br />-   **JoinOnly**要求管理员重置计算机帐户，然后用户可以将计算机加入到域。<br />-   **完整**给用户，包括将计算机加入到域的权限授予完全访问权限。|
|[/JoinDomain:{Yes &#124; No}]|指定在计算机应加入到域与此计算机帐户在 Windows 部署服务安装过程。 默认设置是**是**。|
|[/BootImagepath:<Relative path>]|指定该计算机将使用的启动映像从 remoteInstall 文件夹的相对路径。|
|[/ 域：<Domain>]|指定要在其中搜索的预留计算机的域。 默认值为本地域。|
|[/resetAccount]|重置指定的计算机上的权限，以便使用适当的权限的任何人都可以通过使用此帐户加入域。|
## <a name="BKMK_examples"></a>示例
若要为计算机设置网络引导程序，并引用服务器，请键入：
```
wdsutil /Set-Device /Device:computer1 /ReferralServer:MyWDSServer
/BootProgram:boot\x86\pxeboot.n12
```
若要设置的计算机的各种设置，请键入：
```
wdsutil /verbose /Set-Device /Device:computer2 /ID:00-B0-56-88-2F-DC /WdsClientUnattend:WDSClientUnattend\unattend.xml 
/User:Domain\user /JoinRights:JoinOnly /JoinDomain:No /BootImagepath:boot\x86\images\boot.wim /Domain:NorthAmerica /resetAccount
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加设备命令](using-the-add-device-command.md)
[使用 get AllDevices 命令](using-the-get-alldevices-command.md)
[使用获取设备命令](using-the-get-device-command.md)
