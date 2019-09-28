---
title: 子命令启动-服务器
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78407f3474c48b928535abb652d2c023dd1c1c14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383816"
---
# <a name="subcommand-start-server"></a>子命令：启动-服务器

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

启动 Windows 部署服务服务器的所有服务。
## <a name="syntax"></a>语法
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/Server： @no__t]|指定要启动的服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|
## <a name="BKMK_examples"></a>示例
若要启动服务器，请键入下列内容之一：
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他参考
[命令行语法键](command-line-syntax-key.md)
[使用 disable-](using-the-disable-server-command.md)server 命令 
 使用 "[启用-服务器"](using-the-enable-server-command.md)命令 
 使用 "服务器[" 命令 @no__t](using-the-get-server-command.md)-7 使用 "[初始化-服务器"](using-the-initialize-server-command.md)命令 
[子命令： set-Server](subcommand-set-server.md)1[子命令： Stop-server](subcommand-stop-server.md)3["取消初始化-服务器" 选项](the-uninitialize-server-option.md)
