---
title: 查询会话
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abc0ace8-0b74-4b6e-a937-a78bb4b61a1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bde9a246f2c46eaa466f2863c2cfc3c28a3a04eb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384908"
---
# <a name="query-session"></a>查询会话

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示有关远程桌面会话主机（rd 会话主机）服务器上的会话的信息。
此列表不仅包括有关活动会话的信息，还包括有关服务器运行的其他会话的信息。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解最新版本中的新增功能，请参阅 Windows server TechNet 库中的[Windows server 2012 远程桌面服务中的新增功能](https://technet.microsoft.com/library/hh831527)。
> ## <a name="syntax"></a>语法
> ```
> query session [<SessionName> | <UserName> | <SessionID>] [/server:<ServerName>] [/mode] [/flow] [/connect] [/counter]
> ```
> ## <a name="parameters"></a>Parameters
> 
> |      参数       |                                                      描述                                                      |
> |----------------------|-----------------------------------------------------------------------------------------------------------------------|
> |    <SessionName>     |                               指定要查询的会话的名称。                               |
> |      <UserName>      |                           指定要查询其会话的用户的名称。                            |
> |     <SessionID>      |                                指定要查询的会话的 ID。                                |
> | /server:<ServerName> |                  标识要查询的 rd 会话主机服务器。 默认值为当前服务器。                   |
> |        /mode         |                                            显示当前行设置。                                            |
> |        /flow         |                                        显示当前的流控制设置。                                        |
> |       /connect       |                                          显示当前连接设置。                                           |
> |       /counter       | 显示当前计数器信息，包括创建、断开连接和重新连接的会话总数。 |
> |          /?          |                                         在命令提示符下显示帮助。                                          |
> 
> ## <a name="remarks"></a>备注
> - 用户始终可以查询用户当前登录到的会话。 若要查询其他会话，用户必须具有查询信息特殊访问权限。
> - 如果未使用 <*SessionName*> 指定会话、<*UserName*> 或 <*SessionID*>，则**查询会话**将显示系统中所有活动会话的相关信息。
> - 当**查询会话**返回信息时，将在当前会话之前显示大于号（>）。 下面是**查询会话**的示例输出：
>   ```
>   C:\>query session
>    SESSIONNAME    USERNAME       ID STATE  TYPE   DEVICE
>   console        Administrator1  0 active wdcon
>    rdp-tcp#1      User1           1 active wdtshare
>    rdp-tcp                        2 listen wdtshare
>                                   4 idle
>                                   5 idle
>   ```
>   大于号（>）表示当前会话。 SESSIONNAME 指定分配给会话的名称。 用户名表示连接到该会话的用户的用户名。 状态提供有关会话的当前状态的信息。 类型指示会话类型。 设备（对于控制台或网络连接的会话不存在）是分配给会话的设备名称。 以下会话信息的注释来自会话配置文件。 初始状态配置为 "已禁用" 的任何会话都不会显示在 "**查询会话**" 列表中，直到启用它们。
>   ## <a name="BKMK_examples"></a>示例
> - 若要显示有关服务器 SERver2 上的所有活动会话的信息，请键入：
>   ```
>   query session /server:SERver2
>   ```
> - 若要显示有关活动会话 modeM02 的信息，请键入：
>   ```
>   query session modeM02
>   ```
>   #### <a name="additional-references"></a>其他参考
>   [命令行语法关键字](command-line-syntax-key.md)
>   [query](query.md)
>   [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
