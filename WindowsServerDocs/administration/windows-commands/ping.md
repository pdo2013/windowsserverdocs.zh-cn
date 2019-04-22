---
title: ping
description: 使用 ping 验证网络连接。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 49272671-2eec-4fa5-881f-65c24cfbef52
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 1ac02a148061cd6eb8480c67f15e934f5fd57768
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816748"
---
# <a name="ping"></a>ping

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

**Ping**命令验证 IP 级连接到其他 TCP/IP 计算机发送 Internet 控制消息协议 (ICMP) 回送请求消息。 相应回送答复消息显示证书以及往返次数的回执。 ping 是用于解决连接性、 可访问性，以及名称解析的主 TCP/IP 命令。 使用不带参数， **ping**显示帮助。

## <a name="syntax"></a>语法

```
ping [/t] [/a] [/n <Count>] [/l <Size>] [/f] [/I <TTL>] [/v <TOS>] [/r <Count>] [/s <Count>] [{/j <Hostlist> | /k <Hostlist>}] [/w <timeout>] [/R] [/S <Srcaddr>] [/4] [/6] <TargetName>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|/t|指定该 ping 继续发送回送请求消息，直到中断目标。 若要中断和显示的统计信息，请按 CTRL + break。 若要中断和退出**ping**，按 CTRL + C。|
|/a|指定的目标 IP 地址执行反向名称解析。 如果此操作成功，ping 将显示相应的主机名。|
|/n \<Count\>|指定的数回送请求发送的消息。 默认值为 4。|
|/l \<Size\>|指定的长度，以字节为单位，回显中的数据字段的请求发送的消息。 默认值为 32。 最大大小为 65,527。|
|/f|指定的回显请求消息发送时 Do 标志设置为 1 （适用于仅 IPv4） IP 标头中的进行分段。 不能通过路由器连接到目标路径中分段的回显请求消息。 此参数可用于故障排除路径的最大传输单元 (PMTU) 问题。|
|/I \<TTL\>|Echo IP 标头中指定的 TTL 字段的值请求发送的消息。 默认值为主机的默认 TTL 值。 最大值*TTL*为 255。|
|/v \<TOS\>|Echo IP 标头中指定的服务 (TOS) 字段的类型的值请求发送 （适用于仅 IPv4） 的消息。 默认值为 0。 *TOS*指定为从 0 到 255 之间的十进制值。|
|/r \<Count\>|指定的 IP 标头中的记录路由选项用于记录的回显请求消息所采用的路径和对应回送答复消息 （适用于仅 IPv4）。 在路径中的每个跃点记录路由选项中使用的条目。 如果可能，请指定*计数*是等于或大于源和目标之间的跃点数目。 *计数*必须最小为 1 且最大为 9。|
|/s \<Count\>|指定的 IP 标头中的 Internet 时间戳选项用于记录的回显请求消息的到达时间和对应回送答复消息的每个跃点。 *计数*必须是最小为 1，最大为 4。 这是必需的链接-本地目标地址。|
|/j\<主机列表\>|指定的回显请求消息使用松散源路由选项中指定的中间目标组的 IP 标头*Hostlist* （适用于仅 IPv4）。 松散源路由中，可以由一个或多个路由器分隔连续的中间目标。 地址或主机列表中的名称的最大数目为 9。 主机列表是用空格分隔的一系列的 IP 地址 （采用点分十进制表示法）。|
|/k\<主机列表\>|指定的回显请求消息使用严格源路由选项中指定的中间目标组的 IP 标头*Hostlist* （适用于仅 IPv4）。 使用严格来源路由下, 一步的中间目标必须是可直接访问 （它必须是路由器接口上的邻居）。 地址或主机列表中的名称的最大数目为 9。 主机列表是用空格分隔的一系列的 IP 地址 （采用点分十进制表示法）。|
|/w \<timeout\>|指定的时间，以毫秒为单位，等待回送答复消息的对应于给定回送请求消息被接收。 如果在超时时间内未收到回送答复消息，显示"请求已超时"错误消息。 默认超时值为 4000 （4 秒）。|
|/R|指定跟踪往返路径 （仅适用于 IPv6)。|
|/S \<Srcaddr\>|指定要使用 （仅适用于 IPv6) 的源地址。|
|/4|指定 IPv4 用于成功进行 ping 操作。 不需要此参数来标识目标主机使用 IPv4 地址。 它仅用于标识目标主机的名称。|
|/6|指定 IPv6 用于执行 ping 操作。 不需要此参数来标识目标主机具有 IPv6 地址。 它仅用于标识目标主机的名称。|
|\<TargetName\>|指定的主机名或 IP 地址的目标。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   可以使用**ping**若要测试的计算机名称和计算机的 IP 地址。 如果 IP 地址执行 ping 操作成功，但是 ping 的计算机名称不是，可能存在名称解析问题。 在这种情况下，确保可通过使用域名系统 (DNS) 查询，通过本地主机文件，解决或 NetBIOS 名称解析技术指定的计算机名称。
-   此命令是作为一个组件中的网络连接的网络适配器的属性在安装了 Internet 协议 (TCP/IP) 协议的情况下才可用。

## <a name="BKMK_Examples"></a>示例

下面的示例演示**ping**命令输出：

```
C:\>ping example.microsoft.com       
         pinging example.microsoft.com [192.168.239.132] with 32 bytes of data:       
         Reply from 192.168.239.132: bytes=32 time=101ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=100ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124
```

若要成功进行 ping 操作目标 10.0.99.221 和其主机名解析 10.0.99.221，键入：

```
ping /a 10.0.99.221
```

若要成功进行 ping 操作目标 10.0.99.221 具有 10 个回显请求消息，其中每个有 1000 个字节的数据字段，键入：

```
ping /n 10 /l 1000 10.0.99.221
```

若要 10.0.99.221 并记录 4 个跃点的路由，请键入：

```
ping /r 4 10.0.99.221
```

若要 10.0.99.221 并指定 10.12.0.1-10.29.3.1-10.1.44.1 松散源路由，请键入：

```
ping /j 10.12.0.1 10.29.3.1 10.1.44.1 10.0.99.221
```

## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)
