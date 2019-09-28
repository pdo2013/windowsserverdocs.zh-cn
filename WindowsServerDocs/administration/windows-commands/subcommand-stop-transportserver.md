---
title: 子命令停止-TransportServer
description: 用于 TransportServer 的 Windows 命令主题
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a2444328a426429c2dce5ceee3272cf1dc814cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370725"
---
# <a name="subcommand-stop-transportserver"></a>子命令： TransportServer

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

停止传输服务器上的所有服务。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Stop-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/Server： @no__t]|指定传输服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定传输服务器，将使用本地服务器。|
## <a name="BKMK_examples"></a>示例
若要停止服务，请键入下列内容之一：
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他参考
使用[TransportServer 命令](using-the-disable-transportserver-command.md)的[命令行语法键](command-line-syntax-key.md)
 @no__t[-3 使用](using-the-enable-transportserver-command.md)TransportServer 命令 
 使用[命令](using-the-get-transportserver-command.md)
[子命令：TransportServer](subcommand-set-transportserver.md)
[子命令： TransportServer](subcommand-start-transportserver.md)
