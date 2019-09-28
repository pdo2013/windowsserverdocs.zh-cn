---
title: 使用移除命名空间命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b4c087442c43fd885fe4554cb29f9b2788420e05
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362790"
---
# <a name="using-the-remove-namespace-command"></a>使用移除命名空间命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

删除自定义命名空间。
## <a name="syntax"></a>语法
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/Namespace： <Namespace name>|指定命名空间的名称。 这不是友好名称，并且必须是唯一的。<br /><br />-   **部署服务器角色服务**：命名空间名称的语法为/Namespace： WDS： <ImageGroup> @ no__t @ no__t-2 @ no__t-3 @ no__t-4。 例如：**WDS： ImageGroup1/install/1**<br />-   **传输服务器角色服务**：此值必须与在服务器上创建命名空间时为命名空间指定的名称相匹配。|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，则使用本地服务器。|
|/force|立即删除命名空间并终止所有客户端。 请注意，除非指定 **/force**，否则现有的客户端可以完成传输，但无法加入新的客户端。|
## <a name="BKMK_examples"></a>示例
若要停止命名空间（当前客户端可以完成传输但新的客户端无法加入），请键入：
```
wdsutil /remove-Namespace /Namespace:"Custom Auto 1"
```
若要强制终止所有客户端，请键入：
```
wdsutil /remove-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /force
```
#### <a name="additional-references"></a>其他参考
[命令行语法键](command-line-syntax-key.md)
[使用 AllNamespaces 命令](using-the-get-allnamespaces-command.md)
 使用[新的命名空间命令](using-the-new-namespace-command.md)
[子命令：启动-命名空间](subcommand-start-namespace.md)
