---
title: msg
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 68a393b57a255915b93759b4b26286ce4d838019
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437220"
---
# <a name="msg"></a>msg

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将消息发送到远程桌面会话主机 (rd 会话主机) 服务器上的用户。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。

## <a name="syntax"></a>语法
```
msg {<UserName> | <SessionName> | <SessionID>| @<FileName> | *} [/server:<ServerName>] [/time:<Seconds>] [/v] [/w] [<Message>]
```

## <a name="parameters"></a>Parameters

|      参数       |                                                                                                                               描述                                                                                                                               |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      <UserName>      |                                                                                                  指定你想要接收消息的用户的名称。                                                                                                   |
|    <SessionName>     |                                                                                                 指定你想要接收消息的会话的名称。                                                                                                 |
|     <SessionID>      |                                                                                            指定你想要收到一条消息的用户会话的数字 ID。                                                                                            |
|     @<FileName>      |                                                                         标识包含列表的用户名称、 会话名称和你想要接收消息的会话 Id 的文件。                                                                         |
|          \*          |                                                                                                           将消息发送到系统上的所有用户名称。                                                                                                            |
| /server:<ServerName> |                                              指定的 rd 会话主机服务器的会话或用户想要接收的消息。 如果未指定， **/server**使用您当前登录到服务器。                                              |
|   /time:<Seconds>    | 指定用户的屏幕显示您发送的消息的时间量。 在达到时间限制后，消息将消失。 如果设置没有时间限制，消息保留在用户屏幕上直到用户将看到此消息，并单击**确定**。 |
|          /v          |                                                                                                         显示有关正在执行的操作的信息。                                                                                                         |
|          /w          |         等待来自用户已收到消息确认。 使用此参数与 **/时间：** <*秒*> 若要避免可能长时间的延迟，如果用户没有立即响应。 使用此参数与 **/v**也会有所帮助。          |
|      <Message>       |                  指定你想要发送的消息的文本。 如果不指定任何消息，则将提示您输入一条消息。 若要将发送一条消息，包含在文件中，键入的小于号 (<) 符号后跟的文件的名称。                  |
|          /?          |                                                                                                                  在命令提示符下显示帮助。                                                                                                                   |

## <a name="remarks"></a>备注
-   如果未指定用户或会话中， **msg**显示一条错误消息。 在指定会话时，它必须是一个活动。
-   用户必须具有将消息发送的消息特殊访问权限。

## <a name="BKMK_examples"></a>示例
-   若要发送的消息标题为"让我们来认识在今天下午 1"到所有会话 user1，请键入：
    ```
    msg User1 Let's meet at 1PM today
    ```
-   若要将同一消息发送到会话 modeM02，键入：
    ```
    msg modem02 Let's meet at 1PM today
    ```
-   若要将消息发送到 12 的会话中，键入：
    ```
    msg 12 Let's meet at 1PM today
    ```
-   若要将消息发送到文件 USERlist 中包含的所有会话中，键入：
    ```
    msg @userlist Let's meet at 1PM today
    ```
-   若要将消息发送到所有用户登录，请键入：
    ```
    msg * Let's meet at 1PM today
    ```
-   若要将消息发送到所有用户，确认超时时间 （例如，10 秒），请键入：
    ```
    msg * /time:10 Let's meet at 1PM today
    ```

#### <a name="additional-references"></a>其他参考
-  [命令行语法项](command-line-syntax-key.md)
-  [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
