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
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 732a021b02193a3bfb5cb573a33879dbecb840b9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363431"
---
# <a name="using-the-enable-transportserver-command"></a>使用 TransportServer 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

启用传输服务器的所有服务。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/Server： @no__t]|指定传输服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定名称，则将使用本地服务器。|
## <a name="BKMK_examples"></a>示例
若要在服务器上启用这些服务，请运行下列操作之一：
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他参考
[命令行语法键](command-line-syntax-key.md)
[使用 TransportServer 命令](using-the-disable-transportserver-command.md)
 使用[TransportServer 命令](using-the-get-transportserver-command.md)
[子命令： TransportServer](subcommand-set-transportserver.md)
[子命令：TransportServer](subcommand-start-transportserver.md)
[子命令： TransportServer](subcommand-stop-transportserver.md)
