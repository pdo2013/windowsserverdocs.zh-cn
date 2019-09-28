---
title: 子命令集-设备
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 92e319e7c6671e9117e33f13ee8fb40a19df93bd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383900"
---
# <a name="subcommand-set-device"></a>子命令：设置-设备

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

更改预留计算机的属性。 预留计算机是已链接到 active directory 域服务器（AD DS）中的计算机帐户对象的计算机。 预留客户端也称为已知计算机。 你可以在计算机帐户上配置属性，以控制客户端的安装。 例如，你可以配置网络启动程序和客户端应接收的无人参与文件，以及客户端应从中下载网络启动程序的服务器。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Set-Device /Device:<Device name> [/ID:<UUID | MAC address>] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] 
[/WdsClientUnattend:<Relative path>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/Domain:<Domain>] [/resetAccount]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/Device： <computer name>|指定计算机的名称（SAM 帐户名）。|
|[/ID： < UUID &#124; MAC 地址 >]|指定计算机的 GUID/UUID 或 MAC 地址。 此值必须是以下三种格式之一：<br /><br />-二进制字符串： **/id： ACEFA3E81F20694E953EB2DAA1E8B1B6**<br />-GUID/UUID 字符串：/ID：**E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br />-MAC 地址：**00B056882FDC** （无短划线）或**00-B0-56-2f** -|
|[/ReferralServer： @no__t]|指定要连接的服务器的名称，以使用普通文件传输协议（tftp）下载网络启动程序和启动映像。|
|[/BootProgram： @no__t]|指定从 remoteInstall 文件夹到指定计算机将接收的网络启动程序的相对路径。 例如： **boot\x86\pxeboot.com**|
|[/WdsClientUnattend： @no__t]|指定从 remoteInstall 文件夹到无人参与文件的相对路径，该文件自动执行 Windows 部署服务客户端的安装屏幕。|
|[/User： < Domain\User &#124; User@Domain >]|设置计算机帐户对象的权限，以向指定的用户授予将计算机加入域所需的权限。|
|[/JoinRights： {JoinOnly &#124; Full}]|指定要分配给用户的权限类型。<br /><br />-   **JoinOnly**要求管理员重置计算机帐户，然后用户才能将计算机加入域。<br />-   **full**为用户提供完全访问权限，包括将计算机加入域的权限。|
|[/JoinDomain： {Yes &#124; No}]|指定是否应在 Windows 部署服务安装期间将计算机作为此计算机帐户加入到域。 默认设置为 **"是"** 。|
|[/BootImagepath： @no__t]|指定从 remoteInstall 文件夹到计算机将使用的启动映像的相对路径。|
|[/Domain： @no__t]|指定要在其中搜索预留计算机的域。 默认值为本地域。|
|[/resetAccount]|重置指定计算机上的权限，以便具有适当权限的任何人都可以使用此帐户加入域。|
## <a name="BKMK_examples"></a>示例
若要为计算机设置网络启动程序和引用服务器，请键入：
```
wdsutil /Set-Device /Device:computer1 /ReferralServer:MyWDSServer
/BootProgram:boot\x86\pxeboot.n12
```
若要设置计算机的各种设置，请键入：
```
wdsutil /verbose /Set-Device /Device:computer2 /ID:00-B0-56-88-2F-DC /WdsClientUnattend:WDSClientUnattend\unattend.xml 
/User:Domain\user /JoinRights:JoinOnly /JoinDomain:No /BootImagepath:boot\x86\images\boot.wim /Domain:NorthAmerica /resetAccount
```
#### <a name="additional-references"></a>其他参考
使用[AllDevices 命令](using-the-get-alldevices-command.md)的[命令行语法键](command-line-syntax-key.md)@no__t[-1 @no__t](using-the-add-device-command.md)-3 使用[get-device](using-the-get-device-command.md)命令 

