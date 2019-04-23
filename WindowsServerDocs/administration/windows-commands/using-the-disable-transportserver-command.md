---
title: 使用禁用 TransportServer 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d653aacf225333ac8a8b090ef53fb6826e84ab5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836978"
---
# <a name="using-the-disable-transportserver-command"></a>使用禁用 TransportServer 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

禁用传输服务器的所有服务。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/ 服务器：<Server name>]|指定要禁用的传输服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果不指定任何传输服务器名称，则将使用本地服务器。|
## <a name="BKMK_examples"></a>示例
若要禁用服务器，请键入：
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用启用 TransportServer 命令](using-the-enable-transportserver-command.md)
[使用 get TransportServer 命令](using-the-get-transportserver-command.md)
 [子命令： Set-transportserver](subcommand-set-transportserver.md)
[子命令： 开始 TransportServer](subcommand-start-transportserver.md)
[子命令： 停止 TransportServer](subcommand-stop-transportserver.md)
