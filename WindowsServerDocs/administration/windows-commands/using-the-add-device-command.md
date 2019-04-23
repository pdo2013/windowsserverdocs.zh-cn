---
title: 使用添加设备命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e599cc4-464a-421b-b6bb-c101af154131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85e9ef4445b4dabbe85c2397d62b06756e17879d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878538"
---
# <a name="using-the-add-device-command"></a>使用添加设备命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

其预留 active directory 域服务中的计算机。 预留的计算机也称为已知的计算机。 这允许您配置属性以控制客户端安装。 例如，可以配置网络启动程序和客户端应接收，无人参与文件，以及客户端应下载的网络启动程序的服务器。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
## <a name="syntax"></a>语法
```
wdsutil /add-Device /Device:<Device name> /ID:<UUID | MAC address> [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/OU:<DN of OU>] [/Domain:<Domain>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/ 设备：<computer name>|指定要添加的计算机的名称。|
|/ID:<UUID &#124; MAC address>|指定 GUID/UUID 或的计算机的 MAC 地址。 在这两种格式的二进制字符串或 GUID 字符串之一必须是 GUID/UUID。 例如：<br /><br />二进制字符串： **/ID:ACEFA3E81F20694E953EB2DAA1E8B1B6**<br /><br />GUID 字符串： **/ID:E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br /><br />MAC 地址必须采用以下格式：**00B056882FDC** （无短划线） 或**00-B0-56-88-2F-DC** （带短划线）|
|[/ReferralServer:<Server name>]|指定要联系来使用普通文件传输协议 (tftp) 下载的网络启动程序和启动映像的服务器的名称。|
|[/BootProgram:<Relative path>]|指定应接收此计算机的网络启动程序从 remoteInstall 文件夹的相对路径。 例如:"boot\x86\pxeboot.com"|
|[/WdsClientUnattend:<Relative path>]|指定可自动执行 Windows 部署服务客户端的安装屏幕的无人参与的安装文件从 remoteInstall 文件夹的相对路径。|
|[/ 用户： < 域 \ 用户&#124; User@Domain>]|要为指定的用户提供足够的权限来将计算机加入到域的计算机帐户对象上设置权限。|
|[/ JoinRights: {JoinOnly&#124;完整}]|指定要分配给用户的权限类型。<br /><br />-   **JoinOnly**要求管理员重置计算机帐户，然后用户可以将计算机加入到域。<br />-   **完整**给用户，其中包括将计算机加入到域的权限授予完全访问权限。|
|[/JoinDomain:{Yes &#124; No}]|指定在计算机应该加入到域与此计算机帐户在操作系统安装过程。 默认值是**是**。|
|[/BootImagepath:<Relative path>]|指定应使用此计算机的启动映像从 remoteInstall 文件夹的相对路径。|
|[/ OU:<DN of OU>]|应在其中创建计算机帐户对象的组织单位的可分辨的名称。 例如：**OU = MyOU，CN = 测试，DC = Domain，DC = com**。 默认位置是默认计算机容器。|
|[/ 域：<Domain>]|应在其中创建计算机帐户对象的域。 默认位置为本地域。|
## <a name="BKMK_examples"></a>示例
若要使用的 MAC 地址添加一台计算机，请键入：
```
wdsutil /add-Device /Device:computer1 /ID:00-B0-56-88-2F-DC
```
若要使用的 GUID 字符串添加一台计算机，请键入：
```
wdsutil /add-Device /Device:computer1 /ID:{E8A3EFAC-201F-4E69-953F-B2DAA1E8B1B6} /ReferralServer:WDSServer1 /BootProgram:boot\x86\pxeboot.com 
/WDSClientUnattend:WDSClientUnattend\unattend.xml /User:Domain\MyUser/JoinRights:Full /BootImagepath:boot\x86\images\boot.wim /OU:"OU=MyOU,CN=Test,DC=Domain,DC=com"
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用 get AllDevices 命令](using-the-get-alldevices-command.md)
[使用 get 设备命令](using-the-get-device-command.md)
[子命令：设置设备](subcommand-set-device.md)
[新建 WdsClient](https://technet.microsoft.com/library/dn283430.aspx)
