---
title: Uninitialize 服务器选项
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73f1ff67331ae41fa0d88cb3a16df5095e0b6d66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873978"
---
# <a name="the-uninitialize-server-option"></a>Uninitialize 服务器选项

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

还原服务器的初始配置期间对服务器所做的更改。 这包括通过以下任一方法所做的更改 **/initialize-server**选项或 Windows 部署服务 mmc 管理单元中。 请注意，此命令将服务器重置为未配置状态。 此命令不修改 remoteInstall 共享文件夹的内容。 相反，它重置服务器的状态，以便可以重新初始化该服务器。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
## <a name="BKMK_examples"></a>示例
若要重新初始化该服务器，请键入以下项之一：
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用禁用服务器命令](using-the-disable-server-command.md)
[使用启用服务器命令](using-the-enable-server-command.md)
[使用获取服务器命令](using-the-get-server-command.md)
[使用初始化服务器命令](using-the-initialize-server-command.md)
[子命令： 设置服务器](subcommand-set-server.md)
 [子命令： 启动服务器](subcommand-start-server.md)
[子命令： 停止服务器](subcommand-stop-server.md)
