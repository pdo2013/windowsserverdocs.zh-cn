---
title: 查询进程
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 36ce3ffc-0092-4eb1-a374-28e6616ca946
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3248c2b1f476a1a9843f7e930b05a3dca812d694
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442123"
---
# <a name="query-process"></a>查询进程

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示有关远程桌面会话主机 (rd 会话主机) 服务器运行的进程的信息。
可以使用此命令来找出特定用户正在运行，哪些程序和哪些用户还运行特定程序。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。
> ## <a name="syntax"></a>语法
> ```
> query process [* | <ProcessID> | <UserName> | <SessionName> | /id:<nn> | <ProgramName>] [/server:<ServerName>]
> ```
> ## <a name="parameters"></a>Parameters
> 
> |      参数       |                                                                 描述                                                                  |
> |----------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
> |          \*          |                                                    列出的所有会话的进程。                                                     |
> |     <ProcessID>      |                                   指定标识您想要查询的进程的数字 ID。                                   |
> |      <UserName>      |                                       指定想要列出其进程的用户的名称。                                       |
> |    <SessionName>     |                                     指定想要列出其进程的会话的名称。                                      |
> |       /id:<nn>       |                                      指定想要列出其进程的会话 ID。                                       |
> |    <ProgramName>     |                     指定您想要查询其进程的程序的名称。 .Exe 扩展名是必需的。                     |
> | /server:<ServerName> | 指定的 rd 会话主机服务器想要列出其进程。 如果未指定，则使用在其中您当前已登录的服务器。 |
> |          /?          |                                                     在命令提示符下显示帮助。                                                     |
> 
> ## <a name="remarks"></a>备注
> - 管理员可以完全访问所有**查询过程**函数。
> - 如果未指定 <*用户名*>，<*SessionName*>， **/id:** <*nn*>，<*ProgramName*>，或 **\\** * 参数**查询进程**显示仅属于当前用户的进程。
> - 如果指定一个会话，则它必须确定活动的会话。
> - **查询进程**返回以下信息：
>   -   拥有进程的用户
>   -   拥有进程的会话
>   -   会话的 ID
>   -   进程的名称
>   -   进程 ID
> - 当**查询过程**之前属于当前会话的每个进程显示返回的信息，一个大于 (>) 符号。
>   ## <a name="BKMK_examples"></a>示例
> - 若要显示有关正在使用的所有会话的进程的信息，请键入：
>   ```
>   query process *
>   ```
> - 若要显示有关正在使用会话 ID 为 2 的进程的信息，请键入：
>   ```
>   query process /ID:2
>   ```
>   #### <a name="additional-references"></a>其他参考
>   [命令行语法解答](command-line-syntax-key.md)
>   [查询](query.md)
>   [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
