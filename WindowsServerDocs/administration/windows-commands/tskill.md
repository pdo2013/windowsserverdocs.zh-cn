---
title: tskill
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 08986e6a-6900-4ece-85a1-8f73b14db1b3 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 59958481a7c832aca7bc25d7d4d3ebbf4e8ef80c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835038"
---
# <a name="tskill"></a>tskill

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

结束远程桌面会话主机 (rd 会话主机) 服务器上的会话中运行的进程。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。

## <a name="syntax"></a>语法
```
tskill {<ProcessID> | <ProcessName>} [/server:<ServerName>] [/id:<SessionID> | /a] [/v]
```

## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|\<ProcessID>|指定你想要结束的进程 ID。|
|\<ProcessName>|指定你想要结束的进程的名称。 此参数可以包含通配符字符。|
|/server:\<服务器名 >|指定包含要结束的进程的终端服务器。 如果 **/server**未指定，则使用当前的 RD 会话主机服务器。|
|/id:\<SessionID >|结束在指定的会话中运行的进程。|
|/a|结束所有会话中运行的进程。|
|/v|显示有关正在执行的操作的信息。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   可以使用**tskill**结束仅属于你，这些进程，除非你是管理员。 管理员可以完全访问所有**tskill**函数，而可以结束其他用户会话中运行的进程。
-   当在会话中运行的所有进程都结束时，会话也将结束。
-   如果您使用*ProcessName*和 **/server: * * * ServerName*参数，则还必须指定 **/i d: * * * SessionID*或  **/a**参数。

## <a name="BKMK_examples"></a>示例
-   若要结束进程 6543，请键入：
    ```
    tskill 6543
    ```
-   若要结束进程"explorer"会话 5 上运行，请键入：
    ```
    tskill explorer /id:5
    ```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[远程桌面服务 & #40;终端服务和 #41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
