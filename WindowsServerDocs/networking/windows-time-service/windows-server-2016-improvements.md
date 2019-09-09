---
title: Windows Server 2016 改进
description: Windows Server 2016 改进了用于更正时间和条件的算法，该算法用于更正与 UTC 同步的本地时钟。
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 34d05a8058db366714c0ff4fed0b7d80b9150aa4
ms.sourcegitcommit: e2b565ce85a97c0c51f6dfe7041f875a265b35dd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/19/2019
ms.locfileid: "69626292"
---
## <a name="windows-server-2016-improvements"></a>Windows Server 2016 改进

### <a name="windows-time-service-and-ntp"></a>Windows 时间服务和 NTP
Windows Server 2016 改进了用于更正时间和条件的算法，该算法用于更正与 UTC 同步的本地时钟。  NTP 使用4个值根据客户端请求/响应的时间戳和服务器请求/响应来计算时间偏移量。  但是，网络会产生干扰，因为网络拥塞和其他影响网络延迟的因素可能会导致来自 NTP 的数据高峰。  Windows 2016 算法使用多种不同的技术来计算出这种噪音，这种方法会导致稳定的准确时钟。  此外，我们用于精确时间的源引用改进后的 API，使我们更好地解决问题。  通过这些改进，我们可以实现每个域中的 UTC，实现1毫秒的准确性。

### <a name="hyper-v"></a>Hyper-V
Windows 2016 改进了 Hyper-v TimeSync 服务。 改进包括： VM 启动时的初始时间或 VM 还原，以及提供给 w32time 的示例的中断延迟更正。  这项改进使我们可以在具有50μs 的 RMS （根均值方形，表示方差）上保持10μs 的宿主，甚至在负载为 75% 的计算机上继续使用。 有关详细信息，请参阅[hyper-v 体系结构](https://msdn.microsoft.com/library/cc768520.aspx)。

> [!NOTE]
> 已使用平衡配置文件使用 prime95 基准创建负载。

此外，主机向来宾报告的层次级别更透明。  以前，主机会提供2的固定层次，而不考虑其准确性。  随着 Windows Server 2016 中的更改，主机会报告一个层次比主机层次的层次，这将导致虚拟来宾更好的时间。  主机层次通过基于其源时间的标准方法来确定。  已加入域的 Windows 2016 来宾将找出最准确的时钟，而不是默认为主机。  出于此原因，我们建议为在 Windows 2012R2 和更低版本中加入域的计算机手动禁用 Hyper-v 时间提供程序设置。

### <a name="monitoring"></a>监视
已添加性能监视器计数器。  这使你可以对时间准确性进行基准处理、监视和故障排除。  这些计数器包括：

计数器|描述|
----- | ----- |
计算时间偏移|   由 W32Time 服务计算的系统时钟与所选时间源之间的绝对时间偏移（以微秒为单位）。 当新的有效示例可用时，将使用该示例指示的时间偏移量更新计算的时间。 这是本地时钟的实际时间偏移量。 W32time 使用此偏移量启动时钟更正，并使用需要应用于本地时钟的剩余时间偏移量来更新样本之间的计算时间。 可以使用此性能计数器来跟踪时钟准确性，该性能计数器具有较低的轮询间隔（例如：256秒或更少），并查找小于所需时钟准确性限制的计数器值。|
时钟频率调整| 按 W32Time 在部分中每亿个部分对本地系统时钟进行的绝对时钟频率调整。 此计数器有助于直观显示 W32time 正在执行的操作。|
NTP 往返延迟|    NTP 客户端在接收来自服务器的响应时遇到的最新往返延迟（以微秒为单位）。 这是在 NTP 客户端向 NTP 服务器传输请求和从服务器接收有效响应之间经过的时间。 此计数器有助于描述 NTP 客户端遇到的延迟。 更大或不同的往返可能会向 NTP 时间计算添加干扰，这反过来可能会影响通过 NTP 的时间同步准确性。|
NTP 客户端源计数|    NTP 客户端正在使用的 NTP 时间源的活动数目。 这是响应此客户端请求的时间服务器的活动的非重复 IP 地址计数。 此数字可能大于或小于配置的对等方，具体取决于对等名称和当前可访问能力的 DNS 解析。|
NTP 服务器传入的请求|   NTP 服务器收到的请求数（每秒请求数）。|
NTP 服务器传出响应|  NTP 服务器应答的请求数（响应/秒）。|

前3个计数器用于解决准确性问题的目标方案。  [下面的](#BestPractices)故障排除时间准确性和 NTP 部分都具有更多详细信息。
最后3个计数器涵盖了 NTP 服务器方案，在确定负载和基准当前性能时非常有用。

### <a name="configuration-updates-per-environment"></a>每个环境的配置更新
下面介绍了每个角色的 Windows 2016 和早期版本之间的默认配置更改。  Windows Server 2016 和 Windows 10 周年更新（内部版本14393）的设置现在都是唯一的，这就是显示为单独列的原因。 

|Role|设置|Windows Server 2016|Windows 10|Windows Server 2012 R2</br>Windows Server 2008 R2</br>Windows 10|
|---|---|---|---|---|
|**独立/Nano Server**||||
| |*时间服务器*|time.windows.com|不可用|time.windows.com|
| |*轮询频率*|64-1024 秒|不可用|每周一次|
| |*时钟更新频率*|一秒|不可用|每小时一次|
|**独立客户端**||||
| |*时间服务器*|不可用|time.windows.com|time.windows.com|
| |*轮询频率*|不可用|每日一次|每周一次|
| |*时钟更新频率*|不可用|每日一次|每周一次|
|**域控制器**||||
| |*时间服务器*|PDC/GTIMESERV|不可用|PDC/GTIMESERV|
| |*轮询频率*|64-1024 秒|不可用|1024-32768 秒|
| |*时钟更新频率*|每日一次|不可用|每周一次|
|**域成员服务器**||||
| |*时间服务器*|DC|不可用|DC|
| |*轮询频率*|64-1024 秒|不可用|1024-32768 秒|
| |*时钟更新频率*|一秒|不可用|每5分钟一次|
|**域成员客户端**||||
| |*时间服务器*|不可用|DC|DC|
| |*轮询频率*|不可用|1204-32768 秒|1024-32768 秒|
| |*时钟更新频率*|不可用|每5分钟一次|每5分钟一次|
|**Hyper-v 来宾**||||
| |*时间服务器*|基于主机和时间服务器的层次选择最佳选项|基于主机和时间服务器的层次选择最佳选项|默认为主机|
| |*轮询频率*|基于上述角色|基于上述角色|基于上述角色|
| |*时钟更新频率*|基于上述角色|基于上述角色|基于上述角色|

>[!NOTE]
>对于 Hyper-v 中的 Linux，请参阅下面的[允许 linux 使用 Hyper-v 主机时间](#AllowingLinux)部分。

### <a name="impact-of-increased-polling-and-clock-update-frequency"></a>增加的轮询和时钟更新频率的影响
为了提供更准确的时间，将增加轮询频率和时钟更新的默认值，从而使我们能够更频繁地进行小调整。  这将导致 UDP/NTP 流量更大，但这些数据包很小，因此对宽带链路的影响应该非常小或没有影响。 但优点在于，时间应该更好地用于各种硬件和环境。

对于支持电池的设备，增加轮询频率可能会导致问题。  电池设备在关闭时不会存储时间。  当恢复时，可能需要频繁地更正时钟。  增大轮询频率将导致时钟变得不稳定，还可能会占用更多的电量。  Microsoft 建议您不要更改客户端的默认设置。

即使增加了从 AD 域中的 NTP 客户端增加的更新的影响，域控制器也会受到最小程度的影响。  与其他协议相比，NTP 具有更小的资源消耗，并且对边缘的影响很小。  在受到 Windows Server 2016 的增加设置的影响之前，更有可能达到其他域功能的限制。  Active Directory 使用安全 NTP，这往往比简单 NTP 同步时间更少，但我们已验证它将从 PDC 向上扩展到两个层次的客户端。

作为保守计划，应为每个核心保留 100 NTP 请求。  例如，一个域，其中每个包含4个内核的域，每秒可以提供 1600 NTP 请求。  如果你的客户端配置为每隔64秒同步时间一次，并且在一段时间内按时间均匀接收请求，则你将看到 10000/64 或大约160个请求/秒分布在所有 Dc 上。  基于本示例，这在我们的 1600 NTP 请求/秒内非常容易。  这些都是保守的规划建议，当然，在你的网络、处理器速度和负载上都有很大的依赖项，因此在你的环境中始终是基准和测试。

另外，请务必注意，如果 Dc 运行的 CPU 负载相当于 40%，则这几乎肯定会将干扰添加到 NTP 响应并影响域中的时间准确性。  同样，您需要在您的环境中进行测试，以了解实际结果。

## <a name="time-accuracy-measurements"></a>时间准确性度量
### <a name="methodology"></a>体系
为了衡量 Windows Server 2016 的时间准确性，我们使用了各种工具、方法和环境。  你可以使用这些技术来测量和调整环境，并确定准确性结果是否符合你的要求。 

我们的域源时钟包含包含 GPS 硬件的两个高精度 NTP 服务器。  我们还使用了单独的引用测试计算机进行度量，这也是从不同的制造商处安装的高精度 GPS 硬件。  对于某些测试，除了域时钟源外，还需要使用准确且可靠的时钟源作为参考。

我们使用四种不同的方法来衡量物理计算机和虚拟机的准确性。 提供了多个独立方式来验证结果的方法。


1. 针对包含单独 GPS 硬件的引用测试计算机测量由 w32tm 提供的本地时钟。  
2.  使用 W32tm "stripchart" 从 NTP 服务器到客户端度量 NTP ping
3.  使用 W32tm "stripchart" 从客户端向 NTP 服务器测量 NTP ping
4.  使用时间戳计数器（TSC）将主机中的 Hyper-v 结果测量为来宾。  此计数器在两个分区中的分区和系统时间之间共享。  我们计算出了虚拟机中主机时间和客户端时间之间的差异。  然后，使用 TSC 时钟将主机时间从来宾中插值，因为不会同时进行度量。  此外，我们还在 API 中使用 TSV 时钟因子 out 延迟和延迟时间。

W32tm 是内置的，但我们在测试过程中使用的其他工具可用于 GitHub 上的 Microsoft 存储库，作为测试和使用的开源。  存储库上的 WIKI 包含有关如何使用这些工具进行度量的详细信息。

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

下面显示的测试结果是我们在其中一种测试环境中所做的一小部分度量。  它们说明了在时间层次结构开始时维持的准确性，并说明了时间层次结构结束时的子域客户端。  这会与基于2012的拓扑中的相同计算机进行比较，以进行比较。

### <a name="topology"></a>拓扑
为了进行比较，我们测试了基于 Windows Server 2012R2 和 Windows Server 2016 的拓扑。  这两种拓扑都包含两个物理 Hyper-v 主机，它们引用安装了 GPS 时钟硬件的 Windows Server 2016 计算机。  每个主机都运行3个加入域的 windows 来宾，它们按照以下拓扑排列。  行表示时间层次结构，以及使用的协议/传输。

![Windows 时间](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology1.png)

![Windows 时间](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>图形结果概述
以下两个图形基于上述拓扑表示域中两个特定成员的时间准确性。  每个关系图都显示重叠的 Windows Server 2012R2 和2016结果，这会直观显示改进。  在来宾计算机与主机进行比较时，准确性是从中的进行度量。  图形数据表示我们完成的整个测试集的子集，并显示最佳方案和最坏情况。  

![Windows 时间](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>根域 PDC 的性能
根 PDC 将同步到 Hyper-v 主机（使用 VMIC），该主机是一个 Windows Server 2016，其中的 GPS 硬件经过证实可同时保持准确和稳定。  这是 1 ms 准确性的关键要求，显示为绿色阴影区域。

![Windows 时间](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>子域客户端的性能
子域客户端连接到与根 PDC 通信的子域 PDC。  It 时间也在1毫秒的要求内。

![Windows 时间](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart2.png)


### <a name="long-distance-test"></a>远距离测试
下图将1个虚拟网络跃点与 Windows Server 2016 的物理网络跃点进行了比较。  两个图表彼此重叠，具有透明度以显示重叠的数据。  增加网络跃点意味着较高的延迟，并且更大的时间偏差。  图表已放大，因此，由绿色区域表示的1个 ms 边界更大。  正如您所看到的，时间仍在1毫秒内，具有多个跃点。  这会产生负面影响，这表明网络不对称。  当然，每个网络都是不同的，并且度量取决于多种环境因素。

![Windows 时间](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart3.png)

## <a name="BestPractices"></a>准确 timekeeping 的最佳做法
### <a name="solid-source-clock"></a>纯色源时钟
计算机时间只相当于它与之同步的源时钟。  为了实现1毫秒的准确性，你的网络上需要 GPS 硬件或一台时间设备作为主源时钟。  如果使用默认值 time.windows.com，则不能提供稳定的本地时间源。  此外，当你进一步远离源时钟时，网络会影响准确性。  每个数据中心都需要有一个主源时钟才能获得最佳准确性。

### <a name="hardware-gps-options"></a>硬件 GPS 选项
有各种硬件解决方案可以提供准确的时间。  通常，当前的解决方案基于 GPS 天线。  还有使用专用线路的收音机和拨号调制解调器解决方案。  它们以设备的形式附加到你的网络，或插入到电脑中，例如通过 PCIe 或 USB 设备连接 Windows。  不同的选项将提供不同级别的准确性，并且始终会产生不同的结果，具体取决于您的环境。  影响准确性的变量包括 GPS 可用性、网络稳定性和负载以及 PC 硬件。  如果选择源时钟（如我们所述），则必须满足稳定和准确的时间要求。

### <a name="domain-and-synchronizing-time"></a>域和同步时间
域成员使用域层次结构来确定其用作源的计算机，以同步时间。  每个域成员将找到要与其同步的另一台计算机，并将其保存为时钟源。  每种类型的域成员都遵循一组不同的规则，以便查找时间同步的时钟源。  林根中的 PDC 是所有域的默认时钟源。  下面列出了不同的角色以及它们如何查找源的高级说明：


- **域控制器与 PDC 角色**–此计算机是域的权威时间源。 它将具有最准确的域中可用时间，并且必须与父域中的 DC 同步，除非启用了[GTIMESERV](#GTIMESERV)角色。 
- **任何其他域控制器**–此计算机将充当域中的客户端和成员服务器的时间源。 DC 可以与它自己的域或其父域中的任何 DC 的 PDC 同步。
- **客户端/成员服务器**–此计算机可以与父域中其自身域的任何 DC 或 pdc 或父域中的 DC 或 pdc 同步。

根据可用的候选项，计分系统用于查找最佳时间源。  此系统将考虑时间源及其相对位置的可靠性。  当启动服务时，就会发生这种情况。  如果需要更好地控制时间同步的时间，可以在特定位置添加适当的时间服务器或添加冗余。  有关详细信息，请参阅[使用 GTIMESERV 指定本地可靠时间服务](#GTIMESERV)部分。

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>混合 OS 环境（Win2012R2 和 Win2008R2）
尽管需要纯 Windows Server 2016 域环境才能获得最佳准确性，但在混合环境中仍有好处。  由于上述改进，在 Windows 2012 域中部署 Windows Server 2016 Hyper-v 将会受益于来宾，但前提是来宾也是 Windows Server 2016。  Windows Server 2016 PDC 能够提供更准确的时间，因为改进的算法会成为更稳定的源。  因为替换 PDC 可能不是一个选项，所以可以改为使用[GTIMESERV](#GTIMESERV)卷添加 Windows SERVER 2016 DC，这将是你的域的准确性升级。  Windows Server 2016 DC 可为下游时间客户端提供更好的时间，但是，它的源 NTP 时间也非常好。

同样，如上所述，时钟轮询和刷新频率已使用 Windows Server 2016 进行了修改。  可以通过组策略手动更改这些设置，也可以通过组策略进行应用。  虽然我们尚未测试这些配置，但它们应该在 Win2008R2 和 Win2012R2 中的行为良好，并提供一些好处。

Windows Server 2016 之前的版本存在多个问题，请保持准确的时间，这会导致系统时间发生调整后立即偏移。  因此，经常从准确的 NTP 源获取时间样本，并将本地时钟与数据进行调节，使其在采样期间内的系统时钟中出现较小的偏差，从而更好地保持较低的操作系统版本。 当使用高准确性设置配置的 Windows Server 2012R2 NTP 客户端从准确的 Windows 2016 NTP 服务器同步其时间时，最佳观测的准确性大约为5毫秒。

在涉及来宾域控制器的某些情况下，Hyper-v TimeSync 示例可能会中断域时间同步。  对于在服务器 2016 Hyper-v 主机上运行的服务器2016来宾，这应该不再是问题。

若要禁用 Hyper-v TimeSync 服务向 w32time 提供示例，请设置以下来宾注册表项：

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider 
    "Enabled"=dword:00000000

#### <a name="AllowingLinux"></a>允许 Linux 使用 Hyper-v 主机时间
对于在 Hyper-v 中运行的 Linux 来宾，通常会将客户端配置为使用 NTP 守护程序来针对 NTP 服务器进行时间同步。  如果 Linux 分发版支持 TimeSync 版本4协议，而 Linux 来宾已启用 TimeSync integration service，则它将与主机时间同步。 如果同时启用这两种方法，这可能会导致不一致的时间。

若要专门针对主机时间进行同步，建议通过以下方法之一禁用 NTP 时间同步：

- 禁用 ntp 文件中的任何 NTP 服务器
- 或禁用 NTP 守护程序

此配置中的时间服务器参数为此主机。  其轮询频率为5秒，时钟更新频率也是5秒。

若要完全通过 NTP 进行同步，建议在来宾中禁用 TimeSync integration service。

> [!NOTE]
> 注意:使用 Linux 来宾的准确时间支持需要一项功能，该功能仅在最新的上游 Linux 内核中受支持，但并不是所有 Linux 发行版都广泛提供的功能。 有关支持分发的详细信息，请参阅[Windows 上的 Hyper-v 支持的 Linux 和 FreeBSD 虚拟机](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows)。

#### <a name="GTIMESERV"></a>使用 GTIMESERV 指定本地可靠时间服务
可以通过使用 GTIMESERV、良好的时间服务器和标志，将一个或多个域控制器指定为准确的源时钟。  例如，可以将具有 GPS 硬件的特定域控制器标记为 GTIMESERV。  这将确保域根据 GPS 硬件引用时钟。

> [!NOTE]
> 有关域标志的详细信息，请参阅[ADTS 协议文档](https://msdn.microsoft.com/library/mt226583.aspx)。

TIMESERV 是另一个相关的域服务标志，它指示计算机当前是否为权威计算机，如果 DC 失去连接，则可能会更改。  当 DC 通过 NTP 查询时，处于此状态的 DC 将返回 "未知层次"。  尝试多次后，DC 将记录系统事件时间-服务事件36。

如果要将 DC 配置为 GTIMESERV，可以使用以下命令手动配置它。  在这种情况下，DC 使用其他计算机作为主时钟。  这可以是一个设备或专用的计算机。

    w32tm /config /manualpeerlist:”master_clock1,0x8 master_clock2,0x8” /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> 有关详细信息，请参阅[配置 Windows 时间服务](https://technet.microsoft.com/library/cc731191.aspx)

如果 DC 安装了 GPS 硬件，则需要使用这些步骤来禁用 NTP 客户端并启用 NTP 服务器。

首先，禁用 NTP 客户端，并使用这些注册表项更改启用 NTP 服务器。

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

接下来，重新启动 Windows 时间服务

    net stop w32time && net start w32time

最后，你指示此计算机具有使用的可靠时间源。
   
    w32tm /config /reliable:yes /update

若要检查是否已正确完成更改，你可以运行以下命令，这些命令会影响下面所示的结果。 

    w32tm /query /configuration

ReplTest1|预期设置|
----- | ----- |
AnnounceFlags|  5（本地）|
NtpServer   |地方|
DllName |C:\WINDOWS\SYSTEM32\w32time.DLL （本地）|
Enabled |1（本地）|
NtpClient|  地方|

    w32tm /query /status /verbose

ReplTest1|  预期设置|
----- | ----- |
Ntp|    1（主要引用-通过无线电时钟 syncd）|
ReferenceId|    0x4C4F434C （源名称："LOCAL"）|
Source| 本地 CMOS 时钟|
相位偏移量|   0.0000000 s|
服务器角色|    576（可靠时间服务）|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>第三方虚拟平台上的 Windows Server 2016
在虚拟化 Windows 时，默认情况下，虚拟机监控程序负责提供时间。  但是，加入域的成员需要与域控制器 sychronized，以便 Active Directory 正常工作。  最好禁用来宾和任何第三方虚拟平台的主机之间的任何时间虚拟化。

#### <a name="discovering-the-hierarchy"></a>发现层次结构
由于与主时钟源的时间链层次结构在域中是动态的，并且协商，因此你需要查询特定计算机的状态，以了解它的时间源并将其链接到主源时钟。  这可以帮助诊断时间同步问题。

假设您想要对特定客户端进行故障排除，第一步是使用此 w32tm 命令了解其时间源。

    w32tm /query /status

结果会显示其他项目中的源。  源指示您在域中同步时间的用户。  这是此计算机时间层次结构的第一步。
接下来，使用上面的源条目，并使用/StripChart 参数查找链中的下一个时间源。

    w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

此外，以下命令还会列出它可以在指定的域中找到的每个域控制器，并输出一个结果，让你确定每个伙伴。  此命令将包括已手动配置的计算机。
    
    w32tm /monitor /domain:my_domain

使用此列表，可以通过域跟踪结果，并了解层次结构以及每个步骤的时间偏移量。  通过定位时间偏移明显更差的点，可以找出错误时间的根。  此时，你可以通过打开[w32tm 日志记录](#W32Logging)尝试了解此时间不正确的原因。 

#### <a name="using-group-policy"></a>使用组策略
例如，你可以使用组策略来实现更严格的准确性，例如，将客户端分配到使用特定的 NTP 服务器，或控制在虚拟化时如何配置下级操作系统。  
下面是可能的方案和相关组策略设置的列表：

**虚拟化域**-为了控制 Windows 2012R2 中的虚拟化域控制器，使其与域（而不是 hyper-v 主机）同步时间，你可以禁用此注册表项。   对于 PDC，你不想要禁用该条目，因为 Hyper-v 主机将提供最稳定的时间源。  注册表项需要在更改后重新启动 w32time 服务。

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider]
    "Enabled"=dword:00000000

**准确性敏感负载**-对于时间准确性敏感的工作负荷，可以配置计算机组来设置 NTP 服务器和任何相关的时间设置，如轮询和时钟更新频率。  这通常由域进行处理，但为了获得更多控制，你可以针对特定的计算机直接指向主时钟。

组策略设置|   “新值”|
----- | ----- |
NtpServer|  ClockMasterName，0x8|
MinPollInterval|    6–64秒|
MaxPollInterval|    6|
UpdateInterval| 100–每秒一次|
EventLogFlags|  3–所有特殊时间日志记录|

> [!NOTE]
> NtpServer 和 EventLogFlags 设置位于 System\Windows Time Service\Time 提供程序下，使用 "配置 Windows NTP 客户端" 设置。  其他3使用全局配置设置位于 System\Windows 时间服务下。

远程**准确性敏感加载**-对于实例零售和付款信用行业（PCI）的分支域中的系统，Windows 使用当前站点信息和 DC 定位符查找本地 DC，除非已配置手动 NTP 时间源.  此环境需要1秒的准确性，这会使用更快的聚合来实现正确的时间。  此选项允许 w32time 服务向后移动时钟。  如果可以接受并满足你的要求，则可以创建以下策略。   与任何环境一样，请确保对您的网络进行测试和基准测试。 

组策略设置|   “新值”|
----- | ----- |
MaxAllowedPhaseOffset|  1，如果大于，则将时钟设置为正确的时间。|

MaxAllowedPhaseOffset 设置位于使用全局配置设置的 System\Windows 时间服务下。

> [!NOTE]
> 有关组策略和相关条目的详细信息，请参阅 TechNet 上的[Windows 时间服务工具](windows-time-service-tools-and-settings.md)和设置一文。

## <a name="azure-and-windows-iaas-considerations"></a>Azure 和 Windows IaaS 注意事项

### <a name="azure-virtual-machine-active-directory-domain-services"></a>Azure 虚拟机：Active Directory 域服务
如果运行 Active Directory 域服务的 Azure VM 是现有本地 Active Directory 林的一部分，则应禁用 TimeSync （VMIC）。 这将允许林中的所有 Dc （物理和虚拟）使用单个时间同步层次结构。 请参阅最佳做法白皮书["在 hyper-v 中运行域控制器"](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

### <a name="azure-virtual-machine-domain-joined-machine"></a>Azure 虚拟机：已加入域的计算机
如果要托管的计算机是加入现有 Active Directory 林（虚拟或物理）的域，最佳做法是禁用来宾的 TimeSync，并确保 W32Time 配置为与域控制器同步，方法是配置时间Type = NTP5

### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Azure 虚拟机：独立工作组计算机
如果 Azure VM 未加入域，也不是域控制器，则建议保留默认时间配置，并使 VM 与主机同步。

## <a name="windows-application-requiring-accurate-time"></a>需要准确时间的 Windows 应用程序
### <a name="time-stamp-api"></a>时间戳 API
对于需要最准确的 UTC （而不是时间）的程序，应使用[GETSYSTEMTIMEPRECISEASFILETIME API](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx)。  这可确保你的应用程序获取系统时间，这是由 Windows 时间服务进行的。

### <a name="udp-performance"></a>UDP 性能
如果你有一个应用程序，该应用程序使用 UDP 通信来处理事务，并且很重要的是将延迟时间降到最低，则可以使用一些相关的注册表项来配置要从端口上排除的端口，使其从端口成为基本筛选引擎。  这将提高延迟并增加吞吐量。  但是，对注册表所做的更改应限于经验丰富的管理员。  此外，此操作解决了排除防火墙不受防火墙保护的端口的情况。  有关详细信息，请参阅下面的文章参考文章。

对于 Windows Server 2012 和 Windows Server 2008，你将需要先安装修补程序。  可以参考此知识库文章：[在 Windows 8 和 Windows Server 2012 中运行多播接收器应用程序时数据报丢失](https://support.microsoft.com/kb/2808584)

### <a name="update-network-drivers"></a>更新网络驱动程序
某些网络供应商的驱动程序更新会提高驱动程序延迟和缓冲 UDP 数据包的性能。  请与您的网络供应商联系，查看是否有更新来帮助进行 UDP 吞吐量。

## <a name="logging-for-auditing-purposes"></a>用于审核的日志记录
若要遵守时间跟踪法规，可以手动存档 w32tm 日志、事件日志和性能监视器信息。  稍后，已存档的信息可用于证明过去特定时间的符合性。  以下因素用于指示准确性。


1. 使用计算的时间偏移性能监视器计数器的时钟准确性。  这会以所需准确性显示时钟。
2.  在 w32tm 日志中查找 "对等响应" 的时钟源。   消息文本后面是 IP 地址或 VMIC，用于描述要验证的引用时钟链中的时间源和下一个。
3.  使用 w32tm 日志的时钟条件状态来验证 "ClockDispl 学科：\*出现\*扭曲\*时间 "。  这表示在此时，w32tm 处于活动状态。

### <a name="event-logging"></a>事件日志记录
若要获取完整的情景，还需要事件日志信息。  通过收集系统事件日志并筛选时间服务器、Microsoft-内核-启动、Microsoft Windows 内核-常规，你可能会发现是否有其他影响更改了时间（例如，第三方）。  可能需要这些日志来排除外部干扰。  组策略会影响写入日志的事件日志。  有关更多详细信息，请参阅上面有关使用组策略的部分。

### <a name="W32Logging"></a>W32time 调试日志记录
若要启用 w32tm 进行审核，请使用以下命令启用日志记录，以便显示时钟的定期更新并指示源时钟。  重新启动服务以启用新的日志记录。  

有关详细信息，请参阅[如何在 Windows 时间服务中启用调试日志记录](https://support.microsoft.com/kb/816043)。

    w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

### <a name="performance-monitor"></a>性能监视器
Windows Server 2016 Windows 时间服务公开可用于收集审核日志记录的性能计数器。  可以在本地或远程记录这些日志。  可以记录 "计算机时间偏移" 和 "往返行程延迟" 计数器。  
与任何性能计数器一样，你可以远程监视它们并使用 System Center Operations Manager 创建警报。  例如，你可以在时间偏移量偏离的情况下，使用警报向你发出警报。  [System Center 管理包](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx)中提供了详细信息。

### <a name="windows-traceability-example"></a>Windows 可跟踪性示例
从 w32tm 日志文件中，你将需要验证两部分信息。  第一个指示日志文件当前为条件时钟。  这表明，在有争议的时间，Windows 时间服务对您的时钟进行了证实。

    151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307
    151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41
    151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

要点是，你会看到以 ClockDispln 规范为前缀的消息，证明 w32time 正在与系统时钟交互。
 
接下来，需要在有争议的时间之前查找日志中的最后一个报表，该时间报告当前正在用作引用时钟的源计算机。  这可能是 IP 地址、计算机名或 VMIC 提供程序，这表示它与主机同步 Hyper-v。  下面的示例提供10.197.216.105 的 IPv4 地址。

    151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

现在您已经验证了引用时间链中的第一个系统，您需要调查引用时间源上的日志文件，并重复相同的步骤。  这会持续到你到达物理时钟（如 GPS）或已知的时间源（如 NIST）。  如果引用时钟为 GPS 硬件，则可能还需要来自制造制造商的日志。

## <a name="network-considerations"></a>网络注意事项
NTP 协议算法依赖于网络的对称。  由于增加网络跃点的数量，因此不能增加不对称的概率。  对于，很难预测你将在特定环境中看到的准确性类型。 

Windows Server 2016 中的性能监视器和新的 Windows 时间计数器可用于评估你的环境准确性和创建基线。 此外，还可以执行故障排除，确定网络上任何计算机的当前偏移量。

网络上准确的时间有两个通用标准。  PTP （[精度时间协议-IEEE 1588](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)）对网络基础结构的要求更严格，但通常可以提供二微秒的准确性。  NTP （[网络时间协议– RFC 1305](https://tools.ietf.org/html/rfc1305)）适用于更多的网络和环境，从而使管理更容易。 

默认情况下，对于未加入域的计算机，Windows 支持简单的 NTP （RFC2030）。  对于加入域的计算机，我们使用名为[MS SNTP](https://msdn.microsoft.com/library/cc246877.aspx)的安全 NTP，它利用域协商的机密，这些机密对 RFC1305 和 RFC5905 中所述的经过身份验证的 NTP 提供管理优势。   

域和未加入域的协议都需要 UDP 端口123。  有关 NTP 最佳实践的详细信息，请参阅[网络时间协议最佳当前实践 IETF 草稿](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00)。

### <a name="reliable-hardware-clock-rtc"></a>可靠硬件时钟（RTC）
除非超出了某些边界，否则 Windows 不会执行单步执行，而是确定时钟的时间。  这意味着，w32tm 将使用时钟更新频率设置（默认情况下，每秒使用 Windows Server 2016）来定期调整时钟的频率。  如果该时钟落后，则它将加速频率，如果超过此频率，则会降低频率。  但是，在时钟频率调整之间，硬件时钟处于控制阶段。  如果固件或硬件时钟有问题，计算机上的时间可能会变得不太准确。

这是您需要在您的环境中进行测试和基准测试的另一个原因。  如果 "计算时间偏移" 性能计数器未按目标的准确性稳定，则可能需要验证固件是否是最新的。  作为另一种测试，你可以查看重复硬件是否再现了相同的问题。

### <a name="troubleshooting-time-accuracy-and-ntp"></a>时间准确性和 NTP 疑难解答
您可以使用上面的 "发现层次结构" 部分来了解不准确的时间源。  查看时间偏移量，查找层次结构中的时间点，其中时间从其 NTP 源与其分离最大。  了解层次结构后，你将想要尝试并了解为什么特定时间源不会获得准确的时间。  

您可以使用以下工具来集中精力集中在系统上，以帮助您确定问题并找到解决方法。  下面的 UpstreamClockSource 引用是使用 "w32tm/config/status" 发现的时钟。


- 系统事件日志
- 使用以下内容启用日志记录： w32tm 日志-w32tm/debug/enable/file： C:\Windows\Temp\w32time-test.log/size： 10000000/entries： 0-300
- w32Time 注册表项 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- 本地网络跟踪
- 性能计数器（来自本地计算机或 UpstreamClockSource）
- W32tm/stripchart/computer： UpstreamClockSource
- PING UpstreamClockSource 以了解延迟和源的跃点数
- Tracert UpstreamClockSource

问题|    症状|   分辨率|
----- | ----- | ----- |
本地 TSC 时钟不稳定。| 使用 Perfmon-物理计算机–同步时钟稳定时钟，但你仍然会看到每1-2 分钟100us 几分钟。 |   更新固件或验证不同的硬件不会显示相同的问题。|
网络延迟|    w32tm stripchart 显示的 RoundTripDelay 超过10毫秒。  延迟的变化会导致噪声长达1/2 （例如，仅在一个方向上的延迟）。</br></br>UpstreamClockSource 是多个跃点，由 PING 指示。  TTL 应接近于128。</br></br>使用 Tracert 查找每个跃点的延迟。    | 查找时间的更接近的时钟源。  一种解决方案是在同一段上安装源时钟，或手动指向地理位置较近的源时钟。  对于域方案，请添加具有 GTimeServ 角色的计算机。 |  
无法可靠地访问 NTP 源|    W32tm/stripchart 间歇返回 "请求超时"    |NTP 源无响应|
NTP 源无响应|    检查用于 NTP 客户端源计数、NTP 服务器传入请求、NTP 服务器传出响应的 Perfmon 计数器，并确定与基线相比的使用情况。|    使用服务器性能计数器，确定负载对基线的引用是否已更改。</br></br>是否存在网络拥塞问题？|
域控制器未使用最准确的时钟|    拓扑中的更改或最近添加的主时间时钟。|   w32tm/resync/rediscover|
客户端时钟偏移| 时间-服务事件36在系统事件日志和/或日志文件的文本中，描述："NTP 客户端时间源计数" 计数器从1到0|排查上游源问题并了解其是否正在运行性能问题。|

### <a name="baselining-time"></a>基线时间
基线比较重要，这样您就可以首先了解网络的性能和准确性，并在问题发生时与未来的基准进行比较。  需要对根 PDC 或标记为 GTIMESRV 的任何计算机进行基准处理。  我们还建议你在每个林中对 PDC 进行基准处理。  最后，选取任何重要的 Dc 或具有感兴趣特征的计算机，如距离或高负载和基线。

这也适用于基准 Windows Server 2016 与 2012 R2，但你只需使用 w32tm/stripchart 作为可用于比较的工具，因为 Windows Server 2012R2 没有性能计数器。  应选择具有相同特征的两台计算机，或升级计算机并在更新后比较结果。  Windows 时间度量附录提供了有关如何在2016与2012之间进行详细度量的详细信息。

使用所有 w32time 性能计数器，收集至少一周的数据。  这将确保你有足够的时间来考虑网络中的各种情况，并有足够的时间来保证你的时间准确性稳定。

### <a name="ntp-server-redundancy"></a>NTP 服务器冗余
对于未加入域的计算机或 PDC 使用的手动 NTP 服务器配置，在可用性的情况下，有多台服务器是一种良好的冗余度量。  如果所有源都准确且稳定，它还可以提供更好的准确性。  但是，如果拓扑设计良好，或者时间源不稳定，则建议的准确性可能会更糟。  W32time 可以手动引用的受支持时间服务器的限制为10。 

## <a name="leap-seconds"></a>闰秒
随着时间的推移，地球的旋转周期随时间而变化，由 climatic 和地质事件引起。 通常情况下，每隔几年，这一变化大约为一秒。 每当原子时间的变体太大时，都会插入一秒（向上或向下），称为闰秒。 这种方法的实现方式是，差异永远不会超过0.9 秒。 此更正将在实际更正之前六个月内发布。 在 Windows Server 2016 之前，Microsoft 时间服务不知道闰秒，而是依赖于外部时间服务来处理此操作。 随着 Windows Server 2016 的时间准确性增加，Microsoft 正致力于为 leap 第二个问题提供更合适的解决方案。

## <a name="secure-time-seeding"></a>安全时间种子设定
服务器2016中的 W32time 包含安全时间种子设定功能。 此功能确定传出 SSL 连接的大致当前时间。  此时间值用来监视本地系统时钟并更正所有的错误。 可在[此博客文章](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/)中阅读有关此功能的详细信息。  在具有可靠时间源的部署和监视时间偏移量良好的监视的计算机上，你可以选择不使用安全时间种子设定功能，而是依赖于现有基础结构。 

可以通过以下步骤禁用该功能：

1.  在特定计算机上将 "UtilizeSSLTimeData" 注册表配置值设置为0：

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config/v UtilizeSslTimeData/t REG_DWORD/d 0/f


2.  如果由于某种原因无法立即重新启动计算机，则可以通知 W32time 服务有关配置更新的信息。 这会根据从 SSL 连接收集的时间数据来停止时间监视和强制执行。 

    W32tm/config/update

3.  重新启动计算机会使设置立即生效，并使其停止从 SSL 连接收集任何时间数据。  后一个部分的开销非常小，不一定是性能问题。

4.  若要在整个域中应用此设置，请将 "W32time 组策略" 设置中的 UtilizeSSLTimeData 值设置为0并发布设置。  当组策略客户端选取设置时，将通知 W32time 服务，并使用 SSL 时间数据停止时间监视和强制执行。 每次重新启动计算机时，SSL 时间数据收集将停止。 如果你的域具有便携式超薄笔记本电脑/平板电脑和其他设备，你可能希望从此策略更改中排除此类计算机。 这些设备最终将面对电池消耗，并需要安全时间种子设定功能来启动他们的时间。


