---
title: 使用 get TransportServer 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de634123-0179-41b2-9c6f-726508130ff5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08aa1273d09ba92de15e13f7bfcc8283ac2fedb6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817418"
---
# <a name="using-the-get-transportserver-command"></a>使用 get TransportServer 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示有关指定的传输服务器的信息。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
|/ 显示: {配置}|返回有关指定的传输服务器的配置信息。|
## <a name="BKMK_examples"></a>示例
若要查看有关服务器的信息，请键入：
```
wdsutil /Get-TransportServer /Show:Config
```
若要查看配置信息，请键入：
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用禁用 TransportServer 命令](using-the-disable-transportserver-command.md)
[使用启用 TransportServer 命令](using-the-enable-transportserver-command.md)
 [子命令： Set-transportserver](subcommand-set-transportserver.md)
[子命令： 开始 TransportServer](subcommand-start-transportserver.md)
[子命令： 停止 TransportServer](subcommand-stop-transportserver.md)
