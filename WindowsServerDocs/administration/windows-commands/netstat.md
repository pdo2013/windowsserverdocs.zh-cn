---
title: netstat
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60e2718f-93cc-4ceb-bf0e-58a6a6e4fc8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 666a15056e75cea37959d821c34288ffc2c6a6c4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825668"
---
# <a name="netstat"></a>netstat

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示活动 TCP 连接，端口的计算机是侦听、 以太网统计信息、 IP 路由表、 IPv4 统计信息 （IP、 ICMP、 TCP 和 UDP 协议中） 和 IPv6 统计信息 （对于 IPv6，ICMPv6，通过 IPv6 的 TCP 和 UDP 通过 IPv6 协议）。 使用不带参数， **netstat**显示活动 TCP 连接。 

## <a name="syntax"></a>语法
```
netstat [-a] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<Interval>]
```

### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|-a|显示所有活动 TCP 连接和计算机正在侦听的 TCP 和 UDP 端口。|
|-e|显示以太网统计信息，例如个字节，并发送和接收的数据包的数目。 此参数可以结合 **-s**。|
|-n|显示活动 TCP 连接，但是，地址和端口号以数字形式表现并不会尝试确定名称。|
|-o|显示活动 TCP 连接，并包括为每个连接的进程 ID (PID)。 您可以找到应用程序基于 Windows 任务管理器中的进程选项卡上的 PID。 此参数可以结合 **-a**， **-n**，并 **-p**。|
|-p <Protocol>|显示指定的协议的连接*协议*。 在这种情况下，*协议*可以是 tcp、 udp、 tcpv6 或 udpv6。 如果此参数用于 **-s**以显示统计信息的协议，通过*协议*可以是 tcp、 udp、 icmp、 ip、 tcpv6、 udpv6、 icmpv6 或 ipv6。|
|-s|显示协议统计信息。 默认情况下为 TCP、 UDP、 ICMP 和 IP 协议显示统计信息。 如果安装了 IPv6 协议，统计信息会显示为 TCP 通过 IPv6，UDP 对 IPv6、 ICMPv6 和 IPv6 协议。 **-P**参数可用于指定一组的协议。|
|-r|显示 IP 路由表的内容。 这相当于路由打印命令。|
|<Interval>|重新显示所选的信息每隔*间隔*秒。 按 CTRL + C 来停止起。 如果省略此参数，则**netstat**打印一次的所选的信息。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   此命令使用的参数必须以连字符作为前缀 (**-**) 而不是正斜杠 (**/**)。
-   **netstat**提供的以下统计信息：
    -   Proto 协议 （TCP 或 UDP） 的名称。
    -   本地地址的 IP 地址，在本地计算机和正在使用的端口号。 除非显示对应的 IP 地址的本地计算机的名称和端口的名称 **-n**指定参数。 如果尚未建立该端口，端口号显示为星号 （*）。
    -   外部地址的 IP 地址和端口号的套接字连接到远程计算机。 就会显示对应的 IP 地址和端口的名称 **-n**指定参数。 如果尚未建立该端口，端口号显示为星号 （*）。
    -   （状态）指示 TCP 连接的状态。 可能状态如下所示：CLOSE_WAIT 关闭建立 FIN_WAIT_1 FIN_WAIT_2 LAST_ACK 侦听 SYN_RECEIVED SYN_SEND timeD_WAIT 状态的 TCP 连接，有关详细信息，请参阅 Rfc 793。
-   此命令是作为一个组件中的网络连接的网络适配器的属性在安装了 Internet 协议 (TCP/IP) 协议的情况下才可用。

## <a name="BKMK_Examples"></a>示例
若要显示的以太网统计信息和所有协议的统计信息，请键入：
```
netstat -e -s
```
若要显示只有 TCP 和 UDP 协议的统计信息，请键入：
```
netstat -s -p tcp udp
```
若要每隔 5 秒显示活动 TCP 连接和进程 Id，请键入：
```
netstat -o 5
```
若要显示活动 TCP 连接和进程 Id 以数字形式键入：
```
netstat -n -o
```

## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)
