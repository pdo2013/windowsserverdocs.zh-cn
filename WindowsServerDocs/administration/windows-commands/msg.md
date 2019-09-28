---
title: msg
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9501cf3e-568e-4982-9987-8daecc6c17ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42a614f313d1e68dbf78d19a498563b541c52be1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373431"
---
# <a name="msg"></a>msg

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

向远程桌面会话主机（rd 会话主机）服务器上的用户发送消息。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解最新版本中的新增功能，请参阅 Windows server TechNet 库中的[Windows server 2012 远程桌面服务中的新增功能](https://technet.microsoft.com/library/hh831527)。

## <a name="syntax"></a>语法
```
msg {<UserName> | <SessionName> | <SessionID>| @<FileName> | *} [/server:<ServerName>] [/time:<Seconds>] [/v] [/w] [<Message>]
```

## <a name="parameters"></a>Parameters

|      参数       |                                                                                                                               描述                                                                                                                               |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      <UserName>      |                                                                                                  指定要接收该消息的用户的名称。                                                                                                   |
|    <SessionName>     |                                                                                                 指定要接收消息的会话的名称。                                                                                                 |
|     <SessionID>      |                                                                                            指定要接收消息的会话的数字 ID。                                                                                            |
|     @<FileName>      |                                                                         标识一个文件，该文件包含要接收消息的用户名、会话名称和会话 Id 的列表。                                                                         |
|          \*          |                                                                                                           将消息发送到系统中的所有用户名。                                                                                                            |
| /server:<ServerName> |                                              指定要接收消息的会话或用户的 rd 会话主机服务器。 如果未指定， **/server**将使用你当前登录到的服务器。                                              |
|   /time： <Seconds>    | 指定您发送的消息在用户屏幕上显示的时间长度。 在达到时间限制后，消息将消失。 如果未设置时间限制，则在用户看到该消息并单击 **"确定"** 之前，消息将保留在用户屏幕上。 |
|          /v          |                                                                                                         显示要执行的操作的相关信息。                                                                                                         |
|          /w          |         等待用户确认已收到消息。 如果用户不立即响应，请将此参数与 **/time：** <*秒*>，以避免可能的长时间延迟。 将此参数与 **/v**一起使用也很有用。          |
|      <Message>       |                  指定要发送的消息的文本。 如果未指定邮件，系统将提示您输入一条消息。 若要发送文件中包含的消息，请键入小于号（<）符号，后跟文件名。                  |
|          /?          |                                                                                                                  在命令提示符下显示帮助。                                                                                                                   |

## <a name="remarks"></a>备注
-   如果未指定用户或会话， **msg**会显示错误消息。 指定会话时，它必须是活动的。
-   用户必须具有消息特殊访问权限才能发送消息。

## <a name="BKMK_examples"></a>示例
-   若要向 User1 的所有会话发送名为 "我们今天下午1点的" 的消息，请键入：
    ```
    msg User1 Let's meet at 1PM today
    ```
-   若要将同一消息发送到 session modeM02，请键入：
    ```
    msg modem02 Let's meet at 1PM today
    ```
-   若要将消息发送到会话12，请键入：
    ```
    msg 12 Let's meet at 1PM today
    ```
-   若要将消息发送到文件 USERlist 中包含的所有会话，请键入：
    ```
    msg @userlist Let's meet at 1PM today
    ```
-   若要将消息发送给所有已登录的用户，请键入：
    ```
    msg * Let's meet at 1PM today
    ```
-   若要将消息发送给所有用户，并且具有确认超时（例如10秒），请键入：
    ```
    msg * /time:10 Let's meet at 1PM today
    ```

#### <a name="additional-references"></a>其他参考
-  [命令行语法项](command-line-syntax-key.md)
-  [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
