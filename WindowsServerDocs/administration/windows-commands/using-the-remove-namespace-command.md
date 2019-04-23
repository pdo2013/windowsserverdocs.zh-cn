---
title: 使用删除 Namespace 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 115c0a90a60e18ee4b89758200773d1dfec2163f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842038"
---
# <a name="using-the-remove-namespace-command"></a>使用删除 Namespace 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

删除自定义的命名空间。
## <a name="syntax"></a>语法
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/ Namespace:<Namespace name>|指定的命名空间名称。 这不是友好名称，并且它必须是唯一。<br /><br />-   **部署服务器角色服务**:命名空间名称的语法是 /Namespace:WDS:<ImageGroup>/<ImageName>/<Index>。 例如：**WDS:ImageGroup1/install.wim/1**<br />-   **传输服务器角色服务**:此值必须匹配在服务器上创建时提供给命名空间的名称。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则使用本地服务器。|
|[/force]|立即删除命名空间，并终止所有客户端。 请注意，除非指定，否则 **/force**，现有客户端可以完成传输，但不能加入新客户端。|
## <a name="BKMK_examples"></a>示例
若要停止一个命名空间 （当前客户端可以完成传输，但不能加入新客户端），类型：
```
wdsutil /remove-Namespace /Namespace:"Custom Auto 1"
```
若要强制终止的所有客户端，请键入：
```
wdsutil /remove-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /force
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用 get AllNamespaces 命令](using-the-get-allnamespaces-command.md)
[使用新 Namespace 命令](using-the-new-namespace-command.md)
 [子命令： 开始-Namespace](subcommand-start-namespace.md)
