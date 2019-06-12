---
title: 使用 get AllNamespaces 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b77bb80238ee63cc0d71d88592d75850720e33b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440520"
---
# <a name="using-the-get-allnamespaces-command"></a>使用 get AllNamespaces 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示服务器上的所有命名空间有关的信息。
## <a name="syntax"></a>语法
Windows Server 2008：
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/Show:Clients] [/ExcludedeletePending]
```
Windows Server 2008 R2：
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/details:Clients] [/ExcludedeletePending]
```
## <a name="parameters"></a>Parameters

|         参数         |                                                                               Windows Server 2008                                                                               | Windows Server 2008 R2 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
|  [/ 服务器：<Server name>]  | 指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。 |                        |
| [/ ContentProvider:<name>] |                                                        指定内容提供程序仅显示命名空间。                                                         |                        |
|      [/Show:Clients]      |                            仅支持 Windows Server 2008。 显示有关客户端计算机连接到命名空间的信息。                             |                        |
|    [/details:Clients]     |                           仅支持 Windows Server 2008 R2。 显示有关客户端计算机连接到命名空间的信息。                           |                        |
|  [/ExcludedeletePending]  |                                                              从列表中排除任何已停用的传输。                                                              |                        |

## <a name="BKMK_examples"></a>示例
若要查看所有命名空间，请键入：
```
wdsutil /Get-AllNamespaces
```
若要查看除处于非活动状态的所有命名空间，请键入：
- Windows Server 2008
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /Show:Clients /ExcludedeletePending
  ```
- Windows Server 2008 R2
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /details:Clients /ExcludedeletePending
  ```
  #### <a name="additional-references"></a>其他参考
  [命令行语法解答](command-line-syntax-key.md)
  [使用新 Namespace 命令](using-the-new-namespace-command.md)
  [使用删除 Namespace 命令](using-the-remove-namespace-command.md)
   [子命令： 开始-Namespace](subcommand-start-namespace.md)
