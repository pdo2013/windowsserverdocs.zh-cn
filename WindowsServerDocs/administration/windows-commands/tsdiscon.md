---
title: tsdiscon
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1b5fca329864ebed9eab66671a17493f0fc3ca8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440909"
---
# <a name="tsdiscon"></a>tsdiscon

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

断开会话连接从远程桌面会话主机 (rd 会话主机) 服务器。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。

## <a name="syntax"></a>语法
```
tsdiscon [<SessionID> | <SessionName>] [/server:<ServerName>] [/v]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|\<SessionId>|指定要断开连接的会话 ID。|
|\<SessionName>|指定要断开连接的会话的名称。|
|/server:\<服务器名 >|指定包含你想要断开连接的会话的终端服务器。 否则，使用当前的 rd 会话主机服务器。|
|/v|显示有关正在执行的操作的信息。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   必须将完全控制权限或断开连接断开与会话的另一个用户的特殊访问权限。
-   如果指定没有会话 ID 或会话名称， **tsdiscon**断开当前会话的连接。
-   重新连接到该会话且不会丢失的数据时自动运行时中断了会话正在运行的任何应用程序。 使用**重置会话**若要结束的断开连接的会话正在运行的应用程序，但请注意，这可能导致数据丢失在该会话。
-   **/Server**参数是必需的仅当你使用**tsdiscon**从远程服务器。
-   无法断开连接的控制台会话。

## <a name="BKMK_examples"></a>示例
- 若要断开当前会话，请键入：
  ```
  tsdiscon
  ```
- 若要断开连接会话 10，请键入：
  ```
  tsdiscon 10
  ```
- 若要断开名为 TERM04 会话的连接，请键入：
  ```
  tsdiscon TERM04
  ```
  #### <a name="additional-references"></a>其他参考
  [命令行语法解答](command-line-syntax-key.md)
  [远程桌面服务 & #40;终端服务和 #41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
