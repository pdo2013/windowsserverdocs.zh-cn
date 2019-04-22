---
title: 查询用户
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a670fb78-c055-464a-b61d-3a85632c52c5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e54d1b9701cc2f3a4f229a3910d5b325fee87c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813938"
---
# <a name="query-user"></a>查询用户

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

远程桌面会话主机 (rd 会话主机) 服务器上将显示有关用户会话的信息。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。
## <a name="syntax"></a>语法
```
query user [<UserName> | <SessionName> | <SessionID>] [/server:<ServerName>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|<UserName>|指定您想要查询的用户的登录名。|
|<SessionName>|指定您想要查询的会话的名称。|
|<SessionID>|指定您想要查询的会话的 ID。|
|/server:<ServerName>|指定您想要查询的 rd 会话主机服务器。 否则，使用当前的 rd 会话主机服务器。|
|/?|在命令提示符下显示帮助。|
## <a name="remarks"></a>备注
-   此命令可用于找出是否特定的用户登录到特定的 rd 会话主机服务器。 **查询用户**返回以下信息：
    -   用户的名称
    -   Rd 会话主机服务器上的会话的名称
    -   会话 ID
    -   会话 （活动或已断开连接） 的状态
    -   空闲时间 （最后一个击键或鼠标移动后在该会话的分钟数）
    -   在用户登录的日期和时间
-   若要使用**查询用户**，您必须将完全控制权限或查询的信息的特殊访问权限。
-   如果您使用**查询用户**而无需指定 <*用户名*>，<*SessionName*>，或 <*SessionID*>，所有的列表登录到服务器的用户，则返回。 或者，还可以使用**查询会话**以显示服务器上的所有会话的列表。
-   当**查询用户**在当前会话之前显示返回信息，一个大于 (>) 符号。
-   **/Server**参数是必需的仅当你使用**查询用户**从远程服务器。
## <a name="BKMK_examples"></a>示例
-   若要显示有关记录在系统上的所有用户的信息，请键入：
    ```
    query user
    ```
-   若要显示服务器 SERver1 上的用户 USER1 的信息，请键入：
    ```
    query user USER1 /server:SERver1
    ```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[查询](query.md)
[远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
