---
title: 使用批准 AutoaddDevices 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8d76e8d3-ab35-429c-be7b-904f95d0782d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ce9824c45a00ccb9f1f9e357c7e3d36b2857f69
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886818"
---
# <a name="using-the-approve-autoadddevices-command"></a>使用批准 AutoaddDevices 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

批准挂起的管理批准的计算机。 启用自动添加策略后，未知的计算机 （即未预留） 可以安装映像之前，将需要管理批准。 您可以使用此策略启用**PXE 响应**服务器的属性页的选项卡。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Approve-AutoaddDevices [/Server:<Server name>] /RequestId:{<Request ID>| ALL} [/MachineName:<Device name>] [/OU:<DN of OU>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] [/BootImagepath:<Relative path>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果未指定服务器名称，将使用本地服务器。|
|/RequestId:{Request ID &#124; ALL}|指定分配给挂起计算机的请求 ID。 指定**所有**批准所有挂起的计算机。|
|[/ MachineName:<Device name>]|指定要添加的计算机的名称。 批准所有计算机时，不能使用此选项。|
|[/ OU:<DN of OU>]|指定应在其中创建计算机帐户对象的组织单位 (OU) 的可分辨的名称。 例如：**OU = MyOU，CN = 测试，DC = Domain，DC = com**。 默认位置是计算机的默认的容器。|
|[/ 用户： < 域 \ 用户&#124; User@Domain>]|若要将指定的用户分配所需的权限的计算机帐户对象上设置权限。|
|[/ JoinRights: {JoinOnly&#124;完整}]|指定要分配给指定的用户的权限类型。<br /><br />-   **JoinOnly**要求管理员重置计算机帐户，然后用户可以将计算机加入到域。<br />-   **完整**给用户，其中包括将计算机加入到域的权限授予完全访问权限。|
|[/JoinDomain:{Yes &#124; No}]|指定在计算机应该加入到域与此计算机帐户在操作系统安装过程。 默认值是**是**。|
|[/ReferralServer:<Server name>]|指定要联系来使用普通文件传输协议 (tftp) 下载的网络启动程序和启动映像的服务器的名称。|
|[/BootProgram:<Relative path>]|指定应接收此计算机的网络启动程序从 remoteInstall 文件夹的相对路径。 例如： **boot\x86\pxeboot.com**。|
|[/WdsClientUnattend:<Relative path>]|指定可自动执行 Windows 部署服务客户端无人参与文件的相对路径从 remoteInstall 文件夹的路径。|
|[/BootImagepath:<Relative path>]|指定应接收此计算机的启动映像从 remoteInstall 文件夹的相对路径。|
## <a name="BKMK_examples"></a>示例
若要批准的 12 RequestId 的计算机，键入：
```
wdsutil /Approve-AutoaddDevices /RequestId:12
```
若要批准的请求 Id 为 20 个计算机和部署的映像使用指定的设置，键入：
```
wdsutil /Approve-AutoaddDevices /RequestId:20 /MachineName:computer1 /OU:"OU=Test,CN=company,DC=Domain,DC=Com" /User:Domain\User1 
/JoinRights:Full /ReferralServer:MyWDSServer /BootProgram:boot\x86\pxeboot.n12 /WdsClientUnattend:WDSClientUnattend\Unattend.xml /BootImagepath:boot\x86\images\boot.wim
```
若要批准所有挂起的计算机，键入：
```
wdsutil /verbose /Approve-AutoaddDevices /RequestId:ALL
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用 delete AutoaddDevices 命令](using-the-delete-autoadddevices-command.md)
[使用 get AutoaddDevices 命令](using-the-get-autoadddevices-command.md)
 [使用拒绝 AutoaddDevices 命令](using-the-reject-autoadddevices-command.md)
