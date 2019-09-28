---
title: tracert
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f08fd3276f3377fed06d7b9a2cc3399fa1071f39
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385654"
---
# <a name="tracert"></a>tracert

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

通过将 Internet 控制消息协议（ICMP）回送请求或 ICMPv6 消息发送到目标，以递增递增的生存时间（TTL）字段值，确定目标的路径。 显示的路径为源主机与目标之间路径中的路由器的 near/端路由器接口列表。 近/侧接口是最接近路径中发送主机的路由器接口。 在没有参数的情况下使用，tracert 显示帮助。   

## <a name="syntax"></a>语法  
```  
tracert [/d] [/h <MaximumHops>] [/j <Hostlist>] [/w <timeout>] [/R] [/S <Srcaddr>] [/4][/6] <TargetName>  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|/d|防止**tracert**尝试将中间路由器的 IP 地址解析为其名称。 这可以加速显示**tracert**结果。|  
|/h \<MaximumHops >|指定路径中要搜索目标的最大跃点数（目标）。 默认值为30个跃点。|  
|/j \<Hostlist >|指定回显请求消息使用 IP 标头中的 "松散源路由" 选项，其中包含*Hostlist*中指定的中间目标集。 使用松散源路由时，可通过一个或多个路由器分隔连续的中间目标。 主机列表中的最大地址或名称数为9。 *Hostlist*是一系列由空格分隔的 IP 地址（采用点分十进制表示法）。 仅在跟踪 IPv4 地址时使用此参数。|  
|/w \<timeout >|指定等待超过 ICMP 时间的时间量，或与要接收的给定回响请求消息相对应的回显回复消息的时间（以毫秒为单位）。 如果在超时时间内未收到，则显示星号（*）。 默认超时值为4000（4秒）。|  
|/R|指定使用目标作为中间目标并测试反向路由，使用 IPv6 路由扩展标头向本地主机发送回显请求消息。|  
|/S \<Srcaddr >|指定要在回响请求消息中使用的源地址。 仅在跟踪 IPv6 地址时使用此参数。|  
|/4|指定 tracert 只能对此跟踪使用 IPv4。|  
|/6|指定 tracert 只能对此跟踪使用 IPv6。|  
|\<TargetName >|指定目标，由 IP 地址或主机名标识。|  
|/?|在命令提示符下显示帮助。|  

## <a name="remarks"></a>备注  
-   此诊断工具通过将具有不同的生存时间（TTL）值的 ICMP 回显请求消息发送到目标来确定要到达目标的路径。 需要沿着路径的每个路由器将 IP 数据包中的 TTL 递减至少1，然后再将其转发。 TTL 实际上是最大链接计数器。 当数据包上的 TTL 达到0时，路由器应将 "ICMP 超时" 消息返回到源计算机。 tracert 通过发送第一个带有 TTL 为1的回显请求消息来确定路径，并在每次后续传输时将 TTL 递增1，直到目标响应或达到最大跃点数。 默认情况下，最大跃点数为30，可以使用 **/h**参数指定。 该路径是通过查看中间路由器返回的 "ICMP 超时" 消息和目标返回的回送答复消息确定的。 但是，某些路由器不会为 TTL 值过期的数据包返回超出时间的消息，并将其 invisile 到 tracert 命令。 在这种情况下，将为该跃点显示一个星号（*）行。  
-   若要跟踪路径并为每个路由器和路径中的链接提供网络延迟和数据包丢失，请使用**pathping**命令。  
-   仅当 Internet 协议（TCP/IP）协议安装为网络连接中的网络适配器属性中的组件时，此命令才可用。  

## <a name="BKMK_Examples"></a>示例  
若要跟踪名为 corp7.microsoft.com 的主机的路径，请键入：  
```  
tracert corp7.microsoft.com  
```  
若要跟踪名为 corp7.microsoft.com 的主机的路径，并防止将每个 IP 地址解析为其名称，请键入：  
```  
tracert /d corp7.microsoft.com  
```  
若要跟踪名为 corp7.microsoft.com 的主机的路径并使用松散源路由 10.12.0.1/10.29.3.1/10.1.44.1，请键入：  
```  
tracert /j 10.12.0.1 10.29.3.1 10.1.44.1 corp7.microsoft.com  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  
