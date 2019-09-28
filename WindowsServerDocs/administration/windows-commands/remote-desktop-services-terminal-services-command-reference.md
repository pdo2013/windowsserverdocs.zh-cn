---
title: 远程桌面服务（终端服务）命令参考
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f371848-5c48-470c-908c-afbc95d3a805
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d13d6ac2e423c5a07a2a84af5e17fe9081cd70f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371610"
---
# <a name="remote-desktop-services-terminal-services-command-reference"></a>远程桌面服务（终端服务）命令参考

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

下面是远程桌面服务命令行工具的列表。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解最新版本中的新增功能，请参阅 Windows server TechNet 库中的[Windows server 2012 远程桌面服务中的新增功能](https://technet.microsoft.com/library/hh831527)。
> 
> |                 Command                 |                                                      描述                                                       |
> |-----------------------------------------|------------------------------------------------------------------------------------------------------------------------|
> |           [change](change.md)           | 更改登录、COM 端口映射和安装模式远程桌面会话主机（rd 会话主机）服务器设置。 |
> |     [change logon](change-logon.md)     |    启用或禁用从 rd 会话主机服务器上的客户端会话登录，或显示当前登录状态。     |
> |      [change port](change-port.md)      |                   列出或更改 COM 端口映射，使其与 MS-DOS 应用程序兼容。                    |
> |      [change user](change-user.md)      |                                更改 rd 会话主机服务器的安装模式。                                |
> |         [chglogon](chglogon.md)         |    启用或禁用从 rd 会话主机服务器上的客户端会话登录，或显示当前登录状态。     |
> |          [chgport](chgport.md)          |                   列出或更改 COM 端口映射，使其与 MS-DOS 应用程序兼容。                    |
> |           [chgusr](chgusr.md)           |                                更改 rd 会话主机服务器的安装模式。                                |
> |         [flattemp](flattemp.md)         |                                      启用或禁用平面临时文件夹。                                       |
> |           [logoff](logoff.md)           |          从 rd 会话主机服务器上的会话中注销用户，并从服务器中删除该会话。          |
> |              [msg](msg.md)              |                                向 rd 会话主机服务器上的用户发送消息。                                 |
> |            [mstsc](mstsc.md)            |                       创建与 rd 会话主机服务器或其他远程计算机的连接。                        |
> |          [qappsrv](qappsrv.md)          |                             显示网络上所有 rd 会话主机服务器的列表。                             |
> |         [qprocess](qprocess.md)         |                  显示有关在 rd 会话主机服务器上运行的进程的信息。                   |
> |            [查询](query.md)            |                      显示有关进程、会话和 rd 会话主机服务器的信息。                      |
> |    [查询进程](query-process.md)    |                  显示有关在 rd 会话主机服务器上运行的进程的信息。                   |
> |    [查询会话](query-session.md)    |                           显示有关 rd 会话主机服务器上的会话的信息。                            |
> | [查询 termserver](query-termserver.md) |                             显示网络上所有 rd 会话主机服务器的列表。                             |
> |       [查询用户](query-user.md)       |                         显示有关 rd 会话主机服务器上的用户会话的信息。                         |
> |            [quser](quser.md)            |                         显示有关 rd 会话主机服务器上的用户会话的信息。                         |
> |          [qwinsta](qwinsta.md)          |                           显示有关 rd 会话主机服务器上的会话的信息。                            |
> |          [rdpsign](rdpsign.md)          |                          使您能够对远程桌面协议（.rdp）文件进行数字签名。                          |
> |    [reset session](reset-session.md)    |                         使你能够在 rd 会话主机服务器上重置（删除）会话。                          |
> |          [rwinsta](rwinsta.md)          |                         使你能够在 rd 会话主机服务器上重置（删除）会话。                          |
> |           [shadow](shadow.md)           |            允许您远程控制 rd 会话主机服务器上其他用户的活动会话。             |
> |            [tscon](tscon.md)            |                               连接到 rd 会话主机服务器上的另一个会话。                                |
> |         [tsdiscon](tsdiscon.md)         |                                 断开会话与 rd 会话主机服务器的连接。                                  |
> |           [tskill](tskill.md)           |                           结束在 rd 会话主机服务器上的会话中运行的进程。                            |
> |           [tsprof](tsprof.md)           |              将远程桌面服务用户配置信息从一个用户复制到另一个用户。               |
