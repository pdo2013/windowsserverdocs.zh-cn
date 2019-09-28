---
title: netstat
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 54bd21b7e96275d329e45e825971d9236488c793
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373262"
---
# <a name="netstat"></a>netstat

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示处于活动状态的 TCP 连接、计算机正在侦听的端口、以太网统计信息、IP 路由表、IPv4 统计信息（适用于 IP、ICMP、TCP 和 UDP 协议）以及 IPv6 统计信息（适用于 IPv6、ICMPv6、TCP over IPv6 和 UDP over IPv6 协议）。 在不使用参数的情况下， **netstat**显示活动 TCP 连接。 

## <a name="syntax"></a>语法
```
netstat [-a] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<Interval>]
```

### <a name="parameters"></a>Parameters

|   参数   |                                                                                                                                              描述                                                                                                                                              |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      -a       |                                                                                                   显示所有活动 TCP 连接以及计算机正在侦听的 TCP 和 UDP 端口。                                                                                                   |
|      -e       |                                                                                 显示以太网统计信息，如发送和接收的字节数和数据包数。 此参数可以与 **-s**组合。                                                                                  |
|      -n       |                                                                               显示活动 TCP 连接，但地址和端口号用数字表示，而不会尝试确定名称。                                                                               |
|      -o       |                          显示活动 TCP 连接，并包含每个连接的进程 ID （PID）。 您可以根据 Windows 任务管理器中 "进程" 选项卡上的 PID 查找该应用程序。 此参数可以与 **-a**、 **-n**和 **-p**结合使用。                           |
| -p <Protocol> |               显示*协议*指定的协议的连接。 在这种情况下，该*协议*可以是 tcp、udp、tcpv6 或 udpv6。 如果将此参数与 **-s**一起使用以按协议显示统计信息，则*协议*可以是 tcp、udp、icmp、ip、tcpv6、udpv6、icmpv6 或 ipv6。                |
|      -s       | 按协议显示统计信息。 默认情况下，会显示 TCP、UDP、ICMP 和 IP 协议的统计信息。 如果安装了 IPv6 协议，则会显示 TCP over IPv6、基于 IPv6 的 UDP、ICMPv6 和 IPv6 协议的统计信息。 **-P**参数可用于指定一组协议。 |
|      -r       |                                                                                                     显示 IP 路由表的内容。 这等效于路由打印命令。                                                                                                     |
|  <Interval>   |                                                        每隔*间隔*秒重新计算选定的信息。 按 CTRL + C 停止重新显示。 如果省略此参数，则**netstat**仅打印选定的信息一次。                                                         |
|      /?       |                                                                                                                                 在命令提示符下显示帮助。                                                                                                                                  |

## <a name="remarks"></a>备注
-   与此命令一起使用的参数必须以连字符（ **-** ）作为前缀，而不是斜线（ **/** ）。
-   **netstat**提供以下各项的统计信息：
    -   Proto 协议的名称（TCP 或 UDP）。
    -   本地地址本地计算机的 IP 地址和所使用的端口号。 除非指定了 **-n**参数，否则显示与 IP 地址和端口名称对应的本地计算机的名称。 如果尚未建立端口，则端口号显示为星号（*）。
    -   外部地址套接字连接到的远程计算机的 IP 地址和端口号。 除非指定了 **-n**参数，否则将显示与 IP 地址和端口对应的名称。 如果尚未建立端口，则端口号显示为星号（*）。
    -   状态指示 TCP 连接的状态。 可能的状态如下所示：CLOSE_WAIT 已 FIN_WAIT_1 FIN_WAIT_2 LAST_ACK 侦听 SYN_RECEIVED SYN_SEND timeD_WAIT 有关 TCP 连接状态的详细信息，请参阅 Rfc 793。
-   仅当 Internet 协议（TCP/IP）协议安装为网络连接中的网络适配器属性中的组件时，此命令才可用。

## <a name="BKMK_Examples"></a>示例
若要显示以太网统计信息和所有协议的统计信息，请键入：
```
netstat -e -s
```
若要仅显示 TCP 和 UDP 协议的统计信息，请键入：
```
netstat -s -p tcp udp
```
若要每隔5秒显示一次活动 TCP 连接和进程 Id，请键入：
```
netstat -o 5
```
若要使用数字形式显示活动 TCP 连接和进程 Id，请键入：
```
netstat -n -o
```

## <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)
