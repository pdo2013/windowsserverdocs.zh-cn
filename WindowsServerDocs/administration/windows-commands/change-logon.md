---
title: change logon
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 45c171a1b14cf69abf039d57697cad933a2dd87b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434576"
---
>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

# <a name="change-logon"></a>change logon
启用或禁用从客户端会话，登录或显示当前登录状态。
此实用程序可用于系统维护。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。
> ## <a name="syntax"></a>语法
> ```
> change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
> ```
> ## <a name="parameters"></a>Parameters
> 
> |     参数      |                                                       描述                                                        |
> |--------------------|--------------------------------------------------------------------------------------------------------------------------|
> |       /query       |                             显示当前登录状态，是否启用或禁用。                              |
> |      /enable       |                              允许从客户端会话，但不能从控制台登录登录。                              |
> |      /disable      |  禁用从客户端会话，但不能从控制台的后续登录。 不会影响当前登录的用户。   |
> |       /drain       |                 禁止从新的客户端会话，登录，但允许重新连接到现有会话。                 |
> | /drainuntilrestart | 禁止从新的客户端会话，直到计算机重新启动，但允许重新连接到现有会话登录。 |
> |         /?         |                                           在命令提示符下显示帮助。                                           |
> 
> ## <a name="remarks"></a>备注
> - 只有管理员才能使用**更改登录**命令。
> - 重新启动系统时，会重新启用登录。 如果从客户端会话连接到远程桌面会话主机 (rd 会话主机) 服务器和禁止登录，然后重新启用登录之前先注销，你将不能重新连接到你的会话。 若要重新启用从客户端会话登录，请在控制台上登录。
>   ## <a name="BKMK_examples"></a>示例
> - 若要显示当前登录状态，请键入：
>   ```
>   change logon /query
>   ```
> - 若要启用从客户端会话登录，请键入：
>   ```
>   change logon /enable
>   ```
> - 若要禁用的客户端登录，请键入：
>   ```
>   change logon /disable
>   ```
>   #### <a name="additional-references"></a>其他参考
>   [命令行语法解答](command-line-syntax-key.md)
>   [更改](change.md)
>   [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
