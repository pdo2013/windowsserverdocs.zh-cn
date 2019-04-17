---
ms.assetid: 6086947f-f9ef-4e18-9f07-6c7c81d7002c
title: Windows 时服务工具和设置
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 70b7ee4a9955e023d1664a3c29295a22cd5dc75b
ms.sourcegitcommit: fd6a46b702b235f38d90814dd769106ab37cd61b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="windows-time-service-tools-and-settings"></a>Windows 时服务工具和设置

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012


**在此部分中**  
  
-   [Windows 时服务工具](#w2k3tr_times_tools_dyax)  
  
-   [Windows 时间 Service 注册表项](#w2k3tr_times_tools_uhlp)  
  
-   [Windows 时间 Service 组策略设置](#w2k3tr_times_tools_vwtt)  
  
-   [使用 Windows 时服务的网络端口](#w2k3tr_times_tools_suxb)  
  
-   [相关的信息](#w2k3tr_times_tools_qoep)  
  
> [!NOTE]  
> 本主题包含仅关于工具和 Windows 时间服务 (W32Time) 的设置的信息。 如果你只希望同步加入域的客户端计算机的时间，请参阅[客户端将计算机配置为自动域时间同步](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)。 有关更多有关如何配置 Windows 时间服务的主题，请参阅列表中的部分中的主题[在哪里找到 Windows 时服务配置信息](https://technet.microsoft.com/library/cc773061.aspx)。  
  
> [!CAUTION]  
> 不应使用该网络时命令配置或设置 Windows 时服务运行时的时间。  

此外，在较旧的计算机运行 Windows XP 或更早版本，命令网络时间 /querysntp 显示与之计算机配置为同步，但只有当计算机时客户端配置为 NTP 或放弃时使用该 NTP 服务器的网络时协议 (NTP) 服务器的名称。 由于已弃用该命令。  
  
大多数域成员计算机具有时间客户端类型的 NT5DS，这意味着它们域层次从的时间进行同步。 这仅典型例外是充当的森林根域中，通常配置为与外部时间源时间进行同步主域控制器 (PDC) 仿真器操作主机域控制器。 若要查看时间客户端配置的计算机，运行 W32tm//query /configuration 从提升的命令提示符启动 Windows Server 2008 和 Windows Vista 中的命令和阅读**类型**行中的命令输出。 有关详细信息，请参阅[Windows 时间服务的工作原理](https://go.microsoft.com/fwlink/?LinkId=117753)。 你可以运行命令**注册表查询 HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters**阅读的值和**NtpServer**命令输出。  
  
> [!IMPORTANT]  
> Windows Server 2016 中之前, W32Time 服务并非设计以满足时间敏感型应用程序需要。  但是，对 Windows Server 2016 更新现在允许你实施的 1ms 解决方案域中的准确性。  请参阅[Windows 2016 精确的时间](accurate-time.md)和[配置 Windows 时间为高准确性环境服务的支持边界](https://go.microsoft.com/fwlink/?LinkID=179459)详细信息。  
  
## <a name="w2k3tr_times_tools_dyax"></a>Windows 时服务工具  
以下工具是与 Windows 时间服务相关联。  
  
#### <a name="w32tmexe-windows-time"></a>W32tm.exe: Windows 时间  
**类别**  

作为 Windows XP、 Windows Vista、 Windows 7、 Windows Server 2003、 Windows Server 2003 R2、 Windows Server 2008 和 Windows Server 2008 R2 默认安装的一部分安装该工具。  
  
**版本兼容性**  
  
此工具适用于 Windows XP、 Windows Vista、 Windows 7、 Windows Server 2003，Windows Server 2003 R2、 Windows Server 2008 和 Windows Server 2008 R2 默认安装。  
  
W32tm.exe 用来配置 Windows 时服务设置。 此外可以用于诊断时间服务问题。 W32tm.exe 是配置、 监视或疑难解答 Windows 时间服务首选的命令行工具。  
  
下表介绍了均可与 W32tm.exe 参数。  
  
**W32tm.exe 主要参数**  
  
|参数|描述|  
|-------------|---------------|  
|W32tm /？|W32tm 命令行帮助|  
|W32tm//register|注册时间服务运行作为一项服务，并将默认配置添加对注册表。|  
|W32tm//unregister|注销时间服务，并从的注册表中删除配置的所有信息。|  
|w32tm//monitor<br /><br />[/domain:<domain name>] [/computers:<name>[，<name>[，<name>…]]][/Threads:<num>]|域-指定到监视器的域。 如果没有域名给定，或者指定简域以及计算机选项，则使用默认域。 此选项可能会使用多个一次。<br /><br />计算机-监视给定的计算机列表。 无空格逗号分隔计算机名称。 如果名称前缀使用 *，将其视为 PDC。 此选项可能会使用多个一次。<br /><br />线程-指定计算机分析同时数。 默认值为 3。 允许的范围内是 1-50。|  
|w32tm /ntte <NT time epoch>|在转换 NT 系统的时间 (10 ^-7) s 从 0 h 1-日 1601，较高的可读性格式的时间间隔。|  
|w32tm /ntpte <NTP time epoch>|在转换 NTP 时间，(2 ^-32) s 从 0 h 1-1900 年 1 月，较高的可读性格式的时间间隔。|  
|w32tm//resync<br /><br />[/computer:<computer>]<br /><br />[/nowait]<br /><br />[/Rediscover]<br /><br />[/Soft]|告诉的计算机，它应该重新其时钟尽快，所有累计的错误统计阻隔同步。<br /><br />计算机：<computer> -指定应该重新同步的计算机。 如果未指定，将重新本地计算机进行同步。<br /><br />不等待-不会等待再同步发生;立即返回。 否则，请等待以返回之前完成再同步。<br /><br />重新发现-重新检测网络配置并重新发现网络资源，然后重新同步。<br /><br />柔和-使用现有的错误统计重新进行同步。 很没有、 提供的兼容性。|  
|w32tm /stripchart<br /><br />/computer:<target><br /><br />[/Period:<refresh>]<br /><br />[/dataonly]<br /><br />[/Samples:<count>]<br/><br/>[/rdtsc]<br/>|显示之间这台计算机和另一台计算机的偏移条图表。<br /><br />计算机：<target> -计算机用于测量针对偏移。<br /><br />期间：<refresh> -之间样本秒钟的时间。 默认情况下 2 秒。<br /><br />dataonly-仅显示的数据不图形。<br /><br />示例：<count> -收集<count>样本，然后停止。 如果未指定，直到将收集样本**Ctrl + C**按下。<br/><br/>rdtsc： 对于每个示例中，此选项打印标题 RdtscStart，以及用逗号分隔值 RdtscEnd FileTime、 RoundtripDelay，而不是短信图形 NtpOffset。<br/><ul><li>[RdtscStart – RDTSC （Read 时间戳计数器）](https://en.wikipedia.org/wiki/Time_Stamp_Counter)值收集之前 NTP 请求生成。</li><li>RdtscEnd – RDTSC （Read 时间戳计数器） 值收集只需后 NTP 响应接收和处理。</li><li>FileTime – NTP 请求中使用的本地 FILETIME 值。</li><li>RoundtripDelay – 时间秒之间 NTP 请求生成经过和处理已收到的 NTP 响应，按照 NTP 往返计算计算。</li><li>按照 NTP 对方计算计算 NTPOffset – 时间在本地计算机和 NTP 服务器，秒钟的时间偏移。</li></ul>|
|w32tm /config<br /><br />[/computer:<target>]<br /><br />[/Update]<br /><br />[/manualpeerlist:<peers>]<br /><br />[/syncfromflags:<source>]<br /><br />[/LocalClockDispersion:<seconds>]<br /><br />[/Reliable: （是 & #124; 无)]<br /><br />[/largephaseoffset:<milliseconds>]|计算机：<target> -调整的配置<target>。 如果未指定，默认为在本地计算机。<br /><br />更新-通知时间服务已更改的配置，使更改生效。<br /><br />manualpeerlist:<peers> -设置在手动等列表<peers>，即 DNS 和/或 IP 地址的分隔空间的列表。 指定多个等时, 必须名言用此选项。<br /><br />syncfromflags:<source> -设置 NTP 客户端从应该同步哪些来源。 <source> 应逗号分隔这些关键字齐名 （不区分大小写）：<br /><br />手动-包含等从列表中手动等。<br /><br />DOMHIER-从域层次中域控制器 (DC) 同步。<br /><br />LocalClockDispersion:<seconds> -配置内部时钟假定 W32Time 将时，它无法从其配置源获得时间的准确性。<br /><br />可靠: (是 & #124; 无)-设置此计算机是否可靠的时间源。<br /><br />此设置才有意义的域控制器上。<br /><br />-此计算机是可靠的时间服务。<br /><br />-此计算机不对可靠的时间服务。<br /><br />largephaseoffset:<milliseconds> -设置的时间区别本地网络次这 W32Time 将考虑峰值和。|  
|w32tm /tz|显示当前时区的区域设置。|  
|w32tm /dumpreg<br /><br />[/Subkey:<key>]<br /><br />[/computer:<target>]|显示给定的注册表项的相关联的值。<br /><br />默认该键 HKLM\System\CurrentControlSet\Services\W32Time<br /><br />（时间服务根密钥）。<br /><br />子项：<key> -显示与子项<key>的默认密钥。<br /><br />计算机：<target> -查询注册表计算机设置 <target>|  
|w32tm//query [/computer:<target>] {/source & #124;/configuration & #124;/Peers & #124;/status} [/verbose]|此参数首次进行 Windows 时间客户端版本的 Windows Vista 和 Windows Server 2008 中。<br /><br />显示的计算机 Windows 时间服务的信息。<br /><br />**计算机：<target> ** -查询的信息** <target> **。 如果未指定，默认值为本地计算机。<br /><br />**源**的显示时间源。<br /><br />**配置**-显示运行的时和设置的来自的配置。 详细的模式，在显示定义或未使用设置太。<br /><br />**等**-显示等及其状态的列表。<br /><br />**状态**-显示 Windows 时间服务状态。<br /><br />**详细**-设置要显示的详细信息的详述模式。|  
|w32tm//debug {/disable & #124;{/Enable /file:<name> /size:<bytes> /entries:<value> [/truncate]}}|此参数首次进行 Windows 时间客户端版本的 Windows Vista 和 Windows Server 2008 中。<br /><br />启用或禁用本地计算机 Windows 时间服务专用日志。<br /><br />**禁用**-禁用专用日志。<br /><br />**启用**-启用专用日志。<br /><br />-   **文件：<name> ** -指定绝对文件的名称。<br />-   **大小：<bytes> ** -指定循环记录的最大大小。<br />-   **条目：<value> ** -包含标志，由号指定和逗号指定应登录的信息的类型的分隔的列表。 有效的数字都 0 到 300。 数字各种是有效，除了一个数字，如 0-100,103 106。 值 0 300 是用于登录的所有信息。<br /><br />**截断**-如果存在截断该文件。|  
  
有关详细信息**W32tm.exe**，请参阅帮助和支持中心在 Windows XP、 Windows Vista、 Windows 7、 Windows Server 2003、 Windows Server 2003 R2、 Windows Server 2008 和 Windows Server 2008 R2。  
  
## <a name="w2k3tr_times_tools_uhlp"></a>Windows 时间 Service 注册表项  
以下注册表项是与 Windows 时间服务相关联。  
  
此信息提供为参考疑难解答或验证所需的设置将应用中使用。 建议你执行不直接编辑注册表除非没有没有其他选择。 对注册表进行修改不验证注册表编辑器中或通过 Windows 应用，并可以存储错误值的结果之前。 这可以无法恢复错误导致系统中。  
  
如果可能，请使用组策略或其他 Windows 工具，例如 Microsoft 管理控制台 (MMC) 来完成任务，而不是编辑注册表直接。 如果你必须编辑注册表，使用格外小心。  
  
> [!WARNING]  
> 一些不同于相应的默认注册表项组策略对象 (GPO) 设置为在系统管理模板文件 (System.adm) 配置预设值。 如果您计划使用 GPO 配置任何 Windows 时间设置，请确保你查看[Windows 时间服务组策略设置的预设值会从相应 Windows 时间 service 注册表项在 Windows Server 2003 不同](https://go.microsoft.com/fwlink/?LinkId=186066)。 此问题对 Windows Server 2008 R2、 Windows Server 2008、 Windows Server 2003 R2 和 Windows Server 2003。  
  
许多 Windows 时间送修的注册表项是一样具有相同名称的组策略设置。 组策略设置对应于居住在具有相同名称的注册表项：  
  
>**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\\**

  
在注册表位置有多个注册表项。 Windows 时间设置的所有这些键存入值：
* [参数](#Parameters)
* [配置](#Configuration)
* [NtpClient](#NtpClient)
* [NtpServer](#NtpServer)
  
> [!NOTE]  
> 许多注册表 W32Time 部分中的值用于内部通过 W32Time 存储的信息。 更改这些值应该不会手动在任何时间。 除非您有熟悉的设置，并且某些新值将按预期方式工作，不要修改任何在此部分中的设置。 以下注册表项位于下：

>>**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\\**  
  
某些参数存储在注册表中时钟周期，并且某些位于秒。 将时间从时钟周期转换为秒：  
  
-   1 分钟 = 60 秒  
  
-   1 秒 = 1000 ms  
  
-   1 ms = 10000 时钟周期上的 Windows 系统，述在[DateTime.Ticks 属性](https://msdn.microsoft.com/en-us/library/system.datetime.ticks.aspx)。  
  
例如，5 分钟会变得 5 * 60\ * 1000\ * 10000 = 3000000000 时钟周期。 

包含所有版本的 Windows 7、 Windows 8、 Windows 10、 Windows Server 2008 和 Windows Server 2008 R2、 Windows Server 2012、 Windows Server 2012R2 Windows Server 2016。  某些项是仅适用于较新的 Windows 版本。


#### <a name="Parameters"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Parameters

|注册表项|版本|描述|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|所有|此项指示非标准模式组合允许在等之间同步。 对于域成员默认值为 1。 独立客户端和服务器的默认值为 1。|
|NtpServer|所有|此项指定的同事从中计算机获得时间戳组成的一个或多个 DNS 名称或每行的 IP 地址的分隔空间的列表。 必须唯一每个 DNS 名称或列出的 IP 地址。 连接到某个域中的计算机必须与更可靠的时间源，如官方美国时间时钟进行同步。  <ul><li>0x01 SpecialInterval </li><li>0x02 UseAsFallbackOnly</li><li>0x04 SymmetricActive-有关该模式的详细信息，请参阅[Windows 时间 Server: 3.3 模式的操作](https://go.microsoft.com/fwlink/?LinkId=208012)。</li><li>0x08 客户端</li></ul><br />没有为此域成员上的注册表项默认值。 独立客户端和服务器上的默认值为 time.windows.com，0x1。<br /><br />注意： 可 NTP 服务器上的详细信息，请参阅[Microsoft 知识库文章 262680-Internet 有简单网络协议时间 (SNTP) 的时间服务器的列表](https://go.microsoft.com/fwlink/?LinkId=186067)|
|ServiceDll|所有|此项由 W32Time 维护。 它包含保留的 Windows 操作系统、 所使用的数据，并任何更改此设置可能会导致无法预测结果。 此 DLL 域成员和独立客户端和服务器上的默认位置是 %windir%\system32\w32time.dll。  |
|ServiceMain|所有|此项由 W32Time 维护。 它包含保留的 Windows 操作系统、 所使用的数据，并任何更改此设置可能会导致无法预测结果。 域成员上的默认值为 SvchostEntry_W32Time。 独立客户端和服务器上的默认值为 SvchostEntry_W32Time。  "|
|键入|所有|指示哪同级接受从同步此项：  <ul><li>**非同步**。 与其他来源时间服务不会同步。</li><li>**NTP。** 从中指定的服务器同步时间服务**NtpServer**。 注册表项。</li><li>**NT5DS**。 从域分层同步时间服务。  </li><li>**放弃**。 时间服务使用的所有可用的同步架。  </li></ul>域成员上的默认值**NT5DS**。 独立客户端和服务器上的默认值**NTP**。   |

#### <a name="Configuration"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Config

|注册表项|版本|描述|
|------------------------------------|---------------|----------------------------|
|AnnounceFlags|所有|此项控制是否为可靠的时间服务器标记此计算机。 一台计算机未标记为可靠除非它还标志着作为时间服务器。<br /> -0x00 时间服务器  <br /> -0x01 始终时间服务器  <br /> -0x02 自动时间服务器  <br /> -0x04 始终可靠的时间 server  <br /> -0x08 自动可靠的时间 server  <br />对于域成员默认值为 10。 独立客户端和服务器的默认值为 10。|
|EventLogFlags|所有|此项控制时间服务记录的事件。  <br />-时间跳转： 0x1  <br />-源更改： 0x2  <br />域成员上的默认值为 2。 独立客户端和服务器上的默认值为 2。  |
|FrequencyCorrectRate|所有|此项控制的速率时钟为止。 如果此值为太小，时钟不稳定也 overcorrects。 如果值太大，时钟需要很长的时间进行同步。 域成员上的默认值为 4。 独立客户端和服务器上的默认值为 4。  <br /><br />请注意，0 FrequencyCorrectRate 注册表项的值无效。 在 Windows Server 2003，Windows Server 2003 R2、 Windows Server 2008 和 Windows Server 2008 R2 计算机上，如果值设置为 0 Windows 时间服务会自动将其更改为 1。  |
|HoldPeriod|所有|此项控制峰值检测禁用以便快速将本地时钟带入同步的时间。 峰值是时间示例指示那时已关闭数秒钟后始终已退回大好时机示例, 通常会收到。 域成员上的默认值为 5。 独立客户端和服务器上的默认值为 5。  |
|LargePhaseOffset|所有|此项指定的时间偏移大于或等于 10 中的此值<sup>-7</sup>秒被认为是峰值。 如有大量的交通网络中断可能会导致峰值。 如果问题仍然存在长时间，将忽略峰值。 域成员上的默认值为 50000000。 独立客户端和服务器上的默认值为 50000000。  |
|LastClockRate|所有|此项由 W32Time 维护。 它包含保留的 Windows 操作系统、 所使用的数据，并任何更改此设置可能会导致无法预测结果。 域成员上的默认值为 156250。 独立客户端和服务器上的默认值为 156250。  |
|LocalClockDispersion|所有|此项控制您必须在何时发挥 （在秒钟内） 分散仅时间源内置 CMOS 时钟。 域成员上的默认值为 10。 独立客户端和服务器上的默认值为 10。|
|MaxAllowedPhaseOffset|所有|此项指定尝试使用时钟率调整计算机时钟 W32Time 最大偏移 （在秒）。 当偏移超过此率时，W32Time 直接设置计算机时钟。 对于域成员默认值为 300。 独立客户端和服务器的默认值为 1。  [有关详细信息，请见下文](#MaxAllowedPhaseOffset)。|
|MaxClockRate|所有|此项由 W32Time 维护。 它包含保留的 Windows 操作系统、 所使用的数据，并任何更改此设置可能会导致无法预测结果。 域成员默认值为 155860。 独立客户端和服务器的默认值为 155860。  |
|MaxNegPhaseCorrection|所有|此项指定秒后，该服务使最大的负面时间更正。 如果该服务确定大于这一更改所需，它将改为记录事件。 特殊的用例： 0xFFFFFFFF 表示始终校准时间。 域成员默认值为 0xFFFFFFFF。 默认值独立客户端和服务器是 54000 (15 小时)。  |
|MaxPollInterval|所有|此项指定允许系统轮询间隔最大的时间间隔，log2 秒。 请注意，系统必须轮询根据计划的时间间隔时, 提供商可以拒绝生成样本请求执行此操作时。 对于域控制器的默认值为 10。 对于域成员默认值为 15。 独立客户端和服务器的默认值为 15。  |
|MaxPosPhaseCorrection|所有|此项指定秒后，该服务使最大的正时间更正。 如果该服务确定大于这一更改所需，它将改为记录事件。 特殊的用例： 0xFFFFFFFF 表示始终校准时间。 域成员默认值为 0xFFFFFFFF。 默认值独立客户端和服务器是 54000 (15 小时)。  |
|MinClockRate|所有|此项由 W32Time 维护。 它包含保留的 Windows 操作系统、 所使用的数据，并任何更改此设置可能会导致无法预测结果。 域成员默认值为 155860。 独立客户端和服务器的默认值为 155860。  |
|MinPollInterval|所有|此项 log2 秒系统轮询间隔允许指定小的时间间隔。 请注意时，系统通常比这不会请求样本，提供商可以产生样本有时以外计划的时间间隔。 对于域控制器的默认值为 6。 对于域成员默认值为 10。 独立客户端和服务器的默认值为 10。  |
|PhaseCorrectRate|所有|此项控制从该处更正阶段错误的速度。 指定较小的值快速，更正阶段错误，但是可能会导致时钟变得不稳定。 如果的值为太大，它会拍摄较长时间才能改正阶段错误。 <br /><br />域成员上的默认值为 1。 独立客户端和服务器上的默认值为 7。<br /><br />注意： 0 是 PhaseCorrectRate 注册表项的值无效。 在 Windows Server 2003、 Windows Server 2003 R2、 Windows Server 2008、 和 Windows Server 2008 R2 计算机上，如果值设置为 0，Windows 时间服务会自动变为它 1。  |
|PollAdjustFactor|所有|此项控制决策，以增加或减少系统轮询时间间隔。 值越大，导致减少将轮询间隔错误的小量。 域成员上的默认值为 5。 独立客户端和服务器上的默认值为 5。 |
|SpikeWatchPeriod|所有|此项指定的时间，必须在保持可疑偏移之前被视为正确 （在秒）。 域成员上的默认值为 900。 独立客户端和工作站上的默认值为 900。  |
|TimeJumpAuditOffset|所有|未签名的表示指示时间跳转审核 threshold，秒。 如果时间服务通过直接，设置时钟调整本地时钟时间更正了多个此值，然后时间服务日志审核事件。|
|UpdateInterval|所有|此项指定时钟间隔阶段更正调整数。 对于域控制器的默认值为 100。 对于域成员默认值为 30000。 独立客户端和服务器的默认值为 360000。  <br /><br />**注意**： 零是 UpdateInterval 注册表项的值无效。 在运行 Windows Server 2003，Windows Server 2003 R2、 Windows Server 2008 和 Windows Server 2008 R2、 计算机上如果值设置为 0 Windows 时间服务会自动变为它 1。<br /><br />以下三种注册表项并非 W32Time 默认配置的一部分，但可以添加到注册表以获得更日志记录功能。 通过更改在组策略对象编辑器 EventLogFlags 设置，可以进行修改到系统事件日志记录的信息。 默认情况下，时间服务创建日志事件查看器在每次其切换到新时间的源。<br /><br />**警告**： 预设配置系统管理模板文件 (System.adm) 中的组策略对象 (GPO) 设置不同于相应的值一些默认的注册表项。 如果您计划使用 GPO 配置任何 Windows 时间设置，请确保你查看[Windows 时间服务组策略设置的预设值会从相应 Windows 时间 service 注册表项在 Windows Server 2003 不同](https://go.microsoft.com/fwlink/?LinkId=186066)。 此问题对 Windows Server 2008 R2、 Windows Server 2008、 Windows Server 2003 R2 和 Windows Server 2003。 |
|UtilizeSslTimeData|发布 Windows 10 版本 1511|输入 1 表示 W32Time 将使用多个 SSL 时间戳种子 grossly 不准确时钟。|

必须以启用 W32Time 日志记录添加以下注册表项：  

|注册表项|版本|描述|
|------------------------------------|---------------|----------------------------|
|FileLogEntries|所有|此项控制量创建 Windows 时间日志文件中的条目。 默认值为 none，这不会记录任何 Windows 时间活动。 有效的值为 0 到 300。 此值将不会影响事件日志条目通常由 Windows 时间|
|FileLogName|所有|此项控制位置和文件名的 Windows 时间日志。 默认值为空和应该不会更改，除非**FileLogEntries**发生了更改。 有效的值为完整路径和 Windows 时将使用它来创建日志文件中的文件名。 此值不会影响通常由 Windows 时间事件日志条目。  |
|FileLogSize|所有|此项控制 Windows 时间日志文件的圆形日志记录行为。 当**FileLogEntries**和**FileLogName**是定义，此项定义大小，以字节允许新计算机条目覆盖古老日志条目前联系日志文件。 任何正号码有效，并且 3000000 建议。 此值不会影响通常由 Windows 时间事件日志条目。  |



#### <a name="NtpClient"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient

|注册表项|版本|描述|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|所有|此项指示非标准模式组合允许在等之间同步。 对于域成员默认值为 1。 独立客户端和服务器的默认值为 1。|
|CompatibilityFlags|所有|此项指定以下兼容性标志和值： <br /><br />-DispersionInvalid: 0x00000001  <br />-IgnoreFutureRefTimeStamp: 0x00000002  <br /> -AutodetectWin2K: 0x80000000  <br />-AutodetectWin2KStage2: 0x40000000  <br /><br />域成员默认值为 0x80000000。 独立客户端和服务器的默认值为 0x80000000。  |
|CrossSiteSyncFlags|所有|此项决定是否服务选择同步合作伙伴之外计算机的域。 选项和值是：  <br /><br />-无： 0  <br />-PdcOnly: 1  <br />-所有： 2  <br /><br />如果未设置 NT5DS 值，则忽略此值。 对于域成员默认值为 2。 独立客户端和服务器的默认值为 2。  |
|Dll 名称|所有|此项指定时间服务提供商 DLL 的位置。  <br /><br />此 DLL 域成员和独立客户端和服务器上的默认位置是 %windir%\system32\w32time.dll。|
|启用|所有|此项指示 NtpClient 提供商是否启用当前时间服务中。  <br /><ul><li>是 1</li><li>否 0</li></ul>域成员上的默认值为 1。 独立客户端和服务器上的默认值为 1。|
|EventLogFlags|所有|此项指定由 Windows 时间服务记录的事件。<ul><li>0x1 可访问性的更改</li><li>0x2 大示例扭曲 （这是适用于 Windows Server 2003、 Windows Server 2003 R2、 Windows Server 2008 和 Windows Server 2008 R2 仅）</li></ul>域成员上的默认值为 0x1。 独立客户端和服务器上的默认值为 0x1。|
|InputProvider|所有|此项指示 NtpClient 提供商是否已启用。  <ul><li>是 1  </li><li>否 0 </li></ul>域成员上的默认值为 1。 独立客户端和服务器上的默认值为 1。  |
|LargeSampleSkew|所有|此项指定大示例倾斜用于登录秒。 为遵守规范安全和委员会 （秒），这应该设置到 3 秒钟。 仅当 EventLogFlags 明确配置为扭曲 0x2 大示例，将记录事件针对此设置。 域成员上的默认值为 3。 独立客户端和服务器上的默认值为 3。  |
|ResolvePeerBackOffMaxTimes|所有|此项指定为双等待间隔时重复最大次数尝试查找等与无法进行同步。 值为零意味着等待间隔始终是最低。 域成员上的默认值为 7。 独立客户端和服务器上的默认值为 7。 |
|ResolvePeerBackoffMinutes|所有|此项指定初始的时间间隔等待，分钟，然后再尝试查找等与进行同步。 域成员上的默认值为 15。 独立客户端和服务器上的默认值为 15。  |
|SpecialPollInterval|所有|此项中手动等秒钟指定的特殊轮询间隔。 启用 SpecialInterval 0x1 标志后，W32Time 使用此轮询时间间隔，而不是轮询间隔确定由操作系统。 域成员上的默认值为 3600。 独立客户端和服务器上的默认值都是 604800。<br/><br/>新版本 1702年，SpecialPollInterval 包含的 MinPollInterval 和 MaxPollInterval 配置注册表值。|
|SpecialPollTimeRemaining|所有|此项由 W32Time 维护。 它包含保留由 Windows 操作系统的数据。 它在秒后，再重新启动计算机后，将重新同步 W32Time 指定的时间。 对此设置的任何更改可能会导致无法预测结果。 在这两个域成员和独立客户端和服务器上的默认值为空。  |

#### <a name="NtpServer"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer

|注册表项|版本|描述|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|所有|此项指示非标准模式组合允许在客户端和服务器之间同步。 对于域成员默认值为 1。 独立客户端和服务器的默认值为 1。|
|Dll 名称|所有|此项指定时间服务提供商 DLL 的位置。<br /><br />此 DLL 域成员和独立客户端和服务器上的默认位置是 %windir%\system32\w32time.dll。  |
|启用|所有|此项指示 NtpServer 提供商是否启用当前时间服务中。 <ul><li>是 1</li><li>否 0</li></ul>域成员上的默认值为 1。 独立客户端和服务器上的默认值为 1。  |
|InputProvider|所有|此项指示 NtpServer 提供商是否已启用。  <ul><li>是 1  </li><li>否 0 </li></ul>域成员上的默认值为 1。 独立客户端和服务器上的默认值为 1。  |

#### <a name="MaxAllowedPhaseOffset"></a>MaxAllowedPhaseOffset 信息
为了使 W32Time 逐渐设置的计算机时钟，偏移必须小于 MaxAllowedPhaseOffset 值和满足以下公式同时：  
```  
|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2  
``` 
CurrentTimeOffset 的度量单位时钟周期，其中 1ms = 10000 时钟 Windows 系统上的周期。  
  
SystemClockRate 和 PhaseCorrectRate 还以时钟周期。 若要获取 SystemClockRate，你可以使用以下命令，并将其转换秒为时钟使用秒方程式周期 * 1000\ * 10000:  
  
```  
W32tm /query /status /verbose  
ClockRate: 0.0156000s  
```  
  
SystemclockRate 是在系统时钟率。 以 156000 秒为例，SystemclockRate 将会 = 0.0156000 * 1000 \ * 10000 = 156000 时钟周期。  
  
MaxAllowedPhaseOffset 也是秒。 将其为时钟周期转换，乘 MaxAllowedPhaseOffset * 1000\ * 10000。  
  
以下两个的示例显示了如何将应用  
  
**示例 1**： 相差 4 分钟的时间 （例如，你的时间上午 11:05 和时间示例收到从等并且相信的节日正确是 11:09 上午）。  
```
phasecorrectRate = 1  
  
UpdateInterval = 30000 (clock ticks)  
  
systemclockRate = 156000 (clock ticks)  
  
MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  
  
|currentTimeOffset| = 4mins = 4*60\*1000\*10000 = 2400000000 ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
2400000000 < 6000000000 = TRUE  
```
并且它是否满足上述方程式？ 
```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2)  
  
Is 2,400,000,000 / (30000*1) < 156000/2  
  
Is 100,000 < 78,000  
  
NO/FALSE  
```  
因此 W32tm 会时钟重新设置立即。  
  
> [!NOTE]  
> 在此情况下，如果你想要缓慢重新设置时钟，你将需要调整 PhaseCorrectRate 或的注册表中的 updateInterval 以及以确保在 TRUE 方程式结果值。  
  
**示例 2**： 相差 3 分钟的时间。  
```  
phasecorrectRate = 1  
  
UpdateInterval = 30000 (clock ticks)  
  
systemclockRate = 156000 (clock ticks)  
  
MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  
  
currentTimeOffset = 3mins = 3*60\*1000\*10000 = 1800000000 clock ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
1800000000 < 6000000000 = TRUE  
```  
并且它是否满足上述方程式？
```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2)  
  
Is 3 mins (1,800,000,000) / (30000*1) < 156000/2  
  
Is 60,000 < 78,000  
  
YES/TRUE  
```  
在此情况下时钟将设置重新缓慢。  
  
## <a name="w2k3tr_times_tools_vwtt"></a>Windows 时间 Service 组策略设置  
你可以通过使用组策略对象编辑器配置大多数 W32Time 参数。 这包括计算机配置为不可靠时间源 NTPServer 或 NTPClient，配置时同步机制，并配置一台计算机。  
  
> [!NOTE]  
> 组策略设置 Windows 次可以配置 Windows Server 2003，Windows Server 2003 R2、 Windows Server 2008、 服务和 Windows Server 2008 R2 域控制器，可将只向运行 Windows Server 2003，Windows Server 2003 R2、 Windows Server 2008 和 Windows Server 2008 R2 的计算机。  
  
你可以找到用来配置 W32Time 在以下位置中的组策略对象编辑器贴靠的组策略设置：  
  
-   计算机配置 Templates\System\Windows 时间服务  
  
    配置**全球配置设置**此处。  
  
-   计算机配置 Templates\System\Windows 时间 Service\Time 提供程序  
  
    配置**Windows NTP 客户端**此处的设置。  
  
    启用**Windows NTP 客户端**此处。  
  
    启用**Windows NTP Server**此处。  
  
> [!WARNING]  
> 一些不同于相应的默认注册表项组策略对象 (GPO) 设置为在系统管理模板文件 (System.adm) 配置预设值。 如果您计划使用 GPO 配置任何 Windows 时间设置，请确保你查看[Windows 时间服务组策略设置的预设值会从相应 Windows 时间 service 注册表项在 Windows Server 2003 不同](https://go.microsoft.com/fwlink/?LinkId=186066)。 此问题对 Windows Server 2008 R2、 Windows Server 2008、 Windows Server 2003 R2 和 Windows Server 2003。  
  
下表列出了 Windows 时间服务和每个设置与关联的预定的值相关联的全球的组策略设置。 有关每个设置的详细信息，请参阅相应的注册表项中"[Windows 时间 Service 注册表项](#w2k3tr_times_tools_uhlp)"前面的本主题。 以下设置包含一个名为 GPO 在**全球配置设置**。  
  
**全球组策略设置与 Windows 时间**  
  
|组策略设置|预先设置的值|  
|------------------------|------------------|  
|AnnounceFlags|10|  
|EventLogFlags|2|  
|FrequencyCorrectRate|4|  
|HoldPeriod|5|  
|LargePhaseOffset|1280000|  
|LocalClockDispersion|10|  
|MaxAllowedPhaseOffset|300|  
|MaxNegPhaseCorrection|54000 （15 小时）|  
|MaxPollInterval|15|  
|MaxPosPhaseCorrection|54000 （15 小时）|  
|MinPollInterval|10|  
|PhaseCorrectRate|7|  
|PollAdjustFactor|5|  
|SpikeWatchPeriod|90|  
|UpdateInterval|100|  
  
下表列出了可用的设置**配置 Windows NTP 客户端**GPO 和预先设置与 Windows 时间服务相关联的值。 有关每个设置的详细信息，请参阅相应的注册表项中"[Windows 时间 Service 注册表项](#w2k3tr_times_tools_uhlp)"前面的本主题。  
  
**NTP 客户端组策略设置与 Windows 时间**  
  
|组策略设置|默认值|  
|------------------------|-----------------|  
|NtpServer|time.windows.com 0x1|  
|键入|默认选项：<br /><br />-   **NTP。** 在未加入域的计算机上使用。<br />-   **NT5DS。** 加入域的计算机上使用。|  
|CrossSiteSyncFlags|2|  
|ResolvePeerBackoffMinutes|15|  
|ResolvePeerBackoffMaxTimes|7|  
|SpecialPollInterval|3600|  
|EventLogFlags|0|  
  
## <a name="w2k3tr_times_tools_suxb"></a>使用 Windows 时服务的网络端口  
Windows 时间遵循 NTP 规范，需要使用 UDP 端口 123 所有时间同步通信。 始终保持保留和 Windows 时间保留该端口。 每当计算机同步其时钟或提供时间到另一台计算机时，该通信 UDP 端口 123 上执行。  
  
> [!NOTE]  
> 如果你有一台计算机 （也称为多主计算机） 的多个网络适配器，您不能选择性启用 Windows 时间服务根据该网络适配器。  
  
## <a name="w2k3tr_times_tools_qoep"></a>相关的信息  
以下资源包含相关的此部分中的其他信息。  
  
-   RFC *1305年*IETF RFC 数据库中  

