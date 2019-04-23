---
title: shadow
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f81d9717-6883-4e14-9508-4b2a87e48ea7 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 125b2971d5d4783ea0b974c45b988cbd1c58a2bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877108"
---
# <a name="shadow"></a>shadow

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

可以远程控制的远程桌面会话主机 (rd 会话主机) 服务器上的另一个用户的活动会话。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法
```
shadow {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|\<SessionName>|指定你想要远程控制会话的名称。|
|\<SessionID>|指定你想要远程控制会话的 ID。 使用**查询用户**以显示会话和其会话 Id 的列表。|
|/server:\<服务器名 >|指定包含你想要远程控制会话的 rd 会话主机服务器。 默认情况下，使用当前的 rd 会话 Host4 服务器。|
|/v|显示有关正在执行的操作的信息。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   您可以查看或主动控制会话。 如果选择主动控制用户的会话，您能够输入的键盘和鼠标操作对该会话。
-   您始终可以远程控制您自己的会话 （除当前的会话），但你必须具有完全控制权限或远程控制另一个会话的远程控制特殊访问权限。
-   此外可以通过使用远程桌面服务管理器中启动远程控制。
-   开始监视之前，服务器会发出警告的用户的会话将被远程控制，除非禁用此警告。 你的会话可能会显示为冻结状态几秒钟等待来自用户的响应时。 若要配置远程控制用户和会话，请使用远程桌面服务配置工具或远程桌面服务扩展到本地用户和组以及 active directory 用户和计算机。
-   你的会话必须能够支持使用在您要远程控制，否则操作将失败的会话的视频分辨率。
-   控制台会话可以既不远程控制另一个会话，也可以远程控制它由另一个会话。
-   当你想要结束远程控制 （隐藏） 时，请按 CTRL + * (通过使用\*仅数字小键盘上)。

## <a name="BKMK_examples"></a>示例
-   若要隐藏会话 93，类型：
    ```
    shadow 93
    ```
-   若要隐藏会话 ACCTG01，键入：
    ```
    shadow ACCTG01
    ```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[远程桌面服务 & #40;终端服务和 #41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
