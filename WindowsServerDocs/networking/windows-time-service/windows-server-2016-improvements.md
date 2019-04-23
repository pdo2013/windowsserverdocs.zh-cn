

## <a name="windows-server-2016-improvements"></a>Windows Server 2016 的功能改进
### <a name="windows-time-service-and-ntp"></a>Windows 时间服务和 NTP
Windows Server 2016 进行改进，它使用更正时间和条件与 UTC 进行同步的本地时钟的算法。  NTP 使用 4 个值来计算的时间偏移量，基于客户端请求/响应和服务器请求/响应的时间戳。  但是，网络内容是纷繁多样，并且可能存在由于网络拥塞和其他因素会影响网络滞后时间的 NTP 从数据中的峰值。  Windows 2016 算法平均此干扰使用多种不同的技术这会导致稳定且准确的时钟。  此外，源我们引用时使用准确的时间的改进的 API，这为我们提供更好的分辨率。  借助这些改进，我们将能够实现 1 ms UTC 准确性跨域。

### <a name="hyper-v"></a>Hyper-V
Windows 2016 进行改进，HYPER-V TimeSync 服务。 改进包括 VM 开始或 VM 还原和中断延迟更正示例提供给 w32time 在更准确的初始时间。  此改进使我们能够保持的 rms （根意味着平方，这表示方差），主机在 10µs 50µs，甚至有 75%的负载的计算机上。 有关详细信息，请参阅[HYPER-V 体系结构](https://msdn.microsoft.com/library/cc768520.aspx)。

> [!NOTE]
> 负载是使用 prime95 基准使用平衡配置文件创建的。

此外，在宿主报告到来宾的层次级别是更透明的。  以前主机会提供固定的第 2，而不考虑其准确性。  借助 Windows Server 2016 中的更改，在宿主报告第一个大于主机层次，这会导致更好地时间对于虚拟来宾。  主机第取决于 w32time 通过正常方式基于其源时间。  来宾将查找最准确的时钟，而不是默认设置为主机的 Windows 2016 已加入域。  正是出于这个原因，我们建议您手动禁用计算机加入到域 Windows 2012R2 文件夹及其子文件夹中的 HYPER-V 时间提供程序设置。

### <a name="monitoring"></a>监视
添加了性能监视器计数器。  这些基线，允许您监视和故障排除的时间准确性。  这些计数器包括：

计数器|描述|
----- | ----- |
计算时间偏移量|   当由 W32Time 服务以微秒为单位计算偏移系统时钟与所选的时间源之间的绝对时间。 当可用的新的有效示例，计算的时间是由该示例的时间偏移量的更新。 这是本地时钟的实际时间偏移量。 W32time 启动时钟更正使用此偏移量，并需要将应用到的本地时钟的剩余时间偏移量与更新之间示例的计算的时间。 可以使用较低的轮询间隔使用此性能计数器跟踪时钟准确性 (例如： 256 秒或更少)，然后查找要小于所需的时钟准确性限制的计数器值。|
时钟频率调整| 绝对时钟频率调整对本地系统时钟 W32Time 每 10 亿个部分中所做。 此计数器有助于直观显示 W32time 所执行的操作。|
NTP Roundtrip Delay|    NTP 客户端中以微秒为单位从服务器接收响应所经历的最新往返延迟。 这是次已用在 NTP 客户端之间传输的 NTP 服务器的请求和接收来自服务器的有效响应。 此计数器可帮助描述遇到的 NTP 客户端的延迟特征。 更大或不同的往返操作可以将干扰添加到 NTP 时间计算，这可能会影响通过 NTP 时间同步的准确性。|
NTP 客户端源计数|    正由 NTP 客户端的 NTP 时间源的活动数。 这是处于活动状态，不同的对此客户端的请求的响应的时间服务器的 IP 地址的计数。 此数可能大于或小于配置的对等，具体取决于 DNS 解析的对等名称以及当前的市场宣传功能。|
NTP 服务器传入的请求|   NTP 服务器 （请求/秒） 收到的请求数。|
NTP 服务器传出响应|  回答的 NTP 服务器 （响应数/秒） 的请求数。|

前 3 个计数器目标准确性问题故障排除的方案。  故障排除的时间准确性和 NTP 部分下面下,[最佳做法](#BestPractices)，具有更多详细信息。
最后 3 个计数器介绍 NTP 服务器方案，并会有所帮助时确定的负载和基线你当前的性能。

### <a name="configuration-updates-per-environment"></a>每个环境的配置更新
下面介绍 Windows 2016 和早期版本之间的每个角色的默认配置中的更改。  Windows Server 2016 和 Windows 10 周年更新 （内部版本 14393） 的设置现在是唯一这就是为什么有显示为单独的列。 

|角色|设置|Windows Server 2016|Windows 10|Windows Server 2012 R2</br>Windows Server 2008 R2</br>Windows 10|
|---|---|---|---|---|
|**独立/Nano Server**||||
| |*Time Server*|time.windows.com|NA|time.windows.com|
| |*轮询频率*|64-1024 秒|NA|每周一次|
| |*时钟更新频率*|一次第二个|NA|每小时一次|
|**独立客户端**||||
| |*Time Server*|NA|time.windows.com|time.windows.com|
| |*轮询频率*|NA|每日一次|每周一次|
| |*时钟更新频率*|NA|每日一次|每周一次|
|**域控制器**||||
| |*Time Server*|PDC/GTIMESERV|NA|PDC/GTIMESERV|
| |*轮询频率*|64-1024 秒|NA|1024-32768 秒|
| |*时钟更新频率*|每日一次|NA|每周一次|
|**域成员服务器**||||
| |*Time Server*|DC|NA|DC|
| |*轮询频率*|64-1024 秒|NA|1024-32768 秒|
| |*时钟更新频率*|一次第二个|NA|每 5 分钟一次|
|**域成员客户端**||||
| |*Time Server*|NA|DC|DC|
| |*轮询频率*|NA|1204-32768 秒|1024-32768 秒|
| |*时钟更新频率*|NA|每 5 分钟一次|每 5 分钟一次|
|**HYPER-V 来宾**||||
| |*Time Server*|选择最佳选项基于主机和时间的服务器的第|选择最佳选项基于主机和时间的服务器的第|默认值为主机|
| |*轮询频率*|基于角色的更高版本|基于角色的更高版本|基于角色的更高版本|
| |*时钟更新频率*|基于角色的更高版本|基于角色的更高版本|基于角色的更高版本|

>[!NOTE]
>在 HYPER-V 中的 Linux，请参阅[允许的 linux 操作系统的 HYPER-V 主机时使用](#AllowingLinux)下面一节。

### <a name="impact-of-increased-polling-and-clock-update-frequency"></a>增加的轮询和时钟更新频率的影响
为了提供更准确的时间，轮询频率和时钟更新的默认值会增加，使我们能够更频繁地进行微调。  这将会导致更多的 UDP/NTP 流量，但是，这些数据是小的因此应该有很少或不会影响通过宽带链接。 好处，但是，是，时间应为广泛的硬件和环境上更好。

对于电池备份设备，增加轮询频率可能会导致问题。  电池设备不会存储处于关闭状态时的时间。  在恢复时可能需要频繁更正的时钟。  增加轮询频率将导致变得不稳定的时钟，并且还可以使用更多的能力。  Microsoft 建议您不要更改客户端默认设置。

域控制器应按最小方式影响甚至与 NTP AD 域中的客户端中的提高更新的乘效果。  NTP 具有资源消耗相比其他协议和边际影响更小。  则更有可能，以便在不受 Windows Server 2016 的增加设置的影响之前到达其他域功能的限制。  Active Directory 会使用安全 NTP，往往不太准确比简单 NTP 同步时间，但我们已验证它将扩展到 PDC 离开客户端两个层次。

作为保守的计划，你应保留每个核心每秒 100 个 NTP 请求。  例如，一个域中的 4 个核心的 4 个 Dc 组成，可以为每秒 1600 NTP 请求提供服务。  如果你有 10 个 k 客户端配置为同步时间每秒一次 64，并且随着时间的推移统一接收请求，则会看到 10,000/64，或大约 160 每秒请求数，分布在所有域控制器。  此权限轻松我们 1600 NTP 每秒请求基于此示例。  这些是保守规划建议，当然有大型依赖于网络、 处理器速度和加载，因此像往常一样基准和测试环境中。

务必还请注意，是否你的 Dc 都运行相当多的 CPU 负载大于 40%，这将几乎可以肯定将干扰添加到 NTP 响应并且会影响你在你的域中的时间准确性。  同样，您需要了解实际结果在环境中测试。

## <a name="time-accuracy-measurements"></a>时间准确性度量值
### <a name="methodology"></a>方法
若要测量时间准确性的 Windows Server 2016，我们使用各种工具、 方法和环境。  这些技术可用于测量和优化你的环境并确定准确性结果是否满足您的要求。 

我们域源时钟包括两个 GPS 硬件的高精度 NTP 服务器。  我们还使用单独的参考测试计算机，进行度量，还必须安装不同制造商提供的高精度 GPS 硬件。  对于某些测试，将需要的准确且可靠的时钟源要用作除了域时钟源的引用。

我们使用四种不同方法来度量准确性具有物理和虚拟机。 多个方法提供独立的方法来验证结果。


1. 测量了单独的 GPS 硬件我们引用测试计算机的本地时钟，w32tm 的限制。  
2.  向客户端使用 W32tm"stripchart"度量值 NTP ping 通过 NTP 服务器
3.  从客户端使用 W32tm"stripchart"的 NTP 服务器的度量值 NTP ping
4.  度量值的 HYPER-V 会从主机到来宾使用时间戳计数器 (TSC)。  此计数器中这两个分区的分区和系统时间之间共享。  我们计算虚拟机中的主机和客户端时间差异。  然后我们使用 TSC 时钟由于测量值不在同一时间发生，从来宾，主机时间执行内插。  此外，我们将在 API 中使用 TSV 时钟分离出延迟和延迟。

W32tm 是内置的但是我们在我们的测试过程中使用的其他工具可用于 Microsoft 存储库在 GitHub 上作为开放源代码的测试和使用情况。  存储库上的 WIKI 包含描述如何使用这些工具进行度量的详细信息。

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

如下所示的测试结果是我们在一个测试环境中进行的度量值的子集。  它们说明了维护开始时间层次结构，并在时间层次结构末尾的子域客户端时的准确性。  这与基于 2012年拓扑中的比较在同一台计算机进行比较。

### <a name="topology"></a>拓扑
有关比较，我们已测试的 Windows Server 2012R2 和 Windows Server 2016 基于拓扑。  这两种拓扑包含两个 HYPER-V 主机的物理计算机的 Windows Server 2016 计算机使用 GPS 时钟硬件安装对进行引用。  每个主机运行 3 个已加入域 windows 来宾，按照以下拓扑来排列。  行表示的时间层次结构，并使用协议/传输。

![Windows 时间](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology1.png)

![Windows 时间](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>图形结果概述
以下两个关系图表示基于上述拓扑的域中的两个特定成员的时间准确性。  每个图显示 Windows Server 2012R2 和 2016年结果叠加，直观地演示这些改进。  准确性是度量值从在来宾机器相比主机。  图形数据表示整个组我们所做的测试的子集，并显示最佳事例表和最坏的情况。  

![Windows 时间](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>根级域 PDC 的性能
根 PDC 同步到 （使用 VMIC） 都经验证的可准确且可靠的 GPS 硬件与 Windows Server 2016 的 HYPER-V 主机。  这是一项关键要求为 1 ms 准确性，显示为绿色阴影区域。

![Windows 时间](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>子域客户端的性能
子域客户端会附加到子域 PDC 到根 PDC 进行通信。  它时也是在 1 ms 要求。

![Windows 时间](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart2.png)


### <a name="long-distance-test"></a>长途测试
下表比较了 1 到 6 Windows Server 2016 的物理网络跃点的虚拟网络跃点。  这两个图表相互重叠结构以透明度显示重叠的数据。  不断增加的网络跃点计数意味着更高的延迟和较大的时间偏差。  图表为放大，因此在 1 ms 边界，绿色区域中，由表示较大。  正如您所看到的时间是仍在 1 ms 使用多个跃点。  它会产生负面偏移，用于演示网络不对称。  当然，每个网络都不同，并且度量值依赖于多种环境因素。

![Windows 时间](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart3.png)

## <a name="BestPractices"></a>准确的计时的最佳做法
### <a name="solid-source-clock"></a>Solid 源时钟
计算机时间的好坏都为其与同步源时钟。  为了实现 1 ms 的准确性，你将需要 GPS 硬件或时间设备作为主源时钟引用在网络上。  使用默认值为 time.windows.com，可能无法提供稳定和本地时间源。  此外，随着您进一步远离源时钟，网络会影响的准确性。  具有主源时钟在每个数据中心将需要最高的准确性。

### <a name="hardware-gps-options"></a>硬件 GPS 选项
有各种硬件解决方案能够提供准确的时间。  一般情况下，解决方案现在基于 GPS 天线。  也有单选和拨号调制解调器解决方案使用的专用的线路。  它们将附加到为网络设备，或插入到 PC，例如通过 PCIe 或 USB 设备的 Windows。  不同的选项将提供不同级别的准确性，并与往常一样，结果取决于你的环境。  这会影响准确性的变量包括 GPS 可用性、 网络稳定性和加载和 PC 硬件。  选择源时钟，如我们所述，是必需的稳定且准确的时间时，这些都是很重要的因素。

### <a name="domain-and-synchronizing-time"></a>域和同步时间
域成员使用域层次结构来确定哪台计算机它们使用作为源同步时间。  每个域成员会发现另一台计算机作为时钟源同步并保存它。  每种类型的域成员才能进行时间同步查找时钟源遵循一组不同的规则。  在目录林根 PDC 是所有域的默认时钟源。  下面列出了不同的角色和高级别说明它们如何查找源：


- **与 PDC 角色的域控制器**– 此计算机是域的权威时间源。 它将在域中具有最准确的时间可用并必须在与同步 DC 在父域中，除在情况下， [GTIMESERV](#GTIMESERV)启用角色。 
- **任何其他域控制器**– 此计算机将充当客户端和域中的成员服务器的时间源。 DC 可以与自己域的 PDC 或其父域中的任何 DC 同步。
- **客户端/成员服务器**– 此计算机可以与任何 DC 或其自己的域，或 DC 的 PDC 或在父域的 PDC 同步。

根据可用的候选项，评分系统用于查找最佳的时间源。  此系统将考虑在内的时间源和其相对位置的可靠性。  是当的时间启动服务后，将发生这种情况。  如果你需要具有更好地控制如何时间同步，可以在特定位置中添加合适的时间服务器或添加冗余。  请参阅[指定本地可靠时间服务使用 GTIMESERV](#GTIMESERV)部分，了解详细信息。

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>混合的操作系统环境 （Win2012R2 和 Win2008R2）
虽然需要最高的准确性纯 Windows Server 2016 域环境，则仍有优势在混合环境中。  部署 Windows Server 2016 中的 HYPER-V 的 Windows 2012 域将由于我们前面所述但仅来宾是否还 Windows Server 2016 的改进中受益的来宾。  Windows Server 2016 PDC，将能够更稳定源由于改进的算法，它将提供更准确的时间。  作为替换 PDC 可能不是一种，则可以改为添加 Windows Server 2016 DC [GTIMESERV](#GTIMESERV)前集是准确性为您的域中升级。  Windows Server 2016 DC 能向下游时间客户端提供更好的时间，但是，它仅就是其源 NTP 时间。

如上所述，也已修改的时钟轮询间隔和刷新频率与 Windows Server 2016 中。  这些可以手动更改为下级域控制器或通过组策略应用。  虽然我们尚未测试这些配置，它们应在 Win2008R2 和 Win2012R2 中的行为，提供了一些好处。

版本以前 Windows Server 2016 必须保持准确的时间保持系统的时间偏差，立即进行调整后，触发多个问题。  因此，经常从一个准确的 NTP 源获取时间示例，并规定的数据的本地时钟会导致在其系统时钟的较小偏差在内部采样的时段，从而导致更好地保持低级别操作系统版本上的时间。 Windows Server 2012R2 NTP 配置的客户端，使用高准确性设置时，同步其时间从一个准确的 Windows 2016 NTP 服务器时，最好地观察到的准确性是大约 5 毫秒。

在某些情况下，涉及来宾域控制器，HYPER-V TimeSync 示例可能会中断域时间同步。  这不应该再受 Server 2016 来宾在 Server 2016 HYPER-V 主机上运行的问题。

若要禁用 HYPER-V TimeSync 服务提供到 w32time 示例，请设置以下来宾注册表项：

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider 
    "Enabled"=dword:00000000

#### <a name="AllowingLinux"></a>允许使用的 HYPER-V 主机时间的 linux 操作系统
对于 Linux 来宾在 HYPER-V 中运行，客户端通常配置为使用 NTP 服务器同步的 NTP 守护程序。  如果 Linux 分发版支持 TimeSync 版本 4 协议并 Linux 来宾已启用 TimeSync 集成服务，它将同步针对主机时间。 这可能会导致不一致的时间保持如果启用了这两种方法。

若要以独占方式对主机时间同步，建议禁用 NTP 时间同步通过以下任一方法：

- 禁用在 ntp.conf 文件中的任何 NTP 服务器
- 或禁用 NTP 守护程序

在此配置中，时间服务器参数是此主机。  其轮询频率为 5 秒和时钟更新频率也为 5 秒。

若要以独占方式通过 NTP 同步，建议禁用 TimeSync 集成服务在来宾中。

> [!NOTE]
> 注意：支持与 Linux 来宾的准确时间，需要一项功能，则仅支持的最新上游 Linux 内核，它不是被广泛用于跨所有 Linux 发行版尚。 请参阅[Windows 上的 HYPER-V 的支持的 Linux 和 FreeBSD 虚拟机](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows)支持的分发有关的详细信息。

#### <a name="GTIMESERV"></a>指定使用 GTIMESERV 的可靠的本地时间服务
您可以指定一个或多个域控制器为准确源时钟使用 GTIMESERV，良好的时间服务器，标志。  例如，配备有 GPS 硬件特定的域控制器可以标记为 GTIMESERV。  这将确保你的域引用基于 GPS 硬件时钟。

> [!NOTE]
> 有关域标志的详细信息可在[MS ADTS 协议文档](https://msdn.microsoft.com/library/mt226583.aspx)。

TIMESERV 是另一个相关的域服务标志指示是否当前权威机，则可以更改该如果 DC 失去连接。  在此状态的 DC 将返回"未知层次"时通过 NTP 查询。  在尝试使用多次，DC 将记录系统事件时间服务事件 36。

如果你想要将 DC 配置为 GTIMESERV，这可以使用以下命令手动进行配置。  在这种情况下 DC 使用另一台计算机作为主时钟。  这可能是设备或专用的计算机。

    w32tm /config /manualpeerlist:”master_clock1,0x8 master_clock2,0x8” /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> 有关详细信息，请参阅[配置 Windows 时间服务](https://technet.microsoft.com/library/cc731191.aspx)

如果域控制器必须安装的 GPS 硬件，您需要使用以下步骤禁用 NTP 客户端和启用的 NTP 服务器。

通过禁用 NTP 客户端启动并启用使用这些注册表项更改的 NTP 服务器。

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

接下来，重新启动 Windows 时间服务

    net stop w32time && net start w32time

最后，您指示此计算机具有可靠时间源使用。
   
    w32tm /config /reliable:yes /update

若要检查已正确完成所做的更改，可以运行以下命令，这会影响结果如下所示。 

    w32tm /query /configuration

ReplTest1|预期的设置|
----- | ----- |
AnnounceFlags|  5 （本地）|
NtpServer   |（本地）|
DllName |C:\WINDOWS\SYSTEM32\w32time.DLL (Local)|
Enabled |1 （本地）|
NtpClient|  （本地）|

    w32tm /query /status /verbose

值|  预期的设置|
----- | ----- |
第|    1 （主引用-与无线电时钟同步）|
ReferenceId|    0x4C4F434C (源名称："LOCAL")|
Source| 本地 CMOS 时钟|
阶段偏移量|   0.0000000s|
服务器角色|    576 （可靠的时间服务）|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>第三方虚拟平台上的 Windows Server 2016
Windows 虚拟化，默认情况下在虚拟机监控程序时，负责提供时间。  但已加入域的成员必须是与 Active Directory 才能正常工作的顺序中的域控制器同步。  最好是禁用来宾和主机的任何第三方虚拟平台之间的任何时间虚拟化。

#### <a name="discovering-the-hierarchy"></a>发现在层次结构
由于在域中，是动态的对 master 时钟源的时间层次结构链，协商，您将需要查询特定的计算机，以了解它的时间源和链接到主源时钟的状态。  这可以帮助诊断时间同步问题。

授予你想要进行故障排除特定的客户端;第一步是了解它的时间源通过使用此 w32tm 命令。

    w32tm /query /status

结果显示及其他某些数据源。  源表示与之同步时间域中。  这是此计算机时间层次结构的第一步。
接下来，使用上面提供的源项和 /StripChart 参数用于在链中查找下一步的时间源。

    w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

也很有用，以下命令列出了它可以找到指定的域中每个域控制器，并将打印结果，这样就可以确定每个合作伙伴。  此命令将包括手动配置的计算机。
    
    w32tm /monitor /domain:my_domain

使用列表，你可以跟踪通过域结果并了解在层次结构，以及在每个步骤的时间偏移量。  通过定位的点的时间偏移量获取显著更糟的是，您可以找出不正确的时间的根目录。  从此处可以尝试了解该时间为何开启不正确[w32tm 日志记录](#W32Logging)。 

#### <a name="using-group-policy"></a>使用组策略
您可以使用组策略来完成更严格的准确性，例如，客户端分配到使用特定的 NTP 服务器或虚拟化时，控制如何下层操作系统的配置。  
下面是可能的方案和相关的组策略设置的列表：

**虚拟化域**-为了控制 Windows 2012R2 中虚拟化域控制器，以便其域中，同步时间，而不是与 HYPER-V 主机，您可以禁用此注册表项。   Pdc，您不想要禁用该条目的 HYPER-V 主机将提供最稳定的时间源。  注册表项需要更改后重新启动 w32time 服务。

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider]
    "Enabled"=dword:00000000

**准确性敏感负载**-对于时间准确性敏感的工作负荷，可以配置的设置的 NTP 服务器的计算机组以及任何相关的时间设置，例如轮询和时钟更新频率。  这一般将由域，但可以获得更多控制目标为直接指向主时钟的特定计算机。

组策略设置|   “新值”|
----- | ----- |
NtpServer|  ClockMasterName 0x8|
MinPollInterval|    6 – 64 秒|
MaxPollInterval|    6|
UpdateInterval| 100-每秒一次|
EventLogFlags|  3 – 所有特殊时间日志记录|

> [!NOTE]
> NtpServer 和 EventLogFlags 设置位于下 System\Windows 时间 Service\Time 提供使用配置 Windows NTP 客户端设置。  其他 3 位于下 System\Windows 时间服务使用的全局配置设置。

**远程准确性敏感加载远程**– 对于实例零售和支付信用额度行业 (PCI) 的分支域中的系统，Windows 将使用当前的站点信息和 DC 定位器来查找本地 DC，除非手动 NTP 时间源配置。  此环境需要 1 秒的准确性，它使用更快地融合到正确的时间。  此选项允许 w32time 服务向后移动时钟。  如果这是可接受，满足你的要求，可以创建以下策略。   与任何环境中，可确保测试和基线到你的网络。 

组策略设置|   “新值”|
----- | ----- |
MaxAllowedPhaseOffset|  1，在多个第二个，如果此方法将时钟设置为正确的时间。|

MaxAllowedPhaseOffset 设置位于下 System\Windows 时间服务使用的全局配置设置。

> [!NOTE]
> 有关组策略和相关的项的详细信息，请参阅[Windows 时间服务工具](windows-time-service-tools-and-settings.md)和 TechNet 上的设置一文。

## <a name="azure-and-windows-iaas-considerations"></a>Azure 和 Windows IaaS 注意事项

### <a name="azure-virtual-machine-active-directory-domain-services"></a>Azure 虚拟机：Active Directory 域服务
如果运行 Active Directory 域服务的 Azure 虚拟机属于的现有本地 Active Directory 林，然后 TimeSync(VMIC)，应禁用。 这将允许所有域控制器在林中，物理和虚拟的以使用一次同步层次结构。 请参阅最佳实践白皮书["运行域控制器中的 HYPER-V"](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

### <a name="azure-virtual-machine-domain-joined-machine"></a>Azure 虚拟机：已加入域的计算机
如果要在托管已加入到现有的 Active Directory 林域的计算机，虚拟或物理，最佳做法是为来宾禁用 TimeSync 并确保 W32Time 配置为与它的域控制器通过配置的时间同步类型 = NTP5

### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Azure 虚拟机：独立工作组计算机
如果 Azure VM 未加入到域，也不是域控制器，该建议是保持默认时间配置，并让同步与主机 VM。

## <a name="windows-application-requiring-accurate-time"></a>Windows 应用程序需要准确的时间
### <a name="time-stamp-api"></a>时间戳 API
应使用程序需要 UTC，并不时间流逝的时间，最大准确性[GetSystemTimePreciseAsFileTime API](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx)。  这可确保你的应用程序获取系统时间，Windows 时间服务的条件。

### <a name="udp-performance"></a>UDP 性能
如果必须移植基础筛选引擎使用 UDP 通信为事务和它的重要以尽量降低延迟，有一些相关注册表项可用于配置一系列端口要从排除的应用程序。  这将提高这两个的延迟并提高吞吐量。  但是，对注册表的更改应限制为有经验的管理员。  此外，这种解决办法不包括从受保护的防火墙的端口。  请参阅下面有关详细信息的项目引用。

对于 Windows Server 2012 和 Windows Server 2008，需要先安装一个修补程序。  你可以引用此知识库文章：[在 Windows 8 和 Windows Server 2012 中运行的多播接收方应用程序时的数据报损失](https://support.microsoft.com/kb/2808584)

### <a name="update-network-drivers"></a>更新网络驱动程序
某些网络供应商具有驱动程序更新改善与驱动程序的延迟和缓冲 UDP 数据包相关的性能。  请联系网络供应商联系，以查看是否有更新，以帮助为 UDP 的吞吐量。

## <a name="logging-for-auditing-purposes"></a>用于审核目的的日志记录
为了遵守时间跟踪规定可以手动存档 w32tm 日志、 事件日志和性能监视器信息。  更高版本，存档的信息可以用于证明在特定时间在过去的符合性。  以下因素用于指示准确性。


1. 时钟准确性使用的计算时间的偏移量性能监视器计数器。  这将显示在所需的准确性中处理的时钟。
2.  时钟源查找"对等方响应从"w32tm 日志中。   后面的消息文本是 IP 地址或 VMIC，它描述了时间源和引用时钟链中的下一步，以验证。
3.  时钟条件状态使用 w32tm 日志以验证"ClockDispl 原则：\*倾斜\*时间\*"发生。  这表示该 w32tm 时处于活动状态。

### <a name="event-logging"></a>事件日志记录
若要获取完整的情景，还需要事件日志信息。  通过收集的系统事件日志，并对时间服务器进行筛选，Microsoft Windows 内核启动时、 Microsoft-Windows-内核-General，您可以发现是否已更改时，例如，第三方其他影响。  这些日志可能需要排除外部干扰。  组策略可能会影响哪些事件日志写入到日志。  有关详细信息，请参阅使用组策略一上面部分。

### <a name="W32Logging"></a>W32time 调试日志记录
若要启用以供审核，w32tm，以下命令将启用日志记录，用于显示时钟的定期更新，并指示源时钟。  重新启动服务以启用新的日志记录。  

有关详细信息，请参阅[如何调试 Windows 时间服务中的日志记录启用](https://support.microsoft.com/kb/816043)。

    w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

### <a name="performance-monitor"></a>性能监视器
Windows Server 2016 的 Windows 时间服务会公开性能计数器可以用于收集的审核日志记录。  这些选项可以记录在本地或远程。  您可以记录的计算机时间偏移量和往返行程延迟计数器。  
并可以像任何性能计数器，远程监视它们并创建使用 System Center Operations Manager 警报。  警报，可以用于警报则时所需的准确性从时间偏移偏移。  [System Center 管理包](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx)提供了更多信息。

### <a name="windows-traceability-example"></a>Windows 可跟踪性示例
从 w32tm 日志文件将想要验证两条信息。  第一个是日志文件当前已条件时钟的指示。  这证明，时钟已正在限制 Windows 时间服务在有争议的时间。

    151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307
    151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41
    151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

要点是，您会看到与你的系统时钟进行交互的 ClockDispln 野马即证明 w32time 前缀的消息。
 
接下来，您需要在有争议的时间将报告为引用时钟当前正在使用的源计算机之前在日志中找到上一个报表。  这可能是 IP 地址、 计算机名称或 VMIC 提供程序，这表示它正在同步的 HYPER-V 主机。  下面的示例提供了 10.197.216.105 的 IPv4 地址。

    151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

现在，验证引用时间链中的第一个系统，您需要调查上引用的时间源的日志文件并重复相同的步骤。  此过程将继续，直到你转到物理时钟，例如 GPS 或 NIST 之类的已知的时间源。  如果引用时钟 GPS 硬件，然后从制造的日志可能还需要。

## <a name="network-considerations"></a>网络注意事项
NTP 协议算法具有依赖关系网络的对称性。  随着您增加的网络跃点数，有点不对称的可能性也相应增加。  有，很难预测特定环境中将看到哪些类型的准确性。 

可以使用性能监视器和 Windows Server 2016 中新的 Windows Time 计数器来评估您的环境更精确和创建基线。 此外，还可以执行故障排除，以确定网络上的任何计算机的当前偏移量。

有两种常规标准为准确的时间在网络上。  PTP ([精度时间协议-IEEE 1588](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) 网络基础结构上具有更严格的要求，但通常可以提供子微秒准确性。  NTP ([网络时间协议 – RFC 1305](https://tools.ietf.org/html/rfc1305)) 适用于有更多的网络和环境，因此可以轻松地管理。 

对于非域联接计算机默认情况下，Windows 支持简单 NTP (RFC2030)。  对于加入域的计算机，我们使用名为安全 NTP [MS SNTP](https://msdn.microsoft.com/library/cc246877.aspx)，利用域协商机密，后者提供相比进行身份验证 NTP RFC1305 和 RFC5905 中所述的管理优势。   

域和非域联接的协议需要 UDP 端口 123。  有关 NTP 最佳实践的详细信息，请参阅[网络时间协议最佳实践 IETF 草稿](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00)。

### <a name="reliable-hardware-clock-rtc"></a>可靠的硬件时钟 (RTC)
除非特定边界超过，但而不是惩罚时钟，Windows 将执行不是单步时间。  这意味着 w32tm 在一定的时间间隔，使用设置的默认值为一次第二个 Windows Server 2016 的时钟更新频率调整时钟的频率。  如果时钟位于后面，其加速了频率，如果继续操作，会降低频率。  但是，在此期间之间时钟频率调整，硬件时钟是在控件中。  如果没有固件或硬件时钟存在问题，在计算机上的时间可能会不太准确。

这是您需要测试的另一个原因，您的环境中的基线。  如果你面向的准确性不稳定的"计算时间偏移"性能计数器，您可能要验证您的固件是最新。  作为另一个测试，则可以看到如果重复的硬件重现相同的问题。

### <a name="troubleshooting-time-accuracy-and-ntp"></a>故障排除的时间准确性和 NTP
可以使用发现上面以了解时间错误来源的层次结构部分。  查看的时间偏移量，其中时间分离充分利用其 NTP 源层次结构中找到的点。  一旦您了解在层次结构，你将想要尝试并了解为什么该特定的时间源不会收到准确的时间。  

将重点放在具有同名的不同时间的系统，您可以使用以下这些工具以收集详细信息来帮助您确定问题并查找解决方法。  下面，UpstreamClockSource 引用是使用"w32tm /config /status"发现的时钟。


- 系统事件日志
- 启用日志记录使用： w32tm 日志-w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-300
- w32Time Registry key HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- 本地网络跟踪
- （从本地计算机或 UpstreamClockSource） 的性能计数器
- W32tm /stripchart /computer:UpstreamClockSource
- PING UpstreamClockSource 了解延迟和到源的跃点数
- Tracert UpstreamClockSource

问题|    症状|   分辨率|
----- | ----- | ----- |
本地 TSC 时钟不稳定。| 使用 Perfmon 的物理计算机 – 同步时钟稳定时钟，但你仍看到几个 100us 每隔 1-2 分钟。 |   更新固件或验证不同的硬件不会显示相同的问题。|
网络延迟|    w32tm stripchart 显示多个 10 毫秒的 RoundTripDelay。  在延迟的原因干扰 ½ 一样大的往返行程时间，例如仅在一个方向的延迟的变体。</br></br>UpstreamClockSource 将多个跃点，如 PING 所示。  TTL 应接近 128。</br></br>使用 Tracert 来查找每个跃点的延迟。    | 次查找更接近的时钟来源。  一种解决方案是在同一段上安装源时钟或手动指向地理位置更靠近源时钟。  有关域的方案中，添加具有 GTimeServ 角色的计算机。 |  
无法可靠地访问 NTP 源|    W32tm /stripchart 间歇性地返回"请求已超时"    |NTP 源未响应|
NTP 源未响应|    检查 NTP 客户端的源计数，NTP 服务器的传入请求，NTP 服务器传出响应的 Perfmon 计数器，并确定你的使用情况与您的基线相比。|    通过使用 server 性能计数器，确定负载是否已更改引用你的基线。</br></br>是否存在网络拥塞问题？|
域控制器未使用的最准确的时钟|    中的拓扑或最近添加的主时钟的更改。|   w32tm /resync /rediscover|
客户端时钟偏差| 时间服务事件 36 在系统事件日志和/或文本中描述的日志文件："NTP 客户端时间源 Count"计数器从 1 转到 0|上游源进行故障排除和了解其是否运行出现性能问题。|

### <a name="baselining-time"></a>设置基准时间
以便可以首先，了解性能和准确性的您的网络，并比较在基线与将来出现问题时，基线非常重要。  你将希望基线根 PDC 或任何计算机使用 GTIMESRV 标记。  我们将还建议您基线中每个林 PDC。  最后选择任何关键的 Dc 或了有趣的特征，例如距离或高负载和基线的计算机这些。

它还是适用于 Windows Server 2016 基线 vs 2012 R2 中，但您只需 w32tm /stripchart 作为一种工具可用于比较，因为 Windows Server 2012R2 不包含性能计数器。  应选择具有相同特性的两台计算机或计算机升级和更新后的结果进行比较。  Windows 时间度量附录包含如何执行操作之间 2016年和 2012年的详细的度量的详细信息。

使用所有 w32time 性能计数器，收集至少一周的数据。  这将确保您具有足够的各种随着时间的推移网络中的引用和足够的运行提供时间准确性是稳定的置信度。

### <a name="ntp-server-redundancy"></a>NTP 服务器冗余
有关与非域联接的计算机或 PDC 一起使用的手动 NTP 服务器配置，让多个服务器是发生可用性时的较好冗余性能度量值。  它还可能产生更好的准确性，假设所有源都的准确性和稳定。  但是，如果还没有设计拓扑，或者时间源不稳定，得到的准确性可能更糟的是以便建议要小心。  可以手动引用服务器 w32time 受支持的时间限制为 10。 

## <a name="leap-seconds"></a>闰秒
地球的轮换期间随时间，而引起的气候和地质事件。 通常情况下，变体是有关第二个每隔几年。 每当从原子时间变体增长得过大，更正的一秒 （向上或向下） 是插入，名为闰秒。 这是差异永远不会超过 0.9 秒的方式。 此更正宣布实际更正六个月。 在 Windows Server 2016 之前 Microsoft 时间服务并不了解的闰秒，但依赖于外部时间服务来处理此。 使用 Windows Server 2016 的更高的时间准确性，Microsoft 正致力于闰秒问题更合适的解决方案。

## <a name="secure-time-seeding"></a>安全时间种子设定
W32time 在 Server 2016 中的包括安全时间种子设定的功能。 此功能确定传出的 SSL 连接的近似的当前时间。  此时间值用于监视本地系统时钟并更正任何毛错误。 你可以阅读更多有关中的功能[这篇博客文章](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/)。  与可靠时间源和包括监视时间偏移量的也受监视的计算机的部署，可以选择不使用安全时间种子设定功能，并改为依赖于现有的基础结构。 

你可以禁用该功能通过以下步骤：

1.  设置为 0 的特定计算机上的 UtilizeSSLTimeData 注册表配置值：

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t REG_DWORD /d 0 /f


2.  如果你不能重新启动计算机立即由于某种原因，可以通知有关配置更新 W32time 服务。 这将停止时间监视和强制执行基于时间的数据收集从 SSL 连接。 

    W32tm.exe /config /update

3.  重新启动计算机使设置立即生效，同时也导致其停止收集从 SSL 连接的任何时间数据。  后半部分开销很小，不应是性能问题。

4.  若要应用的整个域中的此设置，请将 UtilizeSSLTimeData 值设置为 0 的 W32time 组策略设置中，并发布设置。  当设置由组策略客户端获取时，通知 W32time 服务和它将停止时间监视和强制使用 SSL 时数据。 每台计算机重启时，SSL 时间数据收集将停止。 如果你的域具有可移植 slim 笔记本电脑/平板电脑和其他设备，你可能想要从本次策略更改排除此类计算机。 这些设备将最终面临电池耗尽，需要保护时间种子设定功能来启动它们的时间。


