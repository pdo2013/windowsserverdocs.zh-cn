---
title: 注销
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 939f09cc-de8c-436c-a05d-aca5f2a06371
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d09b58823f12d0b26bf21c00638b58046119bdab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374226"
---
# <a name="logoff"></a>注销

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

从远程桌面会话主机（rd 会话主机）服务器上的会话中注销用户，并从服务器中删除该会话。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解最新版本中的新增功能，请参阅 Windows server TechNet 库中的[Windows server 2012 远程桌面服务中的新增功能](https://technet.microsoft.com/library/hh831527)。

## <a name="syntax"></a>语法
```
logoff [<SessionName> | <SessionID>] [/server:<ServerName>] [/v]
```
## <a name="parameters"></a>Parameters

|      参数       |                                                                             描述                                                                              |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <SessionName>     |                                                                  指定会话的名称。                                                                  |
|     <SessionID>      |                                                 指定标识服务器会话的数字 ID。                                                 |
| /server:<ServerName> | 指定包含要注销其用户的会话的 rd 会话主机服务器。 如果未指定，则使用当前处于活动状态的服务器。 |
|          /v          |                                                       显示要执行的操作的相关信息。                                                        |
|          /?          |                                                                 在命令提示符下显示帮助。                                                                 |

## <a name="remarks"></a>备注
- 你始终可以从你当前登录到的会话中注销。 但是，您必须拥有 "完全控制" 权限才能从其他会话注销用户。
- 从会话中注销用户而不发出警告可能导致用户会话中的数据丢失。 在执行此操作之前，应使用**msg**命令向用户发送消息来警告用户。
- 如果未指定 <*SessionID*> 或 <*SessionName*>，则**注销**将从当前会话注销用户。 如果指定 <*SessionName*>，则它必须是活动的。
- 注销用户时，所有进程都将结束，并且将从服务器中删除该会话。
- 不能从控制台会话中注销用户。
  ## <a name="BKMK_examples"></a>示例
- 若要从当前会话中注销用户，请键入：
  ```
  logoff
  ```
- 若要使用会话的 ID （例如会话12）从会话中注销用户，请键入：
  ```
  logoff 12
  ```
- 若要使用会话和服务器的名称从会话中注销用户，例如，在 Server1 上使用 session TERM04，请键入：
  ```
  logoff TERM04 /server:Server1
  ```

#### <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)
-   [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
