---
title: telnet
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b70a6156-9413-4300-84ce-a34c467e2b4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6439a91d82d6d199666629e333d8130bf65a384b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858558"
---
# <a name="telnet"></a>telnet

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

与运行 telnet 服务器服务的计算机进行通信。 
## <a name="syntax"></a>语法
```
telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/a|尝试自动登录。 /L 相同选项使用除当前已登录用户 s 名称。|
|/e \<EscapeChar>|转义字符用于输入 telnet 客户端提示符。|
|/f \<FileName>|用于客户端日志记录的文件名称。|
|/l\<用户名 >|指定要在远程计算机上使用登录的用户名称。|
|/t {vt100 &#124; vt52 &#124; ansi &#124; vtnt}|指定的终端类型。 支持的终端类型为 vt100、 vt52、 ansi 和 vtnt。|
|\<Host> [\<Port>]|指定的主机名或 IP 地址要连接到远程计算机和 （可选） 要使用的 TCP 端口 （默认为 TCP 端口 23）。|
|/?|在命令提示符下显示帮助。 或者，你可以键入 /h。|

## <a name="remarks"></a>备注
-   在可以运行此命令之前，必须安装 telnet 客户端软件。 有关详细信息，请参阅[安装 telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)。
-   你可以运行不带参数输入 telnet 上下文中，由 telnet 提示符指示 telnet (**Microsoft telnet >**)。 在 telnet 提示符下，可以使用 telnet 命令来管理运行 telnet 客户端的计算机。

## <a name="BKMK_Examples"></a>示例
使用 telnet 连接到在 telnet.microsoft.com 运行 telnet 服务器服务的计算机。
```
telnet telnet.microsoft.com
```
使用 telnet 连接到 TCP 端口 44 上 telnet.microsoft.com 在运行 telnet 服务器服务的计算机并在名为 telnetlog.txt 的本地文件中记录会话活动
```
telnet /f telnetlog.txt telnet.microsoft.com 44
```

## <a name="additional-references"></a>其他参考
-   [安装 telnet](https://technet.microsoft.com/library/cc754293(v=ws.10).aspx)
-   [telnet 技术参考](https://technet.microsoft.com/library/cc754987(v=ws.10).aspx)
-   [命令行语法解答](command-line-syntax-key.md)
