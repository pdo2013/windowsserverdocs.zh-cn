---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: 适用于 Windows Server 2016 准确的时间
description: Windows Server 2016 中的时间同步准确性已大幅改进了，同时保持完全向后与早期 Windows 版本的 NTP 兼容。
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: b68e6b915d029e53d47c6cffe214ec6e11bba6ea
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812384"
---
# <a name="accurate-time-for-windows-server-2016"></a>适用于 Windows Server 2016 准确的时间

>适用于：Windows Server 2016 中，Windows Server 2012 R2，Windows Server 2012 中，Windows 10 或更高版本

Windows 时间服务是为客户端和服务器时间同步提供程序使用插件模型的组件。  在 Windows 中，有两个内置的客户端提供程序并且没有可用的第三方插件。 一个提供程序使用[NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305)或[MS NTP](https://msdn.microsoft.com/library/cc246877.aspx)同步到 NTP 和/或 MS NTP 符合引用服务器的本地系统时间。 其他提供程序用于 HYPER-V，同步到 HYPER-V 主机的虚拟机 (VM)。  当存在多个提供程序时，Windows 将选择最佳的提供程序，首先，使用层次级别的根延迟，根分布后, 跟和最后的时间偏移量。

> [!NOTE]
> 有关 Windows 时间服务的快速概述，看一看这[高级别概述视频](https://aka.ms/WS2016TimeVideo)。

<!-- Not sure what to do with the following -->
在本主题中，我们讨论了...这些主题与启用准确的时间相关： 

- 改进
- 度量值
- 最佳方案

> [!IMPORTANT]
> 可以下载被 Windows 2016 精确时间项目引用的附录[此处](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)。  本文档提供有关我们的测试和测量方法更多详细信息。

> [!NOTE] 
> Windows 时间提供程序插件模型[TechNet 上所述](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx)。

## <a name="domain-hierarchy"></a>域层次结构
域和独立配置的工作方式。

- 域成员使用安全的 NTP 协议，使用身份验证来确保安全性和时参考的真实性。  域成员与由域层次结构和计分系统主时钟同步。  在域中，没有时间 stratums，每个 DC，由此指向更准确的时间层次的父 DC 的层次结构层。  在层次结构将解析为 PDC 或根林中 DC 或 DC 并且 GTIMESERV 域标志，它表示良好的时间服务器的域。  请参阅[指定本地可靠时间服务使用 GTIMESERV](#GTIMESERV)下面一节。

- 默认情况下，独立计算机都配置为使用 time.windows.com。  该名称解析由 DNS 服务器，这应指向 Microsoft 所拥有的资源。  像所有远程所在的时引用，网络中断，可能会阻止同步。  网络通信负载和非对称的网络路径可能会降低准确性的时间同步。  为 1 ms 准确性，不能依赖于远程时间源。

HYPER-V 来宾将具有至少两个 Windows 时间提供程序可供选择，因为主机时间和 NTP，您可能会看到不同的行为与域或独立时作为来宾运行。

> [!NOTE] 
> 有关域层次结构和计分系统的详细信息，请参阅["什么是 Windows 时间服务？"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) 博客文章。

> [!NOTE]
> 第是 NTP 和 HYPER-V 提供程序中使用的概念，其值指示层次结构中的时钟位置。  为最高级别的时钟，保留第 1 和第 0 保留供假定为准确的硬件，并且几乎没有什么或与之关联的任何延迟。  第 2 服务器与服务器通信层次 1，第 2 到第 3，依此类推。  而较低层次通常指示更准确的时钟，则可以查找差异。  此外，W32time 仅接受从第 15 或下方的时间。  若要查看客户端的层次，请使用*w32tm /query /status*。

## <a name="critical-factors-for-accurate-time"></a>准确的时间的的关键因素
在精确的时间每种情况下，有三个关键因素：

1. **Solid 源时钟**-在您的域需要稳定且精确的源时钟。 这通常意味着安装 GPS 设备或指向第 1 源，考虑到 #3。 开始，如果您有两个船上使用，并且您尝试进行测量相比到另一个的海拔高度的相似之处，在准确性是最佳如果源船不过是非常稳定且不移动。 也是如此的时间，而源时钟不稳定，然后同步时钟的整个链是受影响，在每个阶段放大。 它还必须是可访问因为在连接中的中断会影响时间同步。 并且，最后，它必须是安全。 如果引用不正确的时间保留，或由潜在的恶意方运营，可能会暴露您的域和基于时间的攻击。
2. **稳定的客户端时钟**-稳定的客户端时钟可确保震荡自然偏差是 containable。  NTP 使用从潜在的多个 NTP 服务器的多个样本情形并训练你本地计算机的时钟。  它不会不单步时间将发生更改，但而不是减缓或加快，以便快速进行准确的时间并保持准确 NTP 请求之间的本地时钟。  但是，如果客户端计算机时钟的震荡不稳定，然后详细波动调整之间可能出现，Windows 使用条件时钟的算法不准确地工作。  在某些情况下，固件更新可能需要为准确的时间。
3. **对称 NTP 通信**-至关重要 NTP 通信的连接是对称。  NTP 使用计算来调整的假定网络修补程序是对称的时间。  如果路径 NTP 数据包采用经过服务器采用不同数量的时间才能返回时，会影响准确性。  例如，路径可能因网络拓扑中或通过设备具有不同的接口速度的路由的数据包中的更改而更改。

对于由电池供电的设备，移动和可移植的您必须考虑不同的策略。  根据我们的建议，保持准确的时间需要的时钟; 如果要将符合标准的一次一秒，这对应于时钟更新频率。 这些设置将消耗更多的电池电量超过预期，并且可能会干扰节能模式可在 Windows 中的此类设备。 由电池供电设备还具有某些电源模式，停止所有应用程序运行，干扰 W32time 的功能训练时钟和维护精确的时间。 此外，在移动设备中的时钟可能不是非常准确开始。  环境的环境条件会影响时钟准确性和移动设备可以从一个环境条件移到下一个可能会影响它能够准确地保留时间的。  因此，Microsoft 不建议使用高精度设置设置由电池供电便携式设备。 

## <a name="why-is-time-important"></a>为何重要的时间？  
有许多不同原因，可能需要准确的时间。  Windows 的典型情况是准确性的 Kerberos，需要 5 分钟的客户端和服务器之间。  但是，有可能受到时间准确性包括的许多其他方面：


- 政府法规所示：
    - 在美国的 FINRA 的 50 毫秒准确性
    - 1 ms ESMA (MiFID II) 在欧盟。
- 加密算法
- 群集/SQL/Exchange 和文档数据库这类分布式的系统
- 用于事务比特币区块链框架
- 分布式的日志和威胁分析 
- AD 复制
- PCI （支付卡行业） 当前 1 的第二个准确性



[!INCLUDE [windows-server-2016-improvements](windows-server-2016-improvements.md)]
