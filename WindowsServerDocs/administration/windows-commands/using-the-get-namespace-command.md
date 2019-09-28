---
title: 使用获取命名空间命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 607fb758db64cfc938a08b070b520fe2950aa482
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363078"
---
# <a name="using-the-get-namespace-command"></a>使用获取命名空间命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示有关自定义命名空间的信息。
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

|               参数               |                                                                                                                                                                                         描述                                                                                                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /Namespace： <Namespace name>      | 指定命名空间的名称。 请注意，这不是友好名称，并且必须是唯一的。<br /><br />-部署服务器：命名空间名称的语法为/Namspace： WDS： <ImageGroup> @ no__t @ no__t-2 @ no__t-3 @ no__t-4。 例如：**WDS： ImageGroup1/install/1**<br />-传输服务器：当在服务器上创建命名空间时，此值应与命名空间所提供的名称匹配。 |
|        [/Server： @no__t]        |                                                                                                             指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，则使用本地服务器。                                                                                                              |
| [/Show： Clients] 或 [/details： Clients] |                                                                                                                                                  显示有关连接到指定命名空间的客户端计算机的信息。                                                                                                                                                  |

## <a name="BKMK_examples"></a>示例
若要查看有关命名空间的信息，请键入：
```
wdsutil /Get-Namespace /Namespace:"Custom Auto 1"
```
若要查看有关命名空间和连接的客户端的信息，请键入下列内容之一：
- Windows Server 2008： `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /Show:Clients`
- Windows Server 2008 R2： `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /details:Clients`
  #### <a name="additional-references"></a>其他参考
  [命令行语法键](command-line-syntax-key.md)
   使用[AllNamespaces 命令](using-the-get-allnamespaces-command.md)
   使用[新的命名空间](using-the-new-namespace-command.md)命令 
   使用[删除命名空间](using-the-remove-namespace-command.md)命令 @no__t 7[子命令：启动-命名空间](subcommand-start-namespace.md)
