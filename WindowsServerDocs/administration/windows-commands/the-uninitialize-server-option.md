---
title: "\"取消初始化服务器\" 选项"
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5c63e09738871c5b74c1b564a83c35ad28f4fa80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385590"
---
# <a name="the-uninitialize-server-option"></a>"取消初始化服务器" 选项

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

恢复在初始服务器配置期间对服务器所做的更改。 这包括 **/initialize-server**选项或 Windows 部署服务 mmc 管理单元所做的更改。 请注意，此命令会将服务器重置为未配置状态。 此命令不会修改 remoteInstall 共享文件夹的内容。 相反，它会重置服务器的状态，以便您可以重新初始化服务器。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|
## <a name="BKMK_examples"></a>示例
若要重新初始化服务器，请键入下列内容之一：
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他参考
[命令行语法键](command-line-syntax-key.md)
[使用 disable-](using-the-disable-server-command.md)server 命令 
 使用 "[启用-服务器"](using-the-enable-server-command.md)命令 
 使用 "服务器[" 命令 @no__t](using-the-get-server-command.md)-7 使用 "[初始化-服务器"](using-the-initialize-server-command.md)命令 
[子命令： set-Server](subcommand-set-server.md)1[子命令： Start-server](subcommand-start-server.md)3[子命令： stop-server](subcommand-stop-server.md)
