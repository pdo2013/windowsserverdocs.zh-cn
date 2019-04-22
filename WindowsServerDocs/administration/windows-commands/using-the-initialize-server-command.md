---
title: 使用初始化服务器命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68a26ad9-5eb2-4490-b782-b7cd46b8000d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a1f3a1c02eb21f630aaff7219610864cd1db2a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825188"
---
# <a name="using-the-initialize-server-command"></a>使用初始化服务器命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

安装服务器角色之后配置初次使用的 Windows 部署服务服务器。 在运行此命令后，应使用[使用添加映像命令](using-the-add-image-command.md)命令添加到服务器的映像。
## <a name="syntax"></a>语法
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
|/remInst:"<Full path>"|指定完整路径和 remoteInstall 文件夹的名称。 如果指定的文件夹尚不存在，此选项将创建它时运行该命令。 应始终输入本地路径，即使是在远程计算机。 例如：**D:\remoteInstall**。|
|[/ 授权]|授权服务器中动态主机控制协议 (DHCP)。 此选项是必需仅当启用了 DHCP 恶意检测，也就是说，Windows 部署服务 PXE 服务器必须授权 DHCP 中之前客户端计算机可以提供服务。 请注意，默认情况下禁用 DHCP 恶意检测。|
## <a name="BKMK_examples"></a>示例
若要初始化该服务器并将 remoteInstall 共享的文件夹设置为 f： 驱动器，键入。
```
wdsutil /Initialize-Server /remInst:"F:\remoteInstall"
```
若要初始化该服务器并将 remoteInstall 共享的文件夹设置为 c： 驱动器，键入。
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:"C:\remoteInstall"
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用禁用服务器命令](using-the-disable-server-command.md)
[使用启用服务器命令](using-the-enable-server-command.md)
[使用获取服务器命令](using-the-get-server-command.md)
[子命令： 设置服务器](subcommand-set-server.md)
[子命令： 启动 Server](subcommand-start-server.md)
[子命令：停止服务器](subcommand-stop-server.md)
[uninitialize 服务器选项](the-uninitialize-server-option.md)
