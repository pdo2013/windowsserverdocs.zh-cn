---
title: shadow
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: cb2fad4b0a553e736755f2dc56e5d88297a1fef5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383958"
---
# <a name="shadow"></a>shadow

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

允许您远程控制远程桌面会话主机（rd 会话主机）服务器上其他用户的活动会话。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法
```
shadow {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|\<SessionName >|指定您要远程控制的会话的名称。|
|\<SessionID >|指定您要远程控制的会话的 ID。 使用**query user**显示会话及其会话 id 的列表。|
|/server： \<ServerName >|指定包含您要远程控制的会话的 rd 会话主机服务器。 默认情况下，使用当前 rd 会话 Host4 服务器。|
|/v|显示要执行的操作的相关信息。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   可以查看或主动控制会话。 如果选择主动控制用户会话，则可以向会话输入键盘和鼠标操作。
-   始终可以远程控制自己的会话（当前会话除外），但必须具有 "完全控制" 权限或 "远程控制" 特殊访问权限才能远程控制另一个会话。
-   你还可以使用远程桌面服务管理器启动远程控制。
-   开始监视之前，服务器会警告用户会话将被远程控制，除非禁用此警告。 会话在等待来自用户的响应时可能会显示为 "已冻结"。 若要为用户和会话配置远程控制，请使用远程桌面服务配置工具或本地用户和组以及 active directory 用户和计算机的远程桌面服务扩展。
-   您的会话必须能够支持在您远程控制的会话中使用的视频分辨率，否则该操作将失败。
-   控制台会话既不能远程控制其他会话，也不能由其他会话远程控制。
-   若要结束远程控制（隐藏），请按 CTRL + \* （仅限数字键盘使用 \*）。

## <a name="BKMK_examples"></a>示例
-   若要隐藏会话93，请键入：
    ```
    shadow 93
    ```
-   若要隐藏会话 ACCTG01，请键入：
    ```
    shadow ACCTG01
    ```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[远程桌面服务&#40;终端服务和&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)
