---
title: 使用 AutoaddDevices 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5617837706a778bac2456647f40efd563d861722
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363662"
---
# <a name="using-the-approve-autoadddevices-command"></a>使用 AutoaddDevices 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

批准等待管理审批的计算机。 启用自动添加策略后，在未知计算机（未预留的计算机）之前需要管理审批才能安装映像。 你可以使用 "服务器 s 属性" 页的 " **PXE 响应**" 选项卡启用此策略。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Approve-AutoaddDevices [/Server:<Server name>] /RequestId:{<Request ID>| ALL} [/MachineName:<Device name>] [/OU:<DN of OU>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] [/BootImagepath:<Relative path>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|
|/RequestId： {Request ID &#124; ALL}|指定分配给挂起计算机的请求 ID。 指定 "**全部**" 以批准所有挂起的计算机。|
|[/MachineName： @no__t]|指定要添加的计算机的名称。 在批准所有计算机时，不能使用此选项。|
|[/OU： @NO__T]|指定应在其中创建计算机帐户对象的组织单位（OU）的可分辨名称。 例如：**OU = MyOU，CN = Test，dc = Domain，DC = com**。 默认位置是默认计算机的容器。|
|[/User： < Domain\User &#124; User@Domain >]|在计算机帐户对象上设置权限，以便为指定的用户分配所需的权限。|
|[/JoinRights： {JoinOnly &#124; Full}]|指定要分配给指定用户的权限类型。<br /><br />-   **JoinOnly**要求管理员重置计算机帐户，然后用户才能将计算机加入域。<br />-   **full**为用户提供完全访问权限，包括将计算机加入域的权限。|
|[/JoinDomain： {Yes &#124; No}]|指定是否应在操作系统安装过程中将计算机加入域作为此计算机帐户。 默认值为 **"是"** 。|
|[/ReferralServer： @no__t]|指定要连接的服务器的名称，以使用普通文件传输协议（tftp）下载网络启动程序和启动映像。|
|[/BootProgram： @no__t]|指定从 remoteInstall 文件夹到此计算机应接收的网络引导程序的相对路径。 例如： **boot\x86\pxeboot.com**。|
|[/WdsClientUnattend： @no__t]|指定从 remoteInstall 文件夹到自动 Windows 部署服务客户端自动化的无人参与文件的相对路径。|
|[/BootImagepath： @no__t]|指定从 remoteInstall 文件夹到此计算机应接收的启动映像的相对路径。|
## <a name="BKMK_examples"></a>示例
若要审批 RequestId 为12的计算机，请键入：
```
wdsutil /Approve-AutoaddDevices /RequestId:12
```
若要批准 Id 为20的计算机并使用指定的设置部署映像，请键入：
```
wdsutil /Approve-AutoaddDevices /RequestId:20 /MachineName:computer1 /OU:"OU=Test,CN=company,DC=Domain,DC=Com" /User:Domain\User1 
/JoinRights:Full /ReferralServer:MyWDSServer /BootProgram:boot\x86\pxeboot.n12 /WdsClientUnattend:WDSClientUnattend\Unattend.xml /BootImagepath:boot\x86\images\boot.wim
```
若要批准所有挂起的计算机，请键入：
```
wdsutil /verbose /Approve-AutoaddDevices /RequestId:ALL
```
#### <a name="additional-references"></a>其他参考
[命令行语法键](command-line-syntax-key.md)
[使用 AutoaddDevices 命令](using-the-delete-autoadddevices-command.md)
 使用[AutoaddDevices 命令](using-the-get-autoadddevices-command.md)，使用[AutoaddDevices 命令](using-the-reject-autoadddevices-command.md)

