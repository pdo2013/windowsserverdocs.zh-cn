---
title: 使用 AllNamespaces 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0cd90fc650271c863459dd809e47ca6309132de5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363289"
---
# <a name="using-the-get-allnamespaces-command"></a>使用 AllNamespaces 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示有关服务器上的所有命名空间的信息。
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
|  [/Server： @no__t]  | 指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。 |                        |
| [/ContentProvider： @no__t] |                                                        仅显示指定内容提供程序的命名空间。                                                         |                        |
|      [/Show：个客户端]      |                            仅支持 Windows Server 2008。 显示有关连接到命名空间的客户端计算机的信息。                             |                        |
|    [/details：客户端]     |                           仅支持 Windows Server 2008 R2。 显示有关连接到命名空间的客户端计算机的信息。                           |                        |
|  [/ExcludedeletePending]  |                                                              从列表中排除任何已停用的传输。                                                              |                        |

## <a name="BKMK_examples"></a>示例
若要查看所有命名空间，请键入：
```
wdsutil /Get-AllNamespaces
```
若要查看除已停用的命名空间之外的所有命名空间，请键入：
- Windows Server 2008
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /Show:Clients /ExcludedeletePending
  ```
- Windows Server 2008 R2
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /details:Clients /ExcludedeletePending
  ```
  #### <a name="additional-references"></a>其他参考
  [命令行语法关键字](command-line-syntax-key.md)
  [使用新的命名空间命令](using-the-new-namespace-command.md)
  [使用删除命名空间](using-the-remove-namespace-command.md)命令 
   子命令[：起始-命名](subcommand-start-namespace.md)空间
