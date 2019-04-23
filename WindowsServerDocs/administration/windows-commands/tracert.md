---
title: tracert
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9032a032-2e5e-49d4-9e86-f821600e4ba6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a97c7656e646a22892eee5caa13d0163d05293d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837178"
---
# <a name="tracert"></a>tracert

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

确定发送 Internet 控制消息协议 (ICMP) 回送请求或 ICMPv6 消息具有以增量方式增加时间 (TTL) 字段值的目的地，到目标所采用的路径。 显示的路径是端附近/路由器接口的源主机和目标之间的路径中的路由器的列表。 附近/端接口是路由器的在路径中的发送主机最接近的接口。 Tracert 使用不带参数，将显示的帮助。   

## <a name="syntax"></a>语法  
```  
tracert [/d] [/h <MaximumHops>] [/j <Hostlist>] [/w <timeout>] [/R] [/S <Srcaddr>] [/4][/6] <TargetName>  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|/d|可防止**tracert**尝试解析中间路由器的 IP 地址为它们的名称。 这可以加快显示**tracert**结果。|  
|/h \<MaximumHops>|指定要搜索的目标 （目标） 的路径中的最大跃点数。 默认值为 30 个跃点。|  
|/j \<Hostlist>|指定的回显请求消息中指定的中间目标组的 IP 标头中使用松散源路由选项*Hostlist*。 松散源路由中，可以由一个或多个路由器分隔连续的中间目标。 地址或主机列表中的名称的最大数目为 9。 *Hostlist*是用空格分隔的一系列的 IP 地址 （采用点分十进制表示法）。 使用此参数仅在跟踪过程中 IPv4 地址。|  
|/w \<timeout>|指定的时间 （毫秒） 等待 ICMP 时间超出或回送答复消息对应于给定回送请求消息被接收。 如果未收到超时时间内，将显示一个星号 （*）。 默认超时值为 4000 （4 秒）。|  
|/R|指定用于将回送请求消息发送到本地主机作为中间目标使用目标和测试的反向路由的 IPv6 路由扩展标头。|  
|/S \<Srcaddr>|指定要使用在回送请求消息的源地址。 使用此参数仅在跟踪过程中 IPv6 地址。|  
|/4|指定该 tracert.exe 可以使用此跟踪的仅 IPv4。|  
|/6|指定该 tracert.exe 可用于此跟踪仅 IPv6。|  
|\<TargetName>|指定的目标，标识通过 IP 地址或主机名。|  
|/?|在命令提示符下显示帮助。|  

## <a name="remarks"></a>备注  
-   该诊断工具确定向其目标的生存期 (TTL) 值发送 ICMP 回显请求消息，使用不同的时间，到目标所采用的路径。 在路径上的每个路由器被要求在 IP 数据包中的 TTL 递减至少 1 转发数据包之前。 实际上，TTL 为最大链接计数器。 当数据包上的 TTL 达到 0 时，路由器应返回到源计算机的 ICMP 时间超出消息。 tracert 通过发送第一个回送请求消息 ttl 设置为 1 和递增 1 上每个后续传输直到目标做出响应或最大跃点数 TTL 达到确定的路径。 最大跃点数是 30，默认情况下，可以使用指定 **/h**参数。 通过检查返回的中间路由器和回送答复消息返回目标的 ICMP 时间超出消息确定的路径。 但是，某些路由器不会返回过期 TTL 值的数据包的时间超过消息并且 invisile tracert 命令。 在这种情况下，为该跃点显示星号 （*） 的行。  
-   若要跟踪路径，并提供网络延迟和数据包丢失，每个路由器和链路的路径中，使用**pathping**命令。  
-   此命令是作为一个组件中的网络连接的网络适配器的属性在安装了 Internet 协议 (TCP/IP) 协议的情况下才可用。  

## <a name="BKMK_Examples"></a>示例  
若要跟踪的主机名为 corp7.microsoft.com 的路径，请键入：  
```  
tracert corp7.microsoft.com  
```  
若要跟踪到名为 corp7.microsoft.com 的主机路径，并防止每个 IP 地址的解析为其名称，键入：  
```  
tracert /d corp7.microsoft.com  
```  
若要跟踪的主机名为 corp7.microsoft.com 的路径，并使用松散源路由 10.12.0.1/10.29.3.1/10.1.44.1，请键入：  
```  
tracert /j 10.12.0.1 10.29.3.1 10.1.44.1 corp7.microsoft.com  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  
