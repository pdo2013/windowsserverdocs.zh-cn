---
title: pathping
description: 了解如何获取网络延迟和丢失信息使用 pathping 命令。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec430125-b1dc-4aad-a7c9-b70f486d9e3c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: db3a0f5cd3c711f7df0a13627969dc7b74b3d605
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837238"
---
# <a name="pathping"></a>pathping

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

提供有关网络延迟和在源和目标之间的中间跃点的网络丢失信息。 **pathping**多个回显请求将消息发送到源和目标之间的每个路由器一段时间内，然后根据计算结果的每个路由器从返回的数据包。 因为**pathping**显示程度的数据包丢失了任何给定的路由器或链接，您可以确定哪些路由器或子网可能会出现网络问题。 

**pathping**执行的等效**tracert**命令通过标识哪些路由器在路径上。 然后指定的时间段内定期将 ping 请求发送到的所有路由器，并计算基于从每个返回数的统计信息。 使用不带参数， **pathping**显示帮助。 

## <a name="syntax"></a>语法
```
pathping [/n] [/h] [/g <Hostlist>] [/p <Period>] [/q <NumQueries> [/w <timeout>] [/i <IPaddress>] [/4 <IPv4>] [/6 <IPv6>][<TargetName>]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/n|可防止**pathping**尝试解析中间路由器的 IP 地址为它们的名称。 这可能会加快速度的显示**pathping**结果。|
|/h \<MaximumHops>|指定要搜索的目标 （目标） 的路径中的最大跃点数。 默认值为 30 个跃点。|
|/g\<主机列表 >|指定的回显请求消息使用松散源路由选项中指定的中间目标组的 IP 标头*Hostlist*。 松散源路由中，可以由一个或多个路由器分隔连续的中间目标。 地址或主机列表中的名称的最大数目为 9。 *Hostlist*是用空格分隔的一系列的 IP 地址 （采用点分十进制表示法）。|
|/p \<Period>|指定两次连续的 ping 之间等待毫秒的数。 默认值为 250 毫秒 （1/4 秒）。|
|/q \<NumQueries>|指定的数回送请求消息发送到路径中每个路由器。 默认值为 100 次查询。|
|/w \<timeout>|指定要为每个答复等待毫秒的数。 默认值为 3000 毫秒 （3 秒）。|
|/i \<IPaddress>|指定的源地址。|
|/4 \<IPv4>|指定该 pathping 仅使用 IPv4。|
|/6 \<IPv6>|指定该 pathping 仅使用 IPv6。|
|\<TargetName>|指定的目标，它既可以是 IP 地址或主机名。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   **pathping**参数是区分大小写。
-   若要避免网络拥塞，应该以足够慢的速度发送 ping。
-   为了尽量减少丢失造成的影响迸发，不要发送 ping 过于频繁。
-   使用时 **/p**参数，ping 将单独发送到每个中间跃点。 正因为如此，是两个 ping 命令发送到同一个跃点之间的间隔*期*乘以的跃点数。
-   使用时 **/w**参数，可以并行发送多个 ping 命令。 因此，在指定的时间量*超时*参数不受中指定的时间量*段*ping 之间等待的参数。
-   此命令是作为一个组件中的网络连接的网络适配器的属性在安装了 Internet 协议 (TCP/IP) 协议的情况下才可用。

## <a name="BKMK_Examples"></a>示例

下面的示例演示**pathping**命令输出：

```
D:\>pathping /n corp1
Tracing route to corp1 [10.54.1.196]
over a maximum of 30 hops:
  0  172.16.87.35
  1  172.16.87.218
  2  192.168.52.1
  3  192.168.80.1
  4  10.54.247.14
  5  10.54.1.196
computing statistics for 125 seconds...
            Source to Here   This Node/Link
Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  address
  0                                           172.16.87.35
                                0/ 100 =  0%   |
  1   41ms     0/ 100 =  0%     0/ 100 =  0%  172.16.87.218
                               13/ 100 = 13%   |
  2   22ms    16/ 100 = 16%     3/ 100 =  3%  192.168.52.1
                                0/ 100 =  0%   |
  3   24ms    13/ 100 = 13%     0/ 100 =  0%  192.168.80.1
                                0/ 100 =  0%   |
  4   21ms    14/ 100 = 14%     1/ 100 =  1%  10.54.247.14
                                0/ 100 =  0%   |
  5   24ms    13/ 100 = 13%     0/ 100 =  0%  10.54.1.196
Trace complete.
```
当**pathping**是运行时，第一个结果列出的路径。 这是显示了使用的相同路径**tracert**命令。 接下来，大约 90 秒内 （时间因跃点计数） 显示忙的消息。 在此期间，收集信息从之前列出的所有路由器和链接它们之间。 在此时间段结束时，会显示测试结果。

在示例报表中更高版本，**此节点/链接**，**丢失/Sent = Pct**并**地址**列显示 172.16.87.218 和 192.168.52.1 之间的链接删除 13数据包的百分比。 路由器跃点 2 和 4 还丢失了数据包发送给它们，但这种丢失不会影响他们将不发送给它们的流量转发的能力。

显示标识为垂直条的链接的丢失率 (**|**) 中**地址**列，指示导致转发的数据包丢失的链接拥塞路径。 显示有关 （由其 IP 地址标识） 的路由器的丢失率指示这些路由器可能超载。

## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)