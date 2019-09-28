---
title: 使用 TransportServer 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 282b69162cf3550c5bcba3282b60f15072c96ed6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363072"
---
# <a name="using-the-get-transportserver-command"></a>使用 TransportServer 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示有关指定传输服务器的信息。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|
|/Show： {Config}|返回有关指定传输服务器的配置信息。|
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
[命令行语法键](command-line-syntax-key.md)
[使用 TransportServer 命令](using-the-disable-transportserver-command.md)
 使用[TransportServer 命令](using-the-enable-transportserver-command.md)
[子命令： TransportServer](subcommand-set-transportserver.md)
[子命令：TransportServer](subcommand-start-transportserver.md)
[子命令： TransportServer](subcommand-stop-transportserver.md)
