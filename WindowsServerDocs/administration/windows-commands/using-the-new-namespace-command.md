---
title: 使用新 Namespace 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6df60703-30bd-4d59-a8d9-9fe3efe96add
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50d51101afe95c99b7034fc50b3d30b799ee02ce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871148"
---
# <a name="using-the-new-namespace-command"></a>使用新 Namespace 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建并配置新的命名空间。 必须仅安装传输服务器角色服务时，应使用此选项。 如果必须部署服务器角色服务和传输服务器角色服务安装 （这是默认值），请使用[使用新 MulticastTransmission 命令](using-the-new-multicasttransmission-command.md)。 请注意，在使用此选项之前，必须注册的内容提供程序。
## <a name="syntax"></a>语法
```
wdsutil [Options] /New-Namespace [/Server:<Server name>]
     /FriendlyName:<Friendly name>
     [/Description:<Description>]
     /Namespace:<Namespace name>
     /ContentProvider:<Name>
     [/ConfigString:<Configuration string>]
     /Namespacetype: {AutoCast | ScheduledCast}
         [/time:<YYYY/MM/DD:hh:mm>]
         [/Clients:<Number of clients>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则使用本地服务器。|
|/Friendlyname:<Friendly name>|指定的命名空间的友好名称。|
|[/ 说明：<Description>]|设置命名空间的说明。|
|/ Namespace:<Namespace name>|指定的命名空间名称。 请注意，这不是友好名称，并且它必须是唯一。<br /><br />-   **部署服务器角色服务**:此选项的语法是 /Namespace:WDS:<Image group>/<Image name>/<Index>。 例如：**WDS:ImageGroup1/install.wim/1**<br />-   **传输服务器角色服务**:此值应与命名空间已在服务器上创建时指定的名称匹配。|
|/ContentProvider:<Name>]|指定将为命名空间提供内容的内容提供商的名称。|
|[/ConfigString:<Configuration string>]|指定内容提供程序的配置字符串。|
|/Namespacetype: {AutoCast &#124; ScheduledCast}|指定传输的设置。 指定设置，请使用以下选项：<br /><br />-[/time: <time>]-设置传输应首先使用以下格式的时间：YYYY/MM/DD:hh:mm。 此选项仅适用于 Scheduled-cast 传输。<br />-[/ 客户端： <Number of clients>]-设置客户端传输开始前等待的最小数量。 此选项仅适用于 Scheduled-cast 传输。|
## <a name="BKMK_examples"></a>示例
若要创建 Auto-cast 命名空间，请键入：
```
wdsutil /New-Namespace /FriendlyName:"Custom AutoCast Namespace" /Namespace:"Custom Auto 1" /ContentProvider:MyContentProvider /Namespacetype:AutoCast
```
若要创建 Scheduled-cast 命名空间，请键入：
```
wdsutil /New-Namespace /Server:MyWDSServer /FriendlyName:"Custom Scheduled Namespace" /Namespace:"Custom Auto 1" /ContentProvider:MyContentProvider 
/Namespacetype:ScheduledCast /time:"2006/11/20:17:00" /Clients:20
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用 get AllNamespaces 命令](using-the-get-allnamespaces-command.md)
[使用删除 Namespace 命令](using-the-remove-namespace-command.md)
 [子命令： 开始-Namespace](subcommand-start-namespace.md)
