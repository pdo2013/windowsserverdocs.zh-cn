---
title: 子命令停止-服务器
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7584dcbca5bfc52d303f187f62be24cbad407416
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383740"
---
# <a name="subcommand-stop-server"></a>子命令：停止-服务器

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

停止 Windows 部署服务服务器上的所有服务。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Stop-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|
## <a name="BKMK_examples"></a>示例
若要停止服务，请键入下列内容之一：
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他参考
[命令行语法键](command-line-syntax-key.md)
[使用 disable-](using-the-disable-server-command.md)server 命令 
 使用 "[启用-服务器"](using-the-enable-server-command.md)命令 
 使用 "服务器[" 命令 @no__t](using-the-get-server-command.md)-7 使用 "[初始化-服务器"](using-the-initialize-server-command.md)命令 
[子命令： set-Server](subcommand-set-server.md)1[子命令： Start-Server](subcommand-start-server.md)3：取消[初始化-服务器选项](the-uninitialize-server-option.md)
