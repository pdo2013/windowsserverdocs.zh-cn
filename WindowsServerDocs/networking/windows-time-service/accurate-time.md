---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Windows Server 2016 的准确时间
description: Windows Server 2016 中的时间同步准确性已显著提高，同时保持与早期 Windows 版本的完全向后 NTP 兼容性。
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: acc46dacdf1d8b11aaa45b3665e8d11ee75635cf
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871831"
---
# <a name="accurate-time-for-windows-server-2016"></a>Windows Server 2016 的准确时间

>适用于：Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows 10 或更高版本

Windows 时间服务是一个组件，它使用客户端和服务器时间同步提供程序的插件模型。  Windows 上有两个内置的客户端提供程序，并且有第三方插件可用。 一种提供程序使用[ntp （RFC 1305）](https://tools.ietf.org/html/rfc1305)或[ms ntp](https://msdn.microsoft.com/library/cc246877.aspx)来将本地系统时间同步到 ntp 和/或与 ntp 兼容的引用服务器。 其他提供程序用于 Hyper-v，并将虚拟机（VM）同步到 Hyper-v 主机。  存在多个提供程序时，Windows 将首先使用层次级别选择最佳的提供程序，后跟根延迟、根分散和最后时间偏移。

> [!NOTE]
> 有关 Windows 时间服务的快速概述，请查看此[高级概述视频](https://aka.ms/WS2016TimeVideo)。

在本主题中，我们将讨论 .。。这些主题与启用准确的时间相关： 

- 措施
- 度量
- 最佳方案

> [!IMPORTANT]
> 可在[此处](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)下载 Windows 2016 准确时间文章引用的附录。  本文档提供了有关测试和度量方法的更多详细信息。

> [!NOTE] 
> [TechNet 上记录](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx)了 windows 时间提供程序插件模型。

## <a name="domain-hierarchy"></a>域层次结构
域和独立配置的工作方式不同。

- 域成员使用安全 NTP 协议，该协议使用身份验证来确保时间引用的安全性和真实性。  域成员与由域层次结构和评分系统确定的主时钟同步。  在域中，有一层时间 stratums，因此每个 DC 指向具有更准确时间层次的父 DC。  层次结构解析为根林中的 PDC 或 DC，或具有 GTIMESERV 域标志的 DC，后者表示域的良好时间服务器。  请参阅下面的[使用 GTIMESERV 指定本地可靠时间服务](#GTIMESERV)部分。

- 默认情况下，独立计算机配置为使用 time.windows.com。  此名称由 DNS 服务器解析，它应指向 Microsoft 拥有的资源。  与所有远程定位的时间引用一样，网络中断都可能阻止同步。  网络流量负载和非对称网络路径可能降低时间同步的准确性。  对于 1 ms 准确性，不能依赖于远程时间源。

由于 Hyper-v 来宾将至少有两个 Windows 时间提供程序可供选择，因此，主机时间和 NTP 在作为来宾运行时，您可能会看到域或独立的不同行为。

> [!NOTE] 
> 有关域层次结构和评分系统的详细信息，请参阅["什么是 Windows 时间服务？"。](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) 博客文章。

> [!NOTE]
> 层次是 NTP 和 Hyper-v 提供程序中使用的概念，其值指示层次结构中的时钟位置。  层次1是为最高级别时钟预留的，而层次0保留给假定为准确的硬件，并且没有与之相关联的延迟。  层次2与层次1服务器通信，层次3到层次2，等等。  虽然低层次通常指示更准确的时钟，但仍有可能发现差异。  此外，W32time 仅接受层次15或更低的时间。  若要查看客户端的层次，请使用*w32tm/query/status*。

## <a name="critical-factors-for-accurate-time"></a>准确时间的关键因素
在每种情况下，对于准确的时间，有三个关键因素：

1. **固态源时钟**-域中的源时钟必须稳定且准确。 这通常意味着安装 GPS 设备或指向层次1源，从而考虑 #3。 类比是，如果您的水上有两个 boats，并且您尝试测量一个比另一个的海拔高度，则您的准确性最好是源船非常稳定且不移动。 对于时间也是如此，并且如果源时钟不稳定，则同步时钟的整个链会受到影响，并在每个阶段进行放大。 它还必须可访问，因为连接中断会干扰时间同步。 最后，该方法必须是安全的。 如果未正确维护时间引用，或者可能由恶意方操作，则可能会将你的域公开给基于时间的攻击。
2. **稳定的客户端时钟**-稳定的客户端时钟可确保震荡的自然偏移为 containable。  NTP 使用多个示例来从可能多个 NTP 服务器到条件，并对您的本地计算机时钟进行规范。  它不会执行时间更改，而是减缓或加速本地时钟，以便快速接近准确的时间，并在 NTP 请求之间保持准确。  然而，如果客户端计算机时钟的震荡不稳定，则可能会发生调整之间的更多波动，以及 Windows 用于条件时钟的算法无法准确工作。  在某些情况下，可能需要更新固件才能获得准确的时间。
3. **对称 ntp 通信**-与 ntp 通信的连接是对称的，这一点非常重要。  NTP 使用计算来调整假设网络修补程序对称的时间。  如果 NTP 包进入服务器的路径需要不同的时间来返回，则准确性会受到影响。  例如，由于网络拓扑发生更改，或者通过具有不同接口速度的设备路由数据包，该路径可能会更改。

对于移动设备和便携式设备的备有电池的设备，你必须考虑不同的策略。  根据我们的建议，若要保持准确的时间，需要将时钟一秒进行一次规范，与时钟更新频率关联。 这些设置将消耗比预期更多的电池电量，并可能干扰 Windows 中为此类设备提供的省电模式。 备有电池的设备还具有特定的电源模式，这些模式会阻止所有应用程序运行，这会干扰 W32time's 能力，并保持准确的时间。 此外，移动设备中的时钟在开始时可能并不太准确。  环境环境条件会影响时钟准确性，移动设备可以从一个环境条件转变到下一个环境条件，这可能会干扰其时间。  因此，Microsoft 不建议使用高准确性设置设置备有电池的便携设备。 

## <a name="why-is-time-important"></a>为什么时间重要？  
有许多不同的原因需要准确的时间。  Windows 的典型情况是 Kerberos，这要求客户端和服务器之间的准确性为5分钟。  但是，有许多其他方面会受到时间准确性的影响，其中包括：


- 政府法规，如：
    - 美国 FINRA 的 50 ms 准确性
    - 第1个 ms ESMA （MiFID）。
- 加密算法
- 群集/SQL/Exchange 和文档数据库等分布式系统
- Bitcoin 事务的区块链框架
- 分布式日志和威胁分析 
- AD 复制
- PCI （支付卡行业），当前1秒钟准确度



[!INCLUDE [windows-server-2016-improvements](windows-server-2016-improvements.md)]
