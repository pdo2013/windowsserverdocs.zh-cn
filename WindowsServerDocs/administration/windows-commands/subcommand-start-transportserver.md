---
title: 子命令开始 TransportServer
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e93bc84-5b9e-4f9d-8cf0-1634417da0f6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5fdfea020019a45eceac0142160f9d5d4d97b989
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848628"
---
# <a name="subcommand-start-transportserver"></a>Subcommand: start-TransportServer

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

启动传输服务器的所有服务。
## <a name="syntax"></a>语法
```
wdsutil [Options] /start-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/ 服务器：<Server name>]|指定在传输服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
## <a name="BKMK_examples"></a>示例
若要启动服务器，请键入以下项之一：
```
wdsutil /start-TransportServer
wdsutil /verbose /start-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用禁用 TransportServer 命令](using-the-disable-transportserver-command.md)
[使用启用 TransportServer 命令](using-the-enable-transportserver-command.md)
 [使用 get TransportServer 命令](using-the-get-transportserver-command.md)
[子命令： Set-transportserver](subcommand-set-transportserver.md)
[子命令： 停止 TransportServer](subcommand-stop-transportserver.md)
