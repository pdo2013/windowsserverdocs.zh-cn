---
title: 使用启用服务器命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: fdf03a778a6c646aa79c2f844212b1728c5c73eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852718"
---
# <a name="using-the-enable-server-command"></a>使用启用服务器命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

启用 Windows 部署服务的所有服务。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
## <a name="BKMK_examples"></a>示例
若要启用的服务器上的服务，请运行以下项之一：
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用禁用服务器命令](using-the-disable-server-command.md)
[使用获取服务器命令](using-the-get-server-command.md)
[使用初始化服务器命令](using-the-initialize-server-command.md)
[子命令： 设置服务器](subcommand-set-server.md)
[子命令： 启动 Server](subcommand-start-server.md) 
 [子命令： 停止服务器](subcommand-stop-server.md)
[uninitialize 服务器选项](the-uninitialize-server-option.md)
