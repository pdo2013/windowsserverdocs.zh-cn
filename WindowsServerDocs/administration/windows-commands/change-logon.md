---
title: change logon
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41466260-aee9-4333-bcb6-178112c22afd Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c04eaffe366dce079aed53351589c1b5026954e3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379642"
---
>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

# <a name="change-logon"></a>change logon
启用或禁用来自客户端会话的登录，或者显示当前登录状态。
此实用程序对于系统维护非常有用。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解最新版本中的新增功能，请参阅 Windows server TechNet 库中的[Windows server 2012 远程桌面服务中的新增功能](https://technet.microsoft.com/library/hh831527)。
> ## <a name="syntax"></a>语法
> ```
> change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
> ```
> ## <a name="parameters"></a>Parameters
> 
> |     参数      |                                                       描述                                                        |
> |--------------------|--------------------------------------------------------------------------------------------------------------------------|
> |       /query       |                             显示当前登录状态，不管是启用还是禁用。                              |
> |      /enable       |                              允许来自客户端会话的登录，但不允许来自控制台的登录。                              |
> |      /disable      |  禁止来自客户端会话的后续登录，而不是从控制台中登录。 不会影响当前登录的用户。   |
> |       /drain       |                 禁止从新客户端会话登录，但允许重新断开现有会话的登录。                 |
> | /drainuntilrestart | 禁止在重新启动计算机之前从新的客户端会话登录，但允许重新与现有会话进行重新启动。 |
> |         /?         |                                           在命令提示符下显示帮助。                                           |
> 
> ## <a name="remarks"></a>备注
> - 只有管理员才能使用**change logon**命令。
> - 重新启动系统时，将重新启用登录。 如果从客户端会话连接到远程桌面会话主机（rd 会话主机）服务器并禁用登录，然后在重新启用登录之前注销，则无法重新连接到会话。 若要重新启用来自客户端会话的登录，请在控制台上登录。
>   ## <a name="BKMK_examples"></a>示例
> - 若要显示当前登录状态，请键入：
>   ```
>   change logon /query
>   ```
> - 若要允许从客户端会话登录，请键入：
>   ```
>   change logon /enable
>   ```
> - 若要禁用客户端登录，请键入：
>   ```
>   change logon /disable
>   ```
>   #### <a name="additional-references"></a>其他参考
>   [命令行语法关键字](command-line-syntax-key.md)
>   [更改](change.md)
>   [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
