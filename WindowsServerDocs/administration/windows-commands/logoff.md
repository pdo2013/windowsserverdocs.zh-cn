---
title: 注销
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 88b3cbbbd52a965fd0a0de3c164e8dbc1f90d053
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437522"
---
# <a name="logoff"></a>注销

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

注销远程桌面会话主机 (rd 会话主机) 服务器上的会话中的用户，并从服务器中删除该会话。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。

## <a name="syntax"></a>语法
```
logoff [<SessionName> | <SessionID>] [/server:<ServerName>] [/v]
```
## <a name="parameters"></a>Parameters

|      参数       |                                                                             描述                                                                              |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <SessionName>     |                                                                  指定会话的名称。                                                                  |
|     <SessionID>      |                                                 指定用于标识连接到服务器的会话的数字 ID。                                                 |
| /server:<ServerName> | 指定包含你想要注销的用户会话的 rd 会话主机服务器。 如果未指定，则使用在其上你当前处于活动状态的服务器。 |
|          /v          |                                                       显示有关正在执行的操作的信息。                                                        |
|          /?          |                                                                 在命令提示符下显示帮助。                                                                 |

## <a name="remarks"></a>备注
- 始终可以注销您当前登录到会话中。 但是，必须有完全控制权限才能从其他会话注销用户。
- 正在注销用户而不发出警告的会话中可导致用户的会话数据丢失。 应通过向用户发送一条消息**msg**命令来执行此操作之前警告用户。
- 如果 <*SessionID*> 或 <*SessionName*> 未指定，则**注销**注销当前会话中的用户。 如果指定 <*SessionName*>，它必须是有效名称。
- 当您注销用户时，所有进程结束，该会话是从服务器中删除。
- 无法注销控制台会话中的用户。
  ## <a name="BKMK_examples"></a>示例
- 若要注销当前会话中的用户，请键入：
  ```
  logoff
  ```
- 若要使用该会话的 ID，例如会话 12 注销用户从会话，请键入：
  ```
  logoff 12
  ```
- 若要注销用户从会话时使用的会话和服务器名称，例如会话 TERM04 在 Server1 上，键入：
  ```
  logoff TERM04 /server:Server1
  ```

#### <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)
-   [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
