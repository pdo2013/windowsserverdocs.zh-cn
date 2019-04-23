---
title: 子命令开始 Namespace
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5dc484a3b159cd510d7a61484f4f46b1f1db7a4e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877128"
---
# <a name="subcommand-start-namespace"></a>子命令： 开始-Namespace

>适用于：Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 启动 Scheduled-cast 命名空间。
## <a name="syntax"></a>语法
```
wdsutil /start-Namespace /Namespace:<Namespace name> [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/ Namespace:<Namespace name>|指定的命名空间名称。 请注意，这不是友好名称，并且它必须是唯一。<br /><br />-   **部署服务器**:命名空间名称的语法是 /Namspace:WDS:<Image group>/<Image name>/<Index>。 例如：**WDS:ImageGroup1/install.wim/1**<br />-   **传输服务器**:此名称必须与在服务器上创建时提供给命名空间的名称匹配。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
## <a name="BKMK_examples"></a>示例
若要启动一个命名空间，请键入以下项之一：
```
wdsutil /start-Namespace /Namespace:"Custom Auto 1"
wdsutil /start-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1"
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用 get AllNamespaces 命令](using-the-get-allnamespaces-command.md)
[使用新 Namespace 命令](using-the-new-namespace-command.md)
[使用删除 Namespace 命令](using-the-remove-namespace-command.md)
