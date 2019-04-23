---
title: 使用 get Namespace 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd11880b6e733850b522c3a06152ac7ce7a28841
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889658"
---
# <a name="using-the-get-namespace-command"></a>使用 get Namespace 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示自定义的命名空间的信息。
## <a name="syntax"></a>语法
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/Show:Clients]
```
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/details:Clients]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/ Namespace:<Namespace name>|指定的命名空间名称。 请注意，这不是友好名称，并且它必须是唯一。<br /><br />部署服务器：命名空间名称的语法是 /Namspace:WDS:<ImageGroup>/<ImageName>/<Index>。 例如：**WDS:ImageGroup1/install.wim/1**<br />-传输服务器：此值应与在服务器上创建时提供给命名空间的名称匹配。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则使用本地服务器。|
|[/ Show： 客户端] 或 [/ 详细信息： 客户端]|显示有关客户端计算机连接到指定的命名空间的信息。|
## <a name="BKMK_examples"></a>示例
若要查看命名空间的信息，请键入：
```
wdsutil /Get-Namespace /Namespace:"Custom Auto 1"
```
若要查看有关命名空间和已连接的客户端的信息，请键入以下项之一：
-   Windows Server 2008: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /Show:Clients`
-   Windows Server 2008 R2: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /details:Clients`
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用 get AllNamespaces 命令](using-the-get-allnamespaces-command.md)
[使用新 Namespace 命令](using-the-new-namespace-command.md)
[使用删除 Namespace 命令](using-the-remove-namespace-command.md)
[子命令： 开始 Namespace](subcommand-start-namespace.md)
