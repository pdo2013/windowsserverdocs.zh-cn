---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Windows 2016 精确的时间。
description: ''
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 3/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: f4b61dbf07fbc21820dd7b9326bbc990e4db3602
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/05/2018
---
# <a name="windows-server-2016-accurate-time"></a>Windows Server 2016 精确的时间

>适用于：Windows Server 2016

在 Windows Server 2016 的时间同步准确性同时保持完整后反汇编 NTP 兼容性较旧的 Windows 版本与已已经提供了可观，得到改进。 可以在合理的操作条件下维护 1 毫秒 UTC 或 Windows Server 2016 和 Windows 10 周年更新域成员更好的准确性。

Windows 时间 service 是一个组件的客户端和服务器时间同步提供程序使用插件模型。  在 Windows 上，有两个内置客户提供程序，并且没有可用的第三方插件。 使用一个提供[NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305)或[MS NTP](https://msdn.microsoft.com/en-us/library/cc246877.aspx)同步到 NTP 和/或 MS NTP 兼容参考服务器系统本地时间。 其他提供商 Hyper-V 并同步到 Hyper-V 主机虚拟机 (VM)。  当存在多个提供商时，Windows 将选取最佳的提供商，首先，使用层次级别跟根延迟，根散布和最后的时间偏移。

>[!NOTE]
>有关 Windows 时间服务快速概述，看看这样[概括视频](https://aka.ms/WS2016TimeVideo)。

<!-- Not sure what to do with the following -->
在本主题中，我们讨论...这些主题因涉及到使精确的时间： 

- 改进
- 测量
- 最佳做法

>[!IMPORTANT]
> 可以下载通过 Windows 2016 精确的时间文章引用的附录[此处](http://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)。  本文提供了更多有关我们的测试和测量方法的详细信息。



> [!NOTE] 
> Windows 时提供程序插件模型[TechNet 上记录](https://msdn.microsoft.com/en-us/library/windows/desktop/ms725475%28v=vs.85%29.aspx)。
<!-- -->





## <a name="domain-hierarchy"></a>域层次
域和独立配置的工作方式不同。

- 域成员使用安全 NTP 协议，使用身份验证来确保的安全性和时间引用的真实性。  与主时钟取决于域层次和评分系统同步域成员。  在某个域，没有时间 stratums，，从而使得每个 DC 指向更精确的时间层次与父 DC 分层层。  层次解析为 PDC 或根林中 DC 或直流与 GTIMESERV 域标志，这表示良好的时间服务器的域。  请参阅[指定本地可靠时间服务使用 GTIMESERV](#GTIMESERV)以下部分。

- 独立计算机配置为使用 time.windows.com 默认情况。  您应指向 Microsoft 拥有资源的 isp 解决此名称。  如所有远程所在位置的时间引用，网络中断可能会阻止同步。  网络交通加载并对称网络路径可能会降低同步时间的准确性。  对于 1 毫秒准确性，不能取决远程时间来源。

Hyper-V 来宾将具有至少两个 Windows 时提供程序从中进行选择，因为主机时间和 NTP，你可能会看到域或独立不同的行为时运行的访客。

> [!NOTE] 
> 有关域层次和评分系统的详细信息，请参阅["近来可 Windows 时间服务？"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) 博客文章。

> [!NOTE]
> 层次是 NTP 和 Hyper-V 提供程序中使用的概念，其值表示中分层时钟位置。  层次 1 保留的最高级别时钟和层次 0 保留假定可准确的硬件，并且拥有一会儿或与之关联的任何延迟。  层次 2 交谈于层次 1 服务器，层次 2 到层次 3，依此类推。  尽管低层次通常指示更准确时钟，就可以查找差异。  此外，W32time 仅接受层次 15 从或下方的时间。  若要查看的客户端层次，使用*w32tm /query /status*。

## <a name="critical-factors-for-accurate-time"></a>重要因素精确的时间
精确的时间为每种情况，有三个关键因素：

1. **稳定源时钟**-源时钟域需要稳定且精确中的。 这通常意味着安装 GPS 设备或指向层次 1 源，考虑 # 3 平板电脑。 转，如果你有两个船上水，并且你正在尝试测量比另一个海拔的相似之处，您的精确最好源船是否非常稳定而不是移动。 也是如此时间，而源时钟不稳定，然后同步时钟整个链是影响，放大在每个阶段。 它还必须可访问因为连接中断会干扰时间同步。 并且最后，必须安全。 如果保留参考不是正确的时间，或由具有潜在危害的方操作，你可能公开你域基于时间的攻击。
2. **稳定客户端时钟**-稳定客户端时钟确保振荡器自然偏移是 containable。  NTP 使用潜在的多个 NTP 服务器从多个示例来条件，并训练本地计算机时钟。  它不步骤时间更改，但而是会延长或加快之间 NTP 请求，你的达到准确快速时间本地时钟和准确保持。  但是，如果客户端计算机时钟振荡器不稳定，然后调整之间的更多波动可能，Windows 用于条件时钟算法准确不起作用。  在某些情况下，固件更新可能需要针对精确的时间。
3. **对称 NTP 通信**-很重要 NTP 通信的连接是对称。  NTP 用于计算调整假定对称网络修补程序的时间。  如果路径 NTP 数据包采用经过服务器采用不同的一段时间返回时，会影响准确性。  例如，由于网络拓扑或通过设备的具有不同的界面速度路由数据包更改还可以更改路径。


电池供电的设备上，移动设备和便携，必须考虑不同的策略。  根据我们的推荐保留精确的时间需要时钟，以将严格一次一下，其中对应于时钟更新频率。 这些设置将占用更多大于预期运行了并且可能会影响使用节电模式可在 Windows 中为此类设备的电池电量。 电池供电的设备也有停止程序运行，干扰 W32time 的能力训练时钟和维护精确的时间的所有应用程序的某些电源模式。 此外，在移动设备的时钟可能不太准确首先。  环境环境状况影响时钟准确性和移动设备可以从一个环境状况移动到下一个可能会影响其功能准确保持时间的。  因此，Microsoft 不建议使用高准确性设置设置电池供电便携设备。 

## <a name="why-is-time-important"></a>为什么很重要的时间？  


## <a name="windows-server-2016-improvements"></a>Windows Server 2016 改进
### <a name="windows-time-service-and-ntp"></a>Windows 时间服务，并 NTP
Windows Server 2016 已改进算法的支持，它用于更正时间和条件本地时钟与 UTC 进行同步。  NTP 使用 4 值来计算的时间偏移，根据的客户端的请求响应和服务器的请求响应时间戳。  但是，网络都恼人，并且可以激增，由于网络拥堵和其他影响延迟网络的因素从 NTP 数据。  Windows 2016 算法平均出使用多种不同的技术这会导致稳定和准确时钟此噪点。  此外，源我们用于精确的时间引用改进 API 这提供给我们更好的解决方法。  了这些改进我们可以实现在 UTC 方面跨域的 1 毫秒准确性。

### <a name="hyper-v"></a>Hyper-V
Windows 2016 已改进 Hyper-V TimeSync 服务。 改进 VM 开始或 VM 还原和中断延迟更正提供给 w32time 的示例包括初始更精确的时间。  此项改进可使我们可以保持与接 10µs 与（根意味着平方，这表明差异）的 RMS 主机的 50µs，即使在使用 75%负载的计算机上。

> [!NOTE]
> 上，请参阅此文章[Hyper-V 体系结构](https://msdn.microsoft.com/library/cc768520.aspx)详细信息。

> [!NOTE]
> 加载已创建使用 prime95 基准使用平衡配置文件。

此外，主机报告给访客层次级别是一目了然。  之前宿主会提供解决的层次 2，无论它准确性。  与 Windows Server 2016 中的更改，主机报告层次比主机层次，进而虚拟来宾生成更好的时间。  主机层次确定 w32time 正常手段根据其源时间。  域加入 Windows 2016 来宾将查找最准确的时钟，而不是默认为主机。  出于此原因，我们建议手动禁用机加入域 Windows 2012R2 文件夹及其子文件夹中的 Hyper-V 时间提供商设置了很。

### <a name="monitoring"></a>监视
添加了监视器的性能计数器。  这些基准，监视器，允许你，并解决相应时间的准确性。  这些计数器包括：

计数器|描述|
----- | ----- |
计算的时间偏移。|   当通过 W32Time 服务计算毫秒中，系统时钟和选的时间源，之间偏移绝对时间。 新的有效示例可用时，使用由示例的时间偏移更新计算的时间。 这是本地时钟的实际的时间偏移。 W32time 启动时钟更正使用此偏移用和更新在样本之间的计算的时间剩余需要到本地时钟应用的时间偏移。 可以使用较低轮询间隔中使用此性能计数器跟踪时钟准确性 (例如：256 秒或更少) 并查找所需的时钟准确性限制小于计数器值。|
时钟频率调整| 由到本地系统时钟 W32Time 中每 10 亿部分绝对时钟频率进行调整。 此计数器帮助直观地显示 W32time 所采取的操作。|
NTP 往返延迟|    在毫秒从服务器接收响应 NTP 客户端遇到的最新往返延迟。 在这种情况经过 NTP 客户端之间传输到 NTP 服务器请求和有效响应受到服务器上。 此计数器有助于描述 NTP 客户端经验丰富的延迟。 放大或不同往返可以添加噪点 NTP 时间计算，进而可能会影响通过 NTP 同步时间的准确性。|
NTP 客户端源计数|    活动时间 NTP 源被 NTP 客户使用数。 这是对的活动、明显 IP 地址的响应此射的时间服务器计数。 放大或缩小比配置等，具体取决于 DNS 分辨率等名称和当前星功能，可能会此数字。|
NTP 服务器传入的请求|   请求 NTP 服务器（请求数秒）接收数。|
NTP 服务器传出响应|  通过 NTP 服务器（响应/秒）回答请求数。|

首次 3 计数器目标方案的准确性问题疑难解答。  故障排除时间的准确性和 NTP 部分下下,[最佳做法](#BestPractices)，有更多详细信息。
上次 3 计数器键盘盖 NTP 服务器方案，是有用时确定负载和基准你当前的性能。

### <a name="configuration-updates-per-environment"></a>每个环境的配置更新
以下说明每个角色默认配置 Windows 2016 之间的以前版本的更改。  Windows Server 2016 和 Windows 10 周年更新（版本 14393），设置现已唯一这正是该处将显示为不同的列。 

|角色|设置|Windows Server 2016|Windows 10 版本 1607|Windows Server 2012R2</br>Windows Server 2008 R2</br>Windows 10|
|---|---|---|---|---|
|**独立/Nano 服务器**||||
| |*时间服务器*|time.windows.com|纳帕|time.windows.com|
| |*轮询频率*|64 1024 秒钟|纳帕|一周|
| |*时钟更新频率*|另一次|纳帕|一次小时|
|**独立客户端**||||
| |*时间服务器*|纳帕|time.windows.com|time.windows.com|
| |*轮询频率*|纳帕|每天|一周|
| |*时钟更新频率*|纳帕|每天|一周|
|**域控制器**||||
| |*时间服务器*|PDC/GTIMESERV|纳帕|PDC/GTIMESERV|
| |*轮询频率*|64-1024 秒钟|纳帕|1024 32768 秒钟|
| |*时钟更新频率*|每天|纳帕|一周|
|**域成员服务器**||||
| |*时间服务器*|DC|纳帕|DC|
| |*轮询频率*|64-1024 秒钟|纳帕|1024 32768 秒钟|
| |*时钟更新频率*|另一次|纳帕|5 分钟一次|
|**域成员客户端**||||
| |*时间服务器*|纳帕|DC|DC|
| |*轮询频率*|纳帕|1204 32768 秒钟|1024 32768 秒钟|
| |*时钟更新频率*|纳帕|5 分钟一次|5 分钟一次|
|**Hyper-V 访客**||||
| |*时间服务器*|根据层次主机和时间服务器的最佳选项中选择|根据层次主机和时间服务器的最佳选项中选择|默认值为主机|
| |*轮询频率*|根据角色上述|根据角色上述|根据角色上述|
| |*时钟更新频率*|根据角色上述|根据角色上述|根据角色上述|

>[!NOTE]
>在 Hyper-V Linux，请参阅[允许 Linux 使用 Hyper-V 主机时](#AllowingLinux)以下部分。

### <a name="impact-of-increased-polling-and-clock-update-frequency"></a>增加的轮询和时钟更新频率的影响
为了提供更精确的时间，这使我们能够更频繁地进行细微调整增加轮询频率和时钟更新的默认值。  这将导致更多 UDP/NTP 通信，不过，这些数据包小以便应有几乎或宽带链接上的不会影响。 好处，但这是不是时间应为更好地上更广泛的硬件和环境。

备份的电池设备增加轮询频率可能会导致问题。  电池设备未存储处于关闭状态时的时间。  继续时，它可能需要对时钟频繁更正。  增加轮询频率将导致时钟变得不稳定，也可能使用更多电量。  Microsoft 建议你不会更改客户端默认设置。

域控制器应该至少影响甚至与广告域中 NTP 客户端从增加更新相乘得到效果。  NTP 具有资源消耗与其他协议和勉强影响相比更小。  你会更容易受到 Windows Server 2016 的增加设置前达到其他域功能的限制。  Active Directory 无法使用安全 NTP，准确比简单 NTP 同步时间的推移，但我们已验证，它可以缩放到离开 PDC 客户端两层次。

作为保守套餐，你应预订每个核心每秒 100 NTP 请求。  例如，具有 4 核心 4 Dc 组成某个域，你应该可以为每秒 1600 NTP 请求提供服务。  如果你有 10 k 客户端到同步时间每秒一次 64，配置，并且随着时间的推移统一收到请求，你会看到 10000/64 或大约 160 请求秒跨所有 Dc。  这属于轻松我们 1600 NTP 秒请求根据此示例中。  这些是保守计划的建议，当然具有较大相关性在你的网络、处理器的速度和加载，以便一如既往基准与测试环境中。

务必还请注意，如果 Dc 运行使用大量的 CPU 加载大于 40%，这将几乎可以将噪点添加到 NTP 响应影响你的域中你的时间的准确性。  同样，你需要测试环境了解实际结果中。

## <a name="time-accuracy-measurements"></a>时间的准确性测量
### <a name="methodology"></a>方法
要测量的时间的准确性为 Windows Server 2016 中，我们使用各种工具的方法，环境。  这些技术用于测量 tune 环境和确定是否准确性结果满足你的要求。 

我们域源时钟包括两个高精确式 NTP 服务器 GPS 硬件。  我们还用于测量，还必须安装不同制造商的高精确式 GPS 硬件单独参考测试机。  对于某些进行测试，你将需要为你的域时钟源除了参考使用准确且可靠时钟源。

我们使用四种不同方法来测量物理和虚拟机的准确性。 多个方法提供独立方法来验证结果。


1. 本地时钟，必须通过 w32tm 衡量我们引用的测试机器具有单独的 GPS 硬件。  
2.  使用 W32tm"stripchart"的客户端测量 NTP ping 从 NTP 服务器
3.  使用 W32tm"stripchart"NTP 服务器测量 NTP ping 来自客户端
4.  测量 Hyper-V 主机到使用时间戳添加计数器 (TSC) 访客的结果。  此计数器分区和系统的时间之间共享这两个分区中。  我们计算主机和客户端时间虚拟机差。  然后，我们使用 TSC 时钟插入来宾操作系统，从主机时，由于测量不发生一次。  此外，我们使用 API 中出延迟和延迟的 TSV 时钟因素。

W32tm 内置，但我们在测试期间我们使用的其他工具有可用于 GitHub 上 Microsoft 知识库作为开源测试和使用情况。  上存储库 WIKI 具有介绍如何使用这些工具执行测量值的详细信息。

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

下方显示的测试结果只是我们在一个测试环境中所做的测量的一部分。  这些说明保留在的开始时间分层和时间分层末尾孩子域客户端的准确性。  这将到 2012 基于拓扑中进行比较相同的计算机进行比较。

### <a name="topology"></a>拓扑
为了进行比较，我们将测试 2012R2 Windows Server 和 Windows Server 2016 基于拓扑。  这两个拓扑包含两个物理 Hyper-V 主计算机参考 GPS 时钟硬件安装与 Windows Server 2016 计算机。  每台主机运行 3 域加入 windows 来宾，根据以下拓扑的排列方式。  行表示时间分层，并使用协议/传输。

![Windows 时间](media/Windows-2016-Accurate-Time/topology1.png)

![Windows 时间](media/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>图形结果概述
以下两个图表示基于上述拓扑某个域中的两个特定成员时间的准确性。  每个图显示 Windows Server 2012R2 和重叠，2016 年结果直观地所示的改进。  准确性已从与接测量来宾计算机到主机进行比较。  图形数据表示的我们所做的测试一整套子集，并显示的最佳情况和糟糕的情形。  

![Windows 时间](media/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>根域 PDC 的性能
根 PDC 同步到（使用 VMIC）Hyper-V 主机它已被证明准确而稳定的 GPS 硬件与 Windows Server 2016。  这是一项重要显示为绿色阴影区域的 1 毫秒准确性要求。

![Windows 时间](media/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>孩子域的客户端性能
为孩子域 PDC 传达给根 PDC 附加孩子域的客户端。  时间很还内 1 毫秒要求。

![Windows 时间](media/Windows-2016-Accurate-Time/chart2.png)


### <a name="long-distance-test"></a>长途测试
以下图表比较 1 虚拟网络跳到 6 物理网络跃与 Windows Server 2016。  两个图表彼此重叠结构的透明度以显示重叠的数据。  增加网络跳跃表示较高延迟，以及我们变得更大时间偏差。  此图表是放大，因此 1 毫秒边界，按绿色的区域，表示是变得更大。  如你所见，时间不仍在 1 毫秒具有多个跃点。  它产生负面移动，其中演示网络不对称。  当然，每个网络不同，并且测量取决于众多与环境因素。

![Windows 时间](media/Windows-2016-Accurate-Time/chart3.png)

## <a name="BestPractices"></a>准确计时的最佳实践
### <a name="solid-source-clock"></a>稳定源时钟
相当于它将与同步源时钟仅限机时间。  为了实现 1 毫秒的准确性，你将需要 GPS 硬件或时间装置你参考作为主源时钟网络上。  使用 time.windows.com 默认，可能无法提供稳定，而本地时间源。  此外，当你收到进一步的源时钟离开，该网络将影响准确性。  有主源时钟在每个数据中心是必需的最高的准确性。

### <a name="hardware-gps-options"></a>硬件 GPS 选项
有各种可以提供精确的时间的硬件解决方案。  一般情况下，解决方案今天基于 GPS 天线。  还有无线电和使用专用的行拨号调制解调器解决方案。  他们附加到为您的网络设备，或者插入电脑，例如通过 PCIe 或 USB 设备的 Windows。  其他选项将提供不同级别的准确性、并为往常一样，结果将取决于您的环境。  这会影响的准确程度变量包括 GPS 可用性、网络的稳定性和负载和电脑的硬件。  这是所有重要因素时选择源时钟，正如我们前面，是一项要求稳定和精确的时间。

### <a name="domain-and-synchronizing-time"></a>域和同步时间
域成员使用域层次确定哪些计算机他们使用作为源同步时间。  每个域成员会发现作为时钟源同步和保存它的另一台计算机。  每种类型的域成员以查找时间同步时钟源遵循规则另一组。  在森林根 PDC 是所有域默认时钟源。  下面列出了不同的角色和高级为他们如何查找源描述：


- **域控制器角色 PDC 与**– 此计算机的是域权威时间源。 它将在域中，有可用的最准确的时间，并且必须与同步的 DC 父域，除在情况下在[GTIMESERV](#GTIMESERV)启用角色。 
- **还有其他域控制器**– 本机将充当时间源客户端和域中的成员服务器。 DC 可以使用其自己的域 PDC 或其家长域中的任何直流同步。
- **客户端/成员服务器**– 该计算机可与任何 DC 或 PDC 其自己的域中，或直流或 PDC 家长域中的同步。

根据可用的替代项，评分系统用于查找的最佳时间来源。  此系统会考虑时间源和其相对位置的可靠性。  这种情况下，一旦时间时不启动的服务。  如果你需要能够更好地控制如何时间进行同步时，你可以在特定的位置添加大好时机服务器或添加冗余。  请参阅[指定本地可靠时间服务使用 GTIMESERV](#GTIMESERV)部分以获取更多信息。

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>混合的 OS 环境（Win2012R2 和 Win2008R2）
最高的准确性为所需的纯净 Windows Server 2016 域环境时，仍有权益混合环境中。  部署 Windows Server 2016 Hyper-V Windows 2012 域中将受益来宾，因为我们如上所述，但只来宾是否还 Windows Server 2016 的改进。  Windows Server 2016 PDC，将无法改进算法它将更稳定的源实现更精确的时间。  替换你 PDC 那样可能没有选项，你可以改为添加 Windows Server 2016 DC 与[GTIMESERV](#GTIMESERV)照片集它可以升级你的域的准确性。  Windows Server 2016 DC 后续时间客户端到提供更好的时间，但是，才相当于其源 NTP 时间。

如上所述，还进行了时钟轮询和刷新频率修改与 Windows Server 2016。  它们可以向低级别 Dc 手动更改或通过组策略应用。  尽管我们尚未进行测试这些配置，他们应也在 Win2008R2 和 Win2012R2 行为，并提供一些好处。

在 Windows Server 2016 有保留精确的时间，这将导致系统时间漫天飞扬进行了调整后立即保留多个问题之前的版本。  出于此原因，经常准确 NTP 来源获取时间示例和调节处理数据的本地时钟会导致在其系统时钟较小偏移在内部取样时间内，向低级别 OS 版本上保留更好的时间导致。 Windows Server 2012R2 NTP 配置的客户端，使用高准确性设置同步准确 Windows 2016 NTP 服务器从其时间时，最佳观察到的准确性是大约 5 毫秒。

在某些方案中涉及来宾域控制器，Hyper-V TimeSync 示例可能会中断域时间同步。  这应该不会再 Server 2016 来宾 Server 2016 Hyper-V 主机上运行的问题。

若要禁用从提供给 w32time 示例 Hyper-V TimeSync 服务，设置以下来宾注册表项：

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider 
    "Enabled"=dword:00000000

#### <a name="AllowingLinux"></a>允许 Linux 使用 Hyper-V 主机时间
在 Hyper-V 运行 Linux 来宾，客户端通常配置 NTP 后台用于针对 NTP 服务器时间同步。  如果 Linux 来宾已启用 TimeSync 集成服务 Linux distribution 支持 TimeSync 版本 4 协议，它将同步针对主机时间。 这可能导致违反时间来保持如果启用了这两种方法。

若要同步专门针对主机时，建议禁用 NTP 时间同步方法之一：

- 禁用 ntp.conf 文件中的任何 NTP 服务器
- 或禁用 NTP 后台程序

在此配置，时间服务器参数是此主机。  它的轮询频率 5 秒并且时钟更新频率还 5 秒。

若要同步独享通过 NTP，建议禁用 TimeSync 集成服务来宾操作系统中。

> [!NOTE]
> 注意：使用 Linux 来宾精确的时间获得支持，需要一项功能，才支持在新上游 Linux 内核，并且它不是广泛提供跨所有 Linux distros 尚未的内容。 请参考[支持 Linux 和 FreeBSD 虚拟机上 Windows Hyper-V](https://technet.microsoft.com/en-us/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows)有关支持分配的更多详细信息。

#### <a name="GTIMESERV"></a>指定使用 GTIMESERV 可靠的本地时间服务
你可以指定一个或多个域控制器准确源时钟作为使用 GTIMESERV，良好的时间服务器标记。  例如，可为 GTIMESERV 标记特定的域控制器配备 GPS 硬件。  这将确保你的域引用时钟基于 GPS 的硬件。

> [!NOTE]
> 可以在中找到有关域标志的详细信息[MS ADTS 协议文档](https://msdn.microsoft.com/library/mt226583.aspx)。

TIMESERV 是另一个相关的域服务标记这表示当前权威计算机是否，它可以更改如果 DC 失去连接。  在这种状态 DC 会返回"未知层次"通过 NTP 查询时。  尝试多次后, DC 将记录系统事件时间服务事件 36。

如果你想要作为 GTIMESERV 配置 DC，这可以配置手动使用以下命令。  在此情况下 DC 作为主时钟使用另一台计算机。  这可能是设备或台专用的计算机。

    w32tm /config /manualpeerlist:”master_clock1,0x8 master_clock2,0x8” /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> 有关详细信息，请参阅[配置 Windows 时间服务](https://technet.microsoft.com/library/cc731191.aspx)

如果 DC 已安装的 GPS 硬件，你需要使用以下步骤来禁用 NTP 客户端和启用 NTP 服务器。

开始通过禁用显示器的 NTP 客户端，并支持使用这些注册表关键更改 NTP 服务器。

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

接下来，重新启动 Windows 时服务

    net stop w32time && net start w32time

最后，你表示此计算机具有可靠的时间源中使用。
   
    w32tm /config /reliable:yes /update

若要查看已正确进行更改，你可以运行以下命令，这会影响如下所示的结果。 

    w32tm /query /configuration

值|预期的设置|
----- | ----- |
AnnounceFlags|  5（本地）|
NtpServer   |（本地）|
Dll 名称 |C:\WINDOWS\SYSTEM32\w32time.DLL（本地）|
启用 |1（本地）|
NtpClient|  （本地）|

    w32tm /query /status /verbose

值|  预期的设置|
----- | ----- |
层次|    1（主要参考-与无线电时钟同步）|
相同引用|    0x4C4F434C (源名称:"本地")|
源| 本地 CMOS 时钟|
阶段偏移|   0.0000000s|
服务器角色|    576（可靠的时间服务）|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>在第三方虚拟平台上的 Windows Server 2016
当 Windows 虚拟化时，默认情况下虚拟机监控程序负责提供的时间。  但已加入域成员需将使用域控制器，以便 Active Directory 才能正常工作的同步。  若要禁用访客和的任何第三方虚拟平台主机之间的任何时间虚拟化最好。

#### <a name="discovering-the-hierarchy"></a>发现层次
由于时间分层到主时钟源链动态在域中，并且协商，你将需要查询以了解它是时间源和到主源时钟链某一特定计算机的状态。  这有助于诊断时间同步问题。

给定你想要排除特定的客户端;第一步是通过使用此 w32tm 命令了解它的时间源。

    w32tm /query /status

受其他因素源中显示的结果。  源指示与之你同步时间在域中。  这是此计算机时间分层的第一步。
下一步是使用上面的源条目，然后使用 /StripChart 参数链中查找下一个时间源。

    w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

也很有用，以下命令列出了每个域控制器，它可以指定的域中找到并打印它可以确定每个合作伙伴的结果。  此命令将包括已手动配置的计算机。
    
    w32tm /monitor /domain:my_domain

使用列表中，你可以跟踪域通过结果，并了解层次以及在每个步骤的时间偏移。  通过找到点位置的时间偏移获取明显更糟糕，你可以确定根的时间不正确。  在此处可以尝试了解该时间为什么通过启用不正确[w32tm 日志记录](#W32Logging)。 

#### <a name="using-group-policy"></a>使用组策略
你可以使用组策略以实现更严格准确性，例如，分配到使用特定 NTP 服务器或在虚拟化时配置控制如何下层操作系统的客户端。  
下面是可能的情况和相关的组策略设置的列表：

**虚拟化域**-才能在 Windows 2012R2 控制虚拟化域控制器，以便使时间同步与他们的域中，而不是与 Hyper-V 主机时，您可以禁用该注册表项。   要 PDC，你不希望禁用条目，如 Hyper-V 主机将提供最稳定时间源。  注册表项需要更改后重新启动 w32time 服务。

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider]
    "Enabled"=dword:00000000

**准确性敏感加载**-可能时间的准确性敏感工作负载，为配置计算机设置 NTP 服务器组并任何相关的时间设置，如轮询和时钟更新频率。  通常由域，但获得更多控制可能目标特定计算机直接指向主时钟。

组策略设置|   新的值|
----- | ----- |
NtpServer|  ClockMasterName 0x8|
MinPollInterval|    6 – 64 秒钟|
MaxPollInterval|    6|
UpdateInterval| 100 – 一次，每秒|
EventLogFlags|  3-所有特殊时间日志记录|

> [!NOTE]
> NtpServer 和 EventLogFlags 设置都位于 System\Widows 时间 Service\Time 提供商、使用配置 Windows NTP 客户端设置。  其他 3 位于下 System\Windows 时间服务使用的全球配置设置。

**远程准确性敏感大量远程**-实例零售和支付信用卡 Industry (PCI) 的 branch 域中的系统，Windows 将使用当前站点信息和直流定位器查找本地 DC，除非手动 NTP 时间配置的源。  此环境需要 1 的第二个使用更快地集中到正确的时间的准确性。  此选项允许 w32time 服务向后移动时钟。  如果这是接受和满足你的要求，你可以创建以下策略。   与任何环境，可确保基准与测试你的网络。 

组策略设置|   新的值|
----- | ----- |
MaxAllowedPhaseOffset|  1，如果多个在第二个、设置时钟到正确的时间。|

MaxAllowedPhaseOffset 设置位于 System\Windows 时间服务使用的全球配置设置。

> [!NOTE]
> 组策略和相关的条目的详细信息，请参阅[Windows 时服务工具](windows-time-service-tools-and-settings.md)和设置 TechNet 上的文章。

#### <a name="azure-and-windows-iaas-considerations"></a>Azure 和 Windows IaaS 注意事项

##### <a name="azure-virtual-machine-active-directory-domain-services"></a>Azure 虚拟机：Active Directory 域服务
如果运行 Active Directory 域服务 Azure 虚拟机的一部分现有本地 Active Directory 森林，然后 TimeSync(VMIC)，应禁用。 这是以允许所有 Dc 物理和虚拟，树林中使用的单个时间同步层次。 参阅最佳做法白皮书["Hyper-V 中的运行域控制器"](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

##### <a name="azure-virtual-machine-domain-joined-machine"></a>Azure 虚拟机：已加入域的计算机
如果承载即域加入现有的 Active Directory 森林到计算机时，虚拟或物理，最佳做法是来宾禁用 TimeSync 并确保 W32Time 配置为与它通过类型配置时间的域控制器同步 = NTP5

##### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Azure 虚拟机：独立工作组计算机
Azure 虚拟机未加入域，也不是域控制器，建议来保持默认时间配置并且具有 VM 同步与主机。

### <a name="windows-application-requiring-accurate-time"></a>Windows 应用程序需要精确的时间
#### <a name="time-stamp-api"></a>时间戳 API
应使用程序需要与相关 UTC，并不段时间，最大的准确性[GetSystemTimePreciseAsFileTime API](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx)。  这可以确保你的应用程序获取系统时，必须通过 Windows 时间服务。

#### <a name="udp-performance"></a>UDP 性能
如果你拥有的应用程序使用 UDP 通信的交易，它的重要最小化延迟，有一些相关的注册表项，可用于配置端口要排除从一系列端口基筛选引擎。  这将提高这两个延迟，并增加您吞吐量。  但是，应经验丰富的管理员限制对注册表进行更改。  此外，此解决方法将排除的防火墙保护端口。  请参阅下方的文章引用的详细信息。

对于 Windows Server 2012 和 Windows Server 2008，你将需要首次安装修补程序。  你可以参考此知识库文章：[数据报丢失时你运行的多路广播的接收器应用在 Windows 8 和 Windows Server 2012](https://support.microsoft.com/en-us/kb/2808584)

#### <a name="update-network-drivers"></a>更新网络驱动程序
有些网络供应商具有提高性能与延迟驱动程序和缓冲 UDP 数据包相关的驱动程序更新。  请联系你的网络供应商，以查看是否有更新有助于 UDP 吞吐量。

### <a name="logging-for-auditing-purposes"></a>为了审核日志记录
符合时间跟踪法规可以手动存档 w32tm 日志、事件日志和性能监视器信息。  以后，归档的信息可用于证明合规性在过去的特定时间。  使用以下因素来指示准确性。


1. 使用计算的时间偏移性能监视器计数器时钟准确性。  这将显示在所需的准确性时钟。
2.  查找"等响应从"w32tm 日志中时钟源。   以下消息的文本是 IP 地址或 VMIC，它描述了时间源和在下一步中的参考时钟一系列验证。
3.  时钟条件状态使用 w32tm 日志以验证"ClockDispl 专业：\*SKEW\*TIME\*"出现了。  这表明该 w32tm 处于活动状态的时间。

#### <a name="event-logging"></a>事件日志记录
若要了解完整的内容，还需要事件日志信息。  通过收集系统事件日志中，并筛选时间服务器上，Microsoft Windows 内核启动、Microsoft 的 Windows-内核-常规，你可能能够发现是否有其他已更改时，例如，第三方的影响。  这些日志可能还需要排除外部干扰。  组策略可能会影响的事件日志写入日志。  有关详细信息，请参阅使用组策略一上方的部分。

#### <a name="W32Logging"></a>W32time 调试日志记录
若要启用 w32tm 审核目的，以下命令可使显示时钟定期更新，表明源时钟日志记录。  重启该服务，以使新日志记录。  

有关详细信息，请参阅[如何打开登录 Windows 时服务调试](https://support.microsoft.com/en-us/kb/816043)。

    w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

#### <a name="performance-monitor"></a>Performance Monitor
Windows Server 2016 Windows 时间服务公开可以用于收集的审核日志记录的性能计数器。  在本地或远程可以将记录。  你可以录音了计算机的时间偏移和往返行程延迟计数器。  
如任何性能计数器，你可以监视器它们远程并创建使用 System Center Operations Manager 警报。  你可，例如，用于警报闹钟你时的时间偏移移动从所需的准确性。  [系统中心管理包](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx)具有的详细信息。

#### <a name="windows-traceability-example"></a>Windows 可跟踪示例
从 w32tm 日志文件将想要验证两条信息。  首先是日志文件目前条件时钟指示。  此证明，你时钟已成为可调通过 Windows 时间服务争议次。

    151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307
    151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41
    151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

要点是该位置可以看到带有 ClockDispln 专业即证明 w32time 前缀消息与系统时钟交互。
 
接下来，你需要争议时间这报告源计算机的当前用作参考时钟之前查找日志中的最后一个报告。  这可能是一个 IP 地址、计算机名称或 VMIC 提供商，这表明它已同步的 Hyper-V 主机。  下面的示例提供 10.197.216.105 IPv4 地址。

    151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

现在，你已验证参考时间链中的第一个系统，你需要调查参考时间源日志文件，然后重复相同步骤操作。  这一直持续访问物理时钟，诸如 GPS 或已知的时间等 NIST 源。  如果参考时钟 GPS 硬件，然后从制造日志还可能要求。

### <a name="network-considerations"></a>网络注意事项
你的网络的对称性 NTP 协议算法具有依赖关系。  为你增加网络跳跃大量，不对称概率增加。  有，很难预测将你的特定环境中看到哪些类型的准确性。 

可以使用 performance Monitor 和 Windows Server 2016 中的新 Windows 时间计数器评估环境准确性并创建基准。 此外，你可以执行疑难解答以确定你的网络上的任何计算机当前偏移。

有两种常规标准准确次通过该网络。  PTP ([精确式时间协议-IEEE 1588](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) 具有网络基础结构要求更严格但通常可以提供子微秒准确性。  NTP ([网络时间协议 – RFC 1305](https://tools.ietf.org/html/rfc1305)) 适用于变得更大的各种网络和环境中，从而使其更易于管理。 

Windows 支持简单 NTP (RFC2030)，默认情况下，为非域连接计算机。  有关计算机已加入域，我们会使用安全 NTP 称为[MS SNTP](https://msdn.microsoft.com/en-us/library/cc246877.aspx)，它利用提供轻经过身份验证 NTP，述 RFC1305 和 RFC5905 管理利用域协商机密。   

域和非域连接的协议要求 UDP 端口 123。  有关 NTP 最佳惯例的详细信息，请参阅[网络时间协议最佳当前做法 IETF 草稿](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00)。

### <a name="reliable-hardware-clock-rtc"></a>可靠硬件时钟 (RTC)
Windows 无法不步骤时间，除非某些界限超过时，但而不是惩罚时钟。  这意味着 w32tm 定期，使用的设置，与另一次与 Windows Server 2016 的默认值时钟更新频率调整时钟的频率。  如果时钟后，它加快速度的频率，并且如果继续操作，它会延长频率。  但是，在那段时间之间时钟频率调整，硬件时钟是在控制。  如果存在固件或时钟硬件出现问题，该计算机上的时间可能会变得不太准确。

这是需要测试的另一个原因和您的环境中的基准。  如果"计算的时间偏移"的性能计数器没有不会在您的目标的准确性稳定的你可能需要验证你的固件处于最新状态。  为另一个测试，如果，你可以看到重复硬件重现相同的问题。

### <a name="troubleshooting-time-accuracy-and-ntp"></a>时间的准确性和 NTP 疑难解答
你可以使用发现层次部分所述以了解不准确时间的来源。  查看的时间偏移中分层时间与从其 NTP 来源最的不同位置, 找到的点。  了解层次之后，你将想要试用，并了解为什么该源特定的时间不会收到精确的时间。  

重点关注分歧时间与系统，你可以使用这些工具以下来收集详细信息，以帮助您确定该问题并找到的解决方法。  下，UpstreamClockSource 参考是使用发现时钟"w32tm /config /status"。


- 系统事件日志
- 启用日志记录使用：w32tm 日志-w32tm /debug /enable /file: C:\Windows\Temp\w32time-test.log//size: 10000000 /entries: 0 300
- w32Time 注册表项 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- 本地网络跟踪
- （来自本地计算机或 UpstreamClockSource）的性能计数器
- W32tm /stripchart /computer: UpstreamClockSource
- PING UpstreamClockSource 了解延迟和大量跃源
- Tracert UpstreamClockSource

问题|    症状|   分辨率|
----- | ----- | ----- |
本地 TSC 时钟不稳定。| 使用计算机物理 – Perfmon 同步时钟稳定时钟，但你仍看到的几个 100us 每个 1-2 分钟。 |   更新固件或验证不同的硬件不显示相同的问题。|
网络延迟|    w32tm stripchart 显示超过 10 ms 的 RoundTripDelay。  中延迟的原因噪点 ½ 一样大的往返行程时，例如沿一个方向只是延迟的变体。</br></br>UpstreamClockSource 是多个跃点，述 PING。  TTL 应该 128 靠近。</br></br>使用 Tracert 在每个跃点找到延迟。    | 查找时间仔细时钟源。  一个的解决方案是在同一段上安装源时钟或手动指向地理位置更接近的源时钟。  对于域方案，添加与 GTimeServ 角色计算机。 |  
无法可靠地访问 NTP 源|    W32tm /stripchart 间歇性返回"超时请求"    |NTP 源不响应|
NTP 源不响应|    Perfmon 计数器检查 NTP 客户端源计数 NTP 服务器传入请求 NTP 服务器传出响应，并确定你基线与你的使用量。|    使用服务器性能计数器，确定加载是否已更改关于你的基准。</br></br>还有网络拥挤问题吗？|
不使用的最准确的时钟域控制器|    拓扑或最近添加的主时间时钟中的更改。|   w32tm /resync /rediscover|
漫天飞扬的客户端时钟| 时间服务事件 36 系统事件日志和/或文本中的描述日志文件:"NTP 客户端时间源计数"计数器前往 0 从 1|疑难解答上游的源代码，并了解它是否运行性能问题。|

### <a name="baselining-time"></a>基准时间
基准很重要，以便你可以首先，了解的性能和准确性您的网络，并比较基线与将来时出现问题。  你将想到基准根 PDC 或与 GTIMESRV 标记的任何计算机。  我们还会建议该你基准 PDC 每森林中。  最后，选取任何关键 Dc 或计算机具有有趣的特征，如距离或高负荷和基准这些。

也很有用基准 Windows Server 2016 vs 2012 R2，但是你仅有 w32tm /stripchart 可用于为一个工具进行比较，因为 Windows Server 2012R2 没有性能计数器。  应选取具有相同的特征，两台计算机或升级了一台计算机和在更新后比较的结果。  Windows 时间测量附录具有如何详细的测量 2016 年和 2012 年之间的详细信息。

使用所有 w32time 性能计数器，至少每周收集的数据。  这将确保你有足够的引用用于各种在一段时间的网络帐户和足够以确信时间的准确性为稳定运行。

### <a name="ntp-server-redundancy"></a>NTP 服务器冗余
对于用于非域连接的计算机或 PDC 手动 NTP 服务器配置，有多个服务器是一种好冗余措施可用性情形。  它还可能会为提供更好的准确性，假设所有源都为准确和稳定。  但是，如果拓扑不也设计，或不稳定时来源，结果准确性可能更糟糕以便时要小心。  可以手动参考服务器 w32time 受支持的时间的限制为 10。 

## <a name="leap-seconds"></a>飞跃秒钟
地球旋转期间而异段时间后，气候和地质事件所致。 通常，变体也有关第二个每隔几年。 只要从原子时间的变化占用太多，更正的一秒钟（向上或向下）插入，称为飞跃秒。 这是方式的差永远不会超过 0.9 秒。 此更正被宣布落后于实际更正的六个月。 在 Windows Server 2016 中之前, Microsoft 时间服务者不知道飞跃秒钟，，但依靠外部时间服务注意事项的此。 Windows Server 2016 的增加的时间精度，Microsoft 从事更合适的解决方案飞跃秒的问题。

## <a name="secure-time-seeding"></a>结籽的时间的安全
在 Server 2016 W32time 包括安全时间结籽的功能。 此功能确定传出 SSL 连接的大致当前时间。  此时间值是用于监视器本地系统时钟和任何毛错误。 你可以阅读更多有关中功能[这篇博客文章](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/)。  具有可靠的时间部署在来源监视也包括监视的时间偏移的计算机，你可以选择不使用安全时间结籽的功能，改为依赖你现有的基础结构。 

您可以禁用这些步骤功能：

1.  特定的计算机上设置为 0 的 UtilizeSSLTimeData 注册表配置值：

    注册添加 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t REG_DWORD /d 0 /f


2.  如果你无法重启计算机立即由于某些原因，你可以通知 W32time 有关配置更新的服务。 这段代码停止时间监视和强制基于时间的数据收集来自 SSL 连接。 

    W32tm.exe /config /update

3.  重启计算机使该设置生效立即，还会导致以便停止收集的任何时间数据从 SSL 连接。  后面部分很小开销并不应性能问题。

4.  若要应用整个域此设置，请将 UtilizeSSLTimeData 值设置为 0 W32time 组策略设置中，并发布该设置。  当由组策略客户端获取该设置时，W32time 服务会收到通知，并且它将停止时间监视和实施使用 SSL 的时间数据。 在每个计算机重新启动后，将停止 SSL 的时间数据收集。 你域有便携轻盈笔记本电脑/平板电脑和其他设备，如果你可能希望此类机排除此策略更改。 这些设备将最终面临电池电量消耗，需要安全时间结籽功能用于引导的时间。














 













 




