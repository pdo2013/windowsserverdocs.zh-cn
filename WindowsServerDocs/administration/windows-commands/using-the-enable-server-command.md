---
title: 使用 enable-Server 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b90621ec14c6cf451d7a05eace79f2e0679b2f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363450"
---
# <a name="using-the-enable-server-command"></a>使用 enable-Server 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

启用 Windows 部署服务的所有服务。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|
## <a name="BKMK_examples"></a>示例
若要在服务器上启用这些服务，请运行下列操作之一：
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他参考
[命令行语法键](command-line-syntax-key.md)
 使用[disable-server](using-the-disable-server-command.md)命令 @no__t[-3 使用服务器命令](using-the-get-server-command.md)
，使用[Initialize-](using-the-initialize-server-command.md)server 命令 
[子命令： set-server](subcommand-set-server.md)
[子命令： start-Server](subcommand-start-server.md)1[子命令： Stop-server](subcommand-stop-server.md)3["取消初始化-服务器" 选项](the-uninitialize-server-option.md)
