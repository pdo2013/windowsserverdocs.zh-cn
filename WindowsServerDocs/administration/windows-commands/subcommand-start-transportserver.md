---
title: 子命令开始-TransportServer
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c1bdf80aa9c255e12e1e4821467d556eb67f8691
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370734"
---
# <a name="subcommand-start-transportserver"></a>子命令： TransportServer

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

启动传输服务器的所有服务。
## <a name="syntax"></a>语法
```
wdsutil [Options] /start-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/Server： @no__t]|指定传输服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|
## <a name="BKMK_examples"></a>示例
若要启动服务器，请键入下列内容之一：
```
wdsutil /start-TransportServer
wdsutil /verbose /start-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他参考
使用[TransportServer 命令](using-the-disable-transportserver-command.md)的[命令行语法键](command-line-syntax-key.md)
 @no__t[-3 使用](using-the-enable-transportserver-command.md)TransportServer 命令 
 使用[命令](using-the-get-transportserver-command.md)
[子命令：TransportServer](subcommand-set-transportserver.md)
[子命令： TransportServer](subcommand-stop-transportserver.md)
