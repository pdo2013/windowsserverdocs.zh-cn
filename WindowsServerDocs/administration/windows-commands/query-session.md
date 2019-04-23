---
title: 查询会话
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1b96f7e6a6252b40e5a32910d161b1fa0ff94e11
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868888"
---
# <a name="query-session"></a>查询会话

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

远程桌面会话主机 (rd 会话主机) 服务器上将显示有关会话的信息。
此列表包括不仅关于活动的会话，也关于服务器运行的其他会话的信息。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。
## <a name="syntax"></a>语法
```
query session [<SessionName> | <UserName> | <SessionID>] [/server:<ServerName>] [/mode] [/flow] [/connect] [/counter]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|<SessionName>|指定您想要查询的会话的名称。|
|<UserName>|指定您想要查询其会话的用户的名称。|
|<SessionID>|指定您想要查询的会话的 ID。|
|/server:<ServerName>|标识查询的 rd 会话主机服务器。 默认值为当前服务器。|
|/mode|显示当前行设置。|
|/flow|显示当前的流控制设置。|
|/connect|显示当前连接设置。|
|/counter|显示当前的计数器信息，包括创建，会话总数断开连接，并重新连接。|
|/?|在命令提示符下显示帮助。|
## <a name="remarks"></a>备注
-   用户始终可以查询该用户当前登录到该会话。 若要查询的其他会话，用户必须具有查询信息特殊访问权限。
-   如果不通过使用指定会话 <*SessionName*>，<*用户名*>，或 <*SessionID*>，**查询会话**在系统中显示有关所有活动会话的信息。
-   当**查询会话**在当前会话之前显示返回信息，一个大于 (>) 符号。 下面是示例输出**查询会话**:
    ```
    C:\>query session
     SESSIONNAME    USERNAME       ID STATE  TYPE   DEVICE
    >console        Administrator1  0 active wdcon
     rdp-tcp#1      User1           1 active wdtshare
     rdp-tcp                        2 listen wdtshare
                                    4 idle
                                    5 idle
    ```
    大于 (>) 符号指示当前会话。 会话名指定分配给会话的名称。 用户名指示连接到会话的用户的用户名。 状态提供了有关该会话的当前状态的信息。 类型指示会话类型。 不存在可用于在控制台或网络连接的会话，设备是分配给会话的设备名称。 从会话配置文件后的会话信息的注释。 在其中的初始状态都配置为禁用的任何会话中不显示**查询会话**列表之前启用它们。
## <a name="BKMK_examples"></a>示例
-   若要在服务器 SERver2 上显示有关所有活动会话的信息，请键入：
    ```
    query session /server:SERver2
    ```
-   若要显示活动会话 modeM02 有关的信息，请键入：
    ```
    query session modeM02
    ```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[查询](query.md)
[远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
