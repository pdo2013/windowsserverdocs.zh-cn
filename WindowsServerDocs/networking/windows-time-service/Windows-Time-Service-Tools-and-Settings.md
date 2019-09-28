---
ms.assetid: 6086947f-f9ef-4e18-9f07-6c7c81d7002c
title: Windows 时间服务工具和设置
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 10/16/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 84d10c30d42e92d325475a1143c85acce7456a80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395680"
---
# <a name="windows-time-service-tools-and-settings"></a>Windows 时间服务工具和设置
>适用于：Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows 10 或更高版本

在本主题中，你将了解有关 Windows 时间服务（W32Time）的工具和设置。 

如果只想同步已加入域的客户端计算机的时间，请参阅[为自动域时间同步配置客户端计算机](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)。 有关如何配置 Windows 时间服务的其他主题，请参阅[在何处查找 Windows 时间服务配置信息](https://docs.microsoft.com/windows-server/networking/windows-time-service/windows-time-service-top)。  

> [!CAUTION]  
> 不应使用 Net time 命令来配置或设置 Windows 时间服务运行的时间。  
>
> 此外，在运行 Windows XP 或更早版本的较早计算机上，命令 Net time/querysntp 显示计算机配置为进行同步的网络时间协议（NTP）服务器的名称，但仅当计算机时间客户端配置为 NTP 或 AllSync。 该命令已被弃用。  

大多数域成员计算机的时间客户端类型为 NT5DS，这意味着它们会同步域层次结构中的时间。 唯一的例外是，域控制器充当目录林根级域的主域控制器（PDC）仿真器操作主机，通常配置为使用外部时间源同步时间。 若要查看计算机的时间客户端配置，请从 Windows Server 2008 和 Windows Vista 中启动的提升的命令提示符处运行 W32tm/query/配置命令，并在命令输出中读取**类型**行。 有关详细信息，请参阅[Windows 时间服务的工作原理](https://docs.microsoft.com/windows-server/networking/windows-time-service/How-the-Windows-Time-Service-Works)。 可以运行命令**reg query HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters** ，并在命令输出中读取**NtpServer**的值。  

> [!IMPORTANT]  
> 在 Windows Server 2016 之前，W32Time 服务并未设计为满足区分时间的应用程序需要。  但是，Windows Server 2016 的更新现在允许你在域中实施1ms 准确性解决方案。  有关详细信息，请参阅[windows 2016 准确时间](accurate-time.md)和[支持边界，为高准确性环境配置 windows 时间服务](https://docs.microsoft.com/windows-server/networking/windows-time-service/support-boundary)。  

## <a name="windows-time-service-tools"></a>Windows 时间服务工具  
以下工具与 Windows 时间服务相关联。  

#### <a name="w32tmexe-windows-time"></a>W32tm：Windows 时间  
**类别**  

此工具作为 Windows XP、Windows Vista、Windows 7、Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 默认安装的一部分进行安装。  

**版本兼容性**  

此工具适用于 Windows XP、Windows Vista、Windows 7、Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 默认安装。  

W32tm 用于配置 Windows 时间服务设置。 它还可用于诊断时间服务的问题。 W32tm 是用于配置、监视或排查 Windows 时间服务的首选命令行工具。  

下表描述了与 W32tm 一起使用的参数。  

**W32tm 主参数**  


|                                                                                                                                  参数                                                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                                                                                   W32tm/？                                                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      W32tm 命令行帮助                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                               W32tm/register                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  将时间服务注册为作为服务运行，并将默认配置添加到注册表。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                                                                                                                              W32tm/unregister                                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     注销时间服务并从注册表中删除所有配置信息。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                                                                                 w32tm/monitor<br /><br />[/domain： @no__t][/computers： <name> [，<name> [，<name> ...]]][/threads： @no__t]                                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                 域-指定要监视的域。 如果未指定域名，或者没有指定域或计算机选项，则使用默认域。 此选项可能多次使用。<br /><br />计算机-监视给定的计算机列表。 计算机名称以逗号分隔，不含空格。 如果名称以 "\*" 为前缀，则将其视为 PDC。 此选项可能多次使用。<br /><br />线程-指定要同时分析的计算机数。 默认值为 3。 允许的范围为1-50。                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                                                                         w32tm/ntte <NT time epoch>                                                                                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   将一个 NT 系统时间（从 0h 1 到1601年1月1日）的时间间隔转换为可读格式。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|                                                                                                                        w32tm/ntpte <NTP time epoch>                                                                                                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      将（2 ^-32）秒间隔中的 NTP 时间转换为1900年1月1日到可读格式的0h。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                                               w32tm/resync<br /><br />[/computer： @no__t]<br /><br />/nowait<br /><br />[/rediscover]<br /><br />[/soft]                                                                               |                                                                                                                                                                                                                                                                                                                                                                      告知计算机应该尽快重新同步其时钟，并丢弃所有累积的错误统计信息。<br /><br />计算机： <computer>-指定应重新同步的计算机。 如果未指定，则本地计算机会重新同步。<br /><br />nowait-不等待重新同步，立即返回。 否则，请等待重新同步完成，然后再返回。<br /><br />重新发现-重新检测网络配置并重新发现网络源，然后重新同步。<br /><br />使用现有错误统计信息进行软重新同步。 由于兼容性而提供。                                                                                                                                                                                                                                                                                                                                                                      |
|                                                          w32tm/stripchart<br /><br />/computer： <target><br /><br />[/period： @no__t]<br /><br />[/dataonly]<br /><br />[/samples： @no__t]<br/><br/>[/rdtsc]<br/>                                                          |                            显示此计算机和另一台计算机之间的偏移图。<br /><br />计算机： <target>-要测量其偏移量的计算机。<br /><br />period： <refresh>-样本之间的时间（以秒为单位）。 默认值为2秒。<br /><br />dataonly-仅显示不含图形的数据。<br /><br />示例： <count>-收集 @no__t 样本，然后停止。 如果未指定，则将收集示例，直到按下**Ctrl + C** 。<br/><br/>rdtsc：对于每个示例，此选项会将逗号分隔值连同标头 RdtscStart、RdtscEnd、FileTime、RoundtripDelay、NtpOffset 而不是文本图形一起打印。<br/><ul><li>RdtscStart –在生成 NTP 请求之前收集的[RDTSC （读取时间戳计数器）](https://en.wikipedia.org/wiki/Time_Stamp_Counter)值。</li><li>RdtscEnd – RDTSC （读取时间戳计数器）在接收和处理 NTP 响应之后收集的值。</li><li>FileTime – NTP 请求中使用的本地 FILETIME 值。</li><li>RoundtripDelay-生成 NTP 请求和处理收到的 NTP 响应之间经过的时间（以秒为单位），按 NTP 往返计算计算。</li><li>NTPOffset-本地计算机与 NTP 服务器之间的时间偏移量（以秒为单位），按 NTP 偏移量计算得出。</li></ul>                             |
| w32tm/config<br /><br />[/computer： @no__t]<br /><br />/update<br /><br />[/manualpeerlist： @no__t]<br /><br />[/syncfromflags： @no__t]<br /><br />[/LocalClockDispersion： @no__t]<br /><br />[/reliable：（YES&#124;NO）]<br /><br />[/largephaseoffset： @no__t] | 计算机： <target>-调整 @no__t 的配置。 如果未指定，则默认为本地计算机。<br /><br />update-通知时间服务配置已更改，使更改生效。<br /><br />manualpeerlist： <peers>-将手动对等列表设置为 <peers>，它是以空格分隔的 DNS 和/或 IP 地址的列表。 指定多个对等方时，此选项必须用引号引起来。<br /><br />syncfromflags： @no__t-设置 NTP 客户端应同步的源。 <source> 应为这些关键字的逗号分隔列表（不区分大小写）：<br /><br />手动-包括手动对等列表中的对等方。<br /><br />DOMHIER-从域层次结构中的域控制器（DC）进行同步。<br /><br />LocalClockDispersion： <seconds>-配置 W32Time 在无法从其配置的源中获取时间时将采用的内部时钟的准确性。<br /><br />可靠：（YES&#124;NO）-设置此计算机是否是可靠时间源。<br /><br />此设置仅对域控制器有意义。<br /><br />是-此计算机是可靠的时间服务。<br /><br />否-此计算机不是可靠的时间服务。<br /><br />largephaseoffset： @no__t-设置 W32Time 将考虑峰值的本地时间和网络时间之间的时间差。 |
|                                                                                                                                  w32tm/tz                                                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              显示当前时区设置。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|                                                                                                  w32tm/dumpreg<br /><br />[/subkey： @no__t]<br /><br />[/computer： @no__t]                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                显示与给定注册表项相关联的值。<br /><br />默认密钥为 HKLM\System\CurrentControlSet\Services\W32Time<br /><br />（时间服务的根键）。<br /><br />子项： <key>-显示与默认键的子项 <key> 关联的值。<br /><br />计算机： <target>-查询计算机的注册表设置 <target>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|                                                                                  w32tm/query [/computer： <target>] {/source &#124; /配置&#124; /peers &#124; /status} [/verbose]                                                                                   |                                                                                                                                                                                                                                                                                                                  此参数首先在 windows Vista 的 Windows 时间客户端版本和 Windows Server 2008 中提供。<br /><br />显示计算机的 Windows 时间服务信息。<br /><br />**计算机： <target>** -查询 **@no__t**的信息。 如果未指定，则默认值为本地计算机。<br /><br />**源**-显示时间源。<br /><br />**配置**-显示运行时间的配置以及设置来自的位置。 在详细模式下，也显示未定义或未使用的设置。<br /><br />**对等**方-显示对等节点及其状态的列表。<br /><br />**状态**-显示 Windows 时间服务状态。<br /><br />**详细**-设置详细模式以显示详细信息。                                                                                                                                                                                                                                                                                                                   |
|                                                                                       w32tm/debug {/disable &#124; {/enable/file： <name>/size： <bytes>/entries：<value> [/truncate]}}                                                                                       |                                                                                                                                                                                                                                                                         此参数首先在 windows Vista 的 Windows 时间客户端版本和 Windows Server 2008 中提供。<br /><br />启用或禁用本地计算机 Windows 时间服务专用日志。<br /><br />**禁用**-禁用专用日志。<br /><br />**enable** -启用专用日志。<br /><br />-   **文件： <name>** -指定绝对文件名。<br />-   **size： <bytes>** -指定循环日志记录的最大大小。<br />-   **条目： <value>** -包含由数字指定并以逗号分隔的一系列标志，用于指定应记录的信息的类型。 有效数值为0至300。 除了单个数字（如 0-100103106）外，数字范围还有效。 值0-300 用于记录所有信息。<br /><br />**截断**-截断文件（如果存在）。                                                                                                                                                                                                                                                                          |

---  
有关**W32tm**的详细信息，请参阅 windows XP、windows Vista、windows 7、windows server 2003、windows Server 2003 R2、windows server 2008 和 windows Server 2008 R2 中的帮助和支持中心。  

## <a name="windows-time-service-registry-entries"></a>Windows 时间服务注册表项
以下注册表项与 Windows 时间服务相关联。  

此信息以参考的形式提供，可用于疑难解答或验证是否应用了所需的设置。 建议你不要直接编辑注册表，除非没有其他方法。 注册表编辑器或 Windows 在应用之前对其进行了修改，因此，可能会存储不正确的值。 这可能会导致系统中出现不可恢复的错误。  

如果可能，请使用组策略或其他 Windows 工具（如 Microsoft 管理控制台（MMC））来完成任务，而不是直接编辑注册表。 如果必须编辑注册表，请格外小心。  

> [!WARNING]  
> 组策略对象（GPO）设置的系统管理模板文件（系统 .adm）中配置的某些预设值与相应的默认注册表项不同。 如果你计划使用 GPO 来配置任何 Windows 时间设置，请确保查看[Windows 时间服务的预设值组策略设置不同于 Windows Server 2003 中的相应 Windows 时间服务注册表项](https://go.microsoft.com/fwlink/?LinkId=186066)。 此问题适用于 Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 R2 和 Windows Server 2003。  

Windows 时间服务的许多注册表项与相同名称的组策略设置相同。 组策略设置对应于中的相同名称的注册表项：  

>**HKLM\SYSTEM\CurrentControlSet\Services\W32Time @ no__t-1**


此注册表位置有多个注册表项。 Windows 时间设置将存储在所有这些键的值中：

* [参数](#hklmsystemcurrentcontrolsetservicesw32timeparameters)
* [Config.xml](#hklmsystemcurrentcontrolsetservicesw32timeconfig)
* [NtpClient](#hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpclient)
* [NtpServer](#hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpserver)

W32Time 的 W32Time 部分中的许多值都用于存储信息。 不应在任何时候手动更改这些值。 请勿修改本部分中的任何设置，除非你熟悉此设置，并且确定新值将按预期方式工作。 以下注册表项位于以下位置：

**HKLM\SYSTEM\CurrentControlSet\Services\W32Time**  

当你创建策略时，将在以下位置中配置设置，这不会优先于下一个位置：

**HKLM\SOFTWARE\Policies\Microsoft\Windows\W32time**

W32time 密钥是通过策略创建的。  删除策略时，也会删除此密钥。

其他默认位置：

**HKLM\SYSTEM\CurrentControlSet\Services\W32time**

某些参数以时钟计时周期存储在注册表中，一些参数以秒为单位。 将时间从时钟刻度转换为秒：  

-   1分钟 = 60 秒  

-   1秒 = 1000 ms  

-   1毫秒 = 10000 在 Windows 系统上的时钟计时周期，如[DateTime. 滴答属性](https://docs.microsoft.com/dotnet/api/system.datetime.ticks?redirectedfrom=MSDN&view=netframework-4.7.2#System_DateTime_Ticks)中所述。  

例如，5分钟将变为 5 @ no__t-060 @ no__t-11000 @ no__t-210000 = 3000000000 时钟计时周期。 

所有版本包括 Windows 7、Windows 8、Windows 10、Windows Server 2008 和 Windows Server 2008 R2，Windows Server 2012，Windows Server 2012R2，Windows Server 2016。  某些条目仅适用于较新的 Windows 版本。

#### <a name="hklmsystemcurrentcontrolsetservicesw32timeparameters"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters

|          注册表项          | Version |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|----------------------------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AllowNonstandardModeCombinations |   全部   |                                                                                                                                                                                                                                                                                                                                                                                                              条目指示在对等方之间的同步中允许非标准模式组合。 域成员的默认值为1。 独立客户端和服务器的默认值为1。                                                                                                                                                                                                                                                                                                                                                                                                              |
|            NtpServer             |   全部   | 条目指定以空格分隔的对等机列表，计算机从该列表中获取时间戳，其中每行包含一个或多个 DNS 名称或 IP 地址。 列出的每个 DNS 名称或 IP 地址必须是唯一的。 已连接到域的计算机必须与更可靠的时间源同步，例如美国官方时间时钟。  <ul><li>0x01 SpecialInterval </li><li>0x02 UseAsFallbackOnly</li><li>0x04 SymmetricActive-有关此模式的详细信息，请参阅 [Windows 时间服务器：3.3 操作模式 @ no__t。</li><li>0x08 客户端</li></ul><br />域成员上没有此注册表项的默认值。 独立客户端和服务器上的默认值为 "时间"。<br /><br />注意:有关可用 NTP 服务器的详细信息，请参阅[Microsoft 知识库文章 262680-Internet 上可用的简单网络时间协议（SNTP）时间服务器的列表](https://support.microsoft.com/help/262680/a-list-of-the-simple-network-time-protocol-sntp-time-servers-that-are) |
|            ServiceDll            |   全部   |                                                                                                                                                                                                                                                                                                                                                              输入由 W32Time 维护。 它包含 Windows 操作系统使用的保留数据，对此设置所做的任何更改都可能导致不可预知的结果。 此 DLL 在域成员和独立客户端和服务器上的默认位置是%windir%\System32\W32Time.dll。                                                                                                                                                                                                                                                                                                                                                               |
|           ServiceMain            |   全部   |                                                                                                                                                                                                                                                                                                                                                       输入由 W32Time 维护。 它包含 Windows 操作系统使用的保留数据，对此设置所做的任何更改都可能导致不可预知的结果。 域成员的默认值为 SvchostEntry_W32Time。 独立客户端和服务器上的默认值为 SvchostEntry_W32Time。  "                                                                                                                                                                                                                                                                                                                                                       |
|               类型               |   全部   |                                                                                                                                                                                                                                  条目指示要接受同步的对等方：  <ul><li>**NoSync**。 时间服务不与其他源同步。</li><li>**NTP.** 时间服务从**NtpServer**中指定的服务器同步。 注册表项。</li><li>**NT5DS**。 时间服务从域层次结构进行同步。  </li><li>**AllSync**。 时间服务使用所有可用的同步机制。  </li></ul>域成员的默认值为**NT5DS**。 独立客户端和服务器上的默认值为**NTP**。                                                                                                                                                                                                                                   |

---
#### <a name="hklmsystemcurrentcontrolsetservicesw32timeconfig"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config

|    注册表项     |          Version           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------|----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     AnnounceFlags     |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              条目控制是否将此计算机标记为可靠时间服务器。 计算机未标记为可靠，除非它也标记为时间服务器。<br /> -0x00 不是时间服务器  <br /> -0x01 Always time 服务器  <br /> -0x02 自动时间服务器  <br /> -0x04 Always 可靠时间服务器  <br /> -0x08 自动可靠时间服务器  <br />域成员的默认值为10。 独立客户端和服务器的默认值为10。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|     EventLogFlags     |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          条目控制时间服务记录的事件。  <br />时间跳跃：0x1  <br />-源更改：0x2  <br />域成员的默认值为2。 独立客户端和服务器上的默认值为2。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| FrequencyCorrectRate  |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    条目控制更正时钟的速率。 如果此值太小，则时钟不稳定，并且 overcorrects。 如果值太大，则时钟需要很长时间才能同步。 域成员的默认值为4。 独立客户端和服务器上的默认值为4。  <br /><br />请注意，0是 FrequencyCorrectRate 注册表项的无效值。 在 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 计算机上，如果将该值设置为0，Windows 时间服务会自动将其更改为1。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|      HoldPeriod       |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   条目控制禁用高峰检测的时间段，以便使本地时钟快速进入同步状态。 峰值是一个时间示例，它指示时间不到几秒钟，通常在合理的时间内返回更好的示例后接收。 域成员的默认值为5。 独立客户端和服务器上的默认值为5。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|   LargePhaseOffset    |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        条目指定大于或等于此值的时间偏移量为 10<sup>-7</sup>秒，视为峰值。 网络中断（如大量流量）可能会导致高峰。 除非长时间保持不变，否则将忽略高峰。 域成员的默认值为50000000。 独立客户端和服务器上的默认值为50000000。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|     LastClockRate     |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           输入由 W32Time 维护。 它包含 Windows 操作系统使用的保留数据，对此设置所做的任何更改都可能导致不可预知的结果。 域成员的默认值为156250。 独立客户端和服务器上的默认值为156250。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| LocalClockDispersion  |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        条目控制在唯一的时间源为内置 CMOS 时钟时必须假定的分散数（以秒为单位）。 域成员的默认值为10。 独立客户端和服务器上的默认值为10。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| MaxAllowedPhaseOffset |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        条目指定 W32Time 尝试使用时钟速率调整计算机时钟的最大偏移量（以秒为单位）。 当偏移量超过此速率时，W32Time 会直接设置计算机时钟。 域成员的默认值为300。 独立客户端和服务器的默认值为1。  [有关详细信息，请参阅下文](#maxallowedphaseoffset-information)。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|     MaxClockRate      |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          输入由 W32Time 维护。 它包含 Windows 操作系统使用的保留数据，对此设置所做的任何更改都可能导致不可预知的结果。 域成员的默认值为155860。 独立客户端和服务器的默认值为155860。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| MaxNegPhaseCorrection |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              条目指定服务所进行的最大负时间反向（以秒为单位）。 如果服务确定某个更改大于所需的更改，则会改为记录一个事件。 特例：0xFFFFFFFF 意味着始终进行时间更正。 域成员的默认值为0xFFFFFFFF。 独立客户端和服务器的默认值为54000（15小时）。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|    MaxPollInterval    |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     条目指定系统轮询间隔允许的最大间隔（以 log2 秒为单位）。 请注意，当系统必须根据计划的时间间隔进行轮询时，提供程序可以在请求时拒绝生成示例。 域控制器的默认值为10。 域成员的默认值为15。 独立客户端和服务器的默认值为15。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| MaxPosPhaseCorrection |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              条目指定服务所进行的最大正时间更正（以秒为单位）。 如果服务确定某个更改大于所需的更改，则会改为记录一个事件。 特例：0xFFFFFFFF 意味着始终进行时间更正。 域成员的默认值为0xFFFFFFFF。 独立客户端和服务器的默认值为54000（15小时）。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|     MinClockRate      |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          输入由 W32Time 维护。 它包含 Windows 操作系统使用的保留数据，对此设置所做的任何更改都可能导致不可预知的结果。 域成员的默认值为155860。 独立客户端和服务器的默认值为155860。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|    MinPollInterval    |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              条目指定系统轮询间隔允许的最小间隔，以 log2 秒为单位。 请注意，虽然系统不会更频繁地请求采样，但提供程序可以在计划的时间间隔以外的时间生成示例。 域控制器的默认值为6。 域成员的默认值为10。 独立客户端和服务器的默认值为10。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|   PhaseCorrectRate    |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           条目控制更正阶段错误的速率。 指定较小的值会快速纠正阶段错误，但可能会导致时钟变得不稳定。 如果该值过大，则需要较长的时间来更正阶段错误。 <br /><br />域成员的默认值为1。 独立客户端和服务器上的默认值为7。<br /><br />注意:0是 PhaseCorrectRate 注册表项的无效值。 在 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 计算机上，如果将该值设置为0，则 Windows 时间服务会自动将其更改为1。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|   PollAdjustFactor    |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       "输入" 控件决定增加还是减少系统的轮询间隔。 值越大，导致轮询间隔减少的错误量就越小。 域成员的默认值为5。 独立客户端和服务器上的默认值为5。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|   SpikeWatchPeriod    |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    条目指定在接受可疑偏移量之前必须保留的时间量（以秒为单位）。 域成员的默认值为900。 独立客户端和工作站上的默认值为900。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|  TimeJumpAuditOffset  |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            一个无符号整数，它指示时间跳跃审核阈值（以秒为单位）。 如果时间服务通过直接设置时钟调整了本地时钟，并且时间更正大于此值，则时间服务将记录一个审核事件。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|    UpdateInterval     |            全部             | 条目指定相位更正调整之间的时钟计时周期数。 域控制器的默认值为100。 域成员的默认值为30000。 独立客户端和服务器的默认值为360000。  <br /><br />**注意**：零是无效的 UpdateInterval 注册表项值。 在运行 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 的计算机上，如果将该值设置为0，Windows 时间服务会自动将其更改为1。<br /><br />以下三个注册表项不属于 W32Time 默认配置，但可以添加到注册表中，以获取更高的日志记录功能。 记录到系统事件日志中的信息可以通过更改组策略对象编辑器中 EventLogFlags 设置的值进行修改。 默认情况下，时间服务会在每次切换到新的时间源时事件查看器创建一个日志。<br /><br />**警告**：组策略对象（GPO）设置的系统管理模板文件（系统 .adm）中配置的某些预设值与相应的默认注册表项不同。 如果你计划使用 GPO 来配置任何 Windows 时间设置，请确保查看[Windows 时间服务的预设值组策略设置不同于 Windows Server 2003 中的相应 Windows 时间服务注册表项](https://go.microsoft.com/fwlink/?LinkId=186066)。 此问题适用于 Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 R2 和 Windows Server 2003。 |
|  UtilizeSslTimeData   | Post Windows 10 内部版本1511 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             条目1表示 W32Time 将使用多个 SSL 时间戳来播种得出十分误导不准确的时钟。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |

---
若要启用 W32Time 日志记录，必须添加下列注册表项：  

|注册表项|Version|描述|
|------------------------------------|---------------|----------------------------|
|FileLogEntries|全部|条目控制 Windows 时间日志文件中创建的条目数量。 默认值为 none，不记录任何 Windows 时间活动。 有效值为0至300。 此值不影响通常由 Windows 时间创建的事件日志条目|
|FileLogName|全部|项控制 Windows 时间日志的位置和文件名。 默认值为空，除非更改**FileLogEntries** ，否则不应更改。 有效的值是 Windows 时间用于创建日志文件的完整路径和文件名。 此值不影响通常由 Windows 时间创建的事件日志条目。  |
|FileLogSize|全部|条目控制 Windows 时间日志文件的循环日志记录行为。 定义**FileLogEntries**和**FileLogName**时，条目会定义大小（以字节为单位），以便在用新条目覆盖最旧的日志条目之前，允许日志文件到达。 对于此设置，请使用1000000或更大值。 此值不影响通常由 Windows 时间创建的事件日志条目。  |

---

#### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpclient"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient

|注册表项|Version|描述|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|全部|条目指示在对等方之间的同步中允许非标准模式组合。 域成员的默认值为1。 独立客户端和服务器的默认值为1。|
|CompatibilityFlags|全部|条目指定以下兼容性标志和值： <br /><br />- DispersionInvalid:0x00000001  <br />- IgnoreFutureRefTimeStamp:0x00000002  <br /> - AutodetectWin2K:0x80000000  <br />- AutodetectWin2KStage2:0x40000000  <br /><br />域成员的默认值为0x80000000。 独立客户端和服务器的默认值为0x80000000。  |
|CrossSiteSyncFlags|全部|条目确定服务是否选择计算机域之外的同步伙伴。 选项和值为：  <br /><br />内容0  <br />- PdcOnly:1  <br />一切2  <br /><br />如果未设置 NT5DS 值，则将忽略此值。 域成员的默认值为2。 独立客户端和服务器的默认值为2。  |
|DllName|全部|条目指定时间提供程序的 DLL 的位置。  <br /><br />此 DLL 在域成员和独立客户端和服务器上的默认位置是%windir%\System32\W32Time.dll。|
|Enabled|全部|条目指示当前时间服务中是否启用了 NtpClient 提供程序。  <br /><ul><li>是1</li><li>无0</li></ul>域成员的默认值为1。 独立客户端和服务器上的默认值为1。|
|EventLogFlags|全部|条目指定 Windows 时间服务记录的事件。<ul><li>0x1 可访问性更改</li><li>0x2 大的样本歪斜（这适用于 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2）</li></ul>域成员的默认值为0x1。 独立客户端和服务器上的默认值为0x1。|
|InputProvider|全部|条目指示是否将 NtpClient 启用为 InputProvider，这将从 NtpServer 获取时间信息。 NtpServer 是一个时间服务器，它通过返回对同步本地时钟非常有用的时间采样来响应网络上的客户端时间请求。 <ul><li>是 = 1  </li><li>No = 0 </li></ul><p>域成员和独立客户端的默认值：1  |
|LargeSampleSkew|全部|条目指定用于日志记录的大型样本偏差（秒）。 若要遵守安全和 Exchange 佣金（秒）规范，应将此设置为三秒。 仅当为0x2 大的样本倾斜显式配置了 EventLogFlags 时，才会为此设置记录事件。 域成员的默认值为3。 独立客户端和服务器上的默认值为3。  |
|ResolvePeerBackOffMaxTimes|全部|条目指定重复尝试查找对等方以使其同步失败时等待间隔的最大次数。 如果值为零，则表示等待间隔始终为最小值。 域成员的默认值为7。 独立客户端和服务器上的默认值为7。 |
|ResolvePeerBackoffMinutes|全部|输入指定要等待的初始间隔，以分钟为单位，然后尝试查找对等方以便与同步。 域成员的默认值为15。 独立客户端和服务器上的默认值为15。  |
|SpecialPollInterval|全部|条目指定手动对等方的特殊轮询间隔（以秒为单位）。 启用 SpecialInterval 0x1 标志后，W32Time 将使用此轮询间隔，而不是由操作系统确定的轮询间隔。 域成员的默认值为3600。 独立客户端和服务器上的默认值为604800。<br/><br/>New for build 1702，SpecialPollInterval 包含在 MinPollInterval 和 MaxPollInterval Config 注册表值中。|
|SpecialPollTimeRemaining|全部|输入由 W32Time 维护。 它包含 Windows 操作系统使用的保留数据。 它指定在计算机重新启动后 W32Time 重新同步之前的时间（以秒为单位）。 对此设置所做的任何更改都可能导致不可预知的结果。 域成员和独立客户端和服务器上的默认值保留为空。  |

---

#### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpserver"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer

|注册表项|Version|描述|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|全部|条目表示在客户端和服务器之间的同步中允许非标准模式组合。 域成员的默认值为1。 独立客户端和服务器的默认值为1。|
|DllName|全部|条目指定时间提供程序的 DLL 的位置。<br /><br />此 DLL 在域成员和独立客户端和服务器上的默认位置是%windir%\System32\W32Time.dll。  |
|Enabled|全部|输入指示是否在当前时间服务中启用了 NtpServer 提供程序。 <ul><li>是1</li><li>无0</li></ul>域成员的默认值为1。 独立客户端和服务器上的默认值为1。  |
|InputProvider|全部|条目指示是否将 NtpClient 启用为 InputProvider，这将从 NtpServer 获取时间信息。 NtpServer 是一个时间服务器，它通过返回对同步本地时钟非常有用的时间采样来响应网络上的客户端时间请求。 <ul><li>是 = 1  </li><li>No = 0 </li></ul><p>域成员和独立客户端的默认值：1  |

---

#### <a name="maxallowedphaseoffset-information"></a>MaxAllowedPhaseOffset 信息
为了使 W32Time 逐步设置计算机时钟，偏移量必须小于**MaxAllowedPhaseOffset**值并同时满足以下公式：  

* Windows Server 2016 及更高版本：
   ```  
    |CurrentTimeOffset| / (16*PhaseCorrectRate*pollIntervalInSeconds) <= SystemClockRate / 2  
   ``` 
* Windows Server 2012 R2 及更早版本：
   ```  
   |CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) <= SystemClockRate / 2  
   ``` 
**CurrentTimeOffset**值以时钟计时周期度量，其中 1ms = 10000 时钟计时周期在 Windows 系统上。  

**SystemClockRate**和**PhaseCorrectRate**也以时钟计时周期来度量。 若要获取**SystemClockRate**值，可以使用以下命令，并使用秒数 * 1000 @ no__t-110000 公式将其从秒转换为时钟计时周期：  

```  
W32tm /query /status /verbose  
ClockRate: 0.0156000s  
```  

**SystemclockRate**是系统上时钟的速率。 使用156000秒作为示例， **SystemclockRate**值应为 = 0.0156000 \* 1000 \* 10000 = 156000 时钟计时周期。  

**MaxAllowedPhaseOffset**也以秒为单位。 若要将其转换为时钟计时周期，请将**MaxAllowedPhaseOffset**\*1000 @ no__t-210000 相乘。  

下面的示例演示如何在使用 Windows Server 2012 R2 或更低版本时应用这些计算。

**示例 1**：时间不同于4分钟（例如，你的时间为11:05，而你从对等方收到的时间示例是11:09）。
  
```
phasecorrectRate = 1  

UpdateInterval = 30000 (clock ticks)  

systemclockRate = 156000 (clock ticks)  

MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  

|currentTimeOffset| = 4mins = 4*60\*1000\*10000 = 2400000000 ticks  

Is CurrentTimeOffset <= MaxAllowedPhaseOffset?  

2400000000 <= 6000000000 = TRUE  
```

它是否满足上述公式？ 

```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) <= SystemClockRate / 2)  

Is 2,400,000,000 / (30000*1) <= 156000/2  

Is 80,000 <= 78,000  

NO/FALSE  
```  

因此，W32tm 会立即设置时钟。  

> [!NOTE]  
> 在这种情况下，如果想要慢慢地设置时钟，还必须在注册表中调整**PhaseCorrectRate**或**updateInterval**的值，以确保公式结果为**TRUE**。  

**示例 2**：时间不同于3分钟。 
 
```  
phasecorrectRate = 1  

UpdateInterval = 30000 (clock ticks)  

systemclockRate = 156000 (clock ticks)  

MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  

currentTimeOffset = 3mins = 3*60\*1000\*10000 = 1800000000 clock ticks  

Is CurrentTimeOffset <= MaxAllowedPhaseOffset?  

1800000000 <= 6000000000 = TRUE  
```  

它是否满足上述公式？

```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) <= SystemClockRate / 2)  

Is 3 mins (1,800,000,000) / (30000*1) <= 156000/2  

Is 60,000 <= 78,000  

YES/TRUE  
```  

在这种情况下，时钟会被设置为慢慢恢复。  

## <a name="windows-time-service-group-policy-settings"></a>Windows 时间服务组策略设置  
您可以使用组策略对象编辑器来配置大多数 W32Time 参数。 这包括将计算机配置为 NTPServer 或 NTPClient，配置时间同步机制，并将计算机配置为可靠时间源。  

> [!NOTE]  
> 可在 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 域控制器上配置 Windows 时间服务组策略设置，并且只能应用于运行 Windows Server 2003、Windows Server 的计算机2003 r2、Windows Server 2008 和 Windows Server 2008 R2。  

可以在 "组策略对象编辑器" 管理单元中的以下位置找到用于配置 W32Time 的组策略设置：  

-   计算机配置 \ 管理 Templates\System\Windows 时间服务  

    在此处配置**全局配置设置**。  

-   计算机配置 \ 管理 Templates\System\Windows 时间 Service\Time 提供程序  

    在此处配置**WINDOWS NTP 客户端**设置。  

    在此处启用**WINDOWS NTP 客户端**。  

    在此处启用**WINDOWS NTP 服务器**。  

> [!WARNING]  
> 组策略对象（GPO）设置的系统管理模板文件（系统 .adm）中配置的某些预设值与相应的默认注册表项不同。 如果你计划使用 GPO 来配置任何 Windows 时间设置，请确保查看[Windows 时间服务的预设值组策略设置不同于 Windows Server 2003 中的相应 Windows 时间服务注册表项](https://go.microsoft.com/fwlink/?LinkId=186066)。 此问题适用于 Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 R2 和 Windows Server 2003。  

下表列出了与 Windows 时间服务相关联的全局组策略设置以及与每个设置关联的预设置值。 有关每个设置的详细信息，请参阅本主题前面的[Windows 时间服务注册表项](#windows-time-service-registry-entries)中的相应注册表项。 以下设置包含在称为**全局配置设置**的单个 GPO 中。  

**与 Windows 时间关联的全局组策略设置**  

|组策略设置|预设置值|  
|------------------------|------------------|  
|AnnounceFlags|10|  
|EventLogFlags|2|  
|FrequencyCorrectRate|4|  
|HoldPeriod|5|  
|LargePhaseOffset|1280000|  
|LocalClockDispersion|10|  
|MaxAllowedPhaseOffset|300|  
|MaxNegPhaseCorrection|54000（15小时）|  
|MaxPollInterval|15|  
|MaxPosPhaseCorrection|54000（15小时）|  
|MinPollInterval|10|  
|PhaseCorrectRate|7|  
|PollAdjustFactor|5|  
|SpikeWatchPeriod|90|  
|UpdateInterval|100|  

下表列出了**配置 WINDOWS NTP 客户端**GPO 的可用设置，以及与 Windows 时间服务相关联的预设置值。 有关每个设置的详细信息，请参阅本主题前面的[Windows 时间服务注册表项](#windows-time-service-registry-entries)中的相应注册表项。  

**NTP 客户端组策略与 Windows 时间关联的设置**  

|组策略设置|Default Value|  
|------------------------|-----------------|  
|NtpServer|time .com，0x1|  
|类型|默认选项：<br /><br />-   **NTP。** 在未加入域的计算机上使用。<br />-   **NT5DS。** 在加入域的计算机上使用。|  
|CrossSiteSyncFlags|2|  
|ResolvePeerBackoffMinutes|15|  
|ResolvePeerBackoffMaxTimes|7|  
|SpecialPollInterval|3600|  
|EventLogFlags|0|  

## <a name="network-ports-used-by-the-windows-time-service"></a>Windows 时间服务使用的网络端口  
Windows 时间遵循 NTP 规范，这需要将 UDP 端口123用于所有时间同步通信。 此端口由 Windows 时间保留并始终保留。 只要计算机同步其时钟或向另一台计算机提供时间，就会在 UDP 端口123上执行通信。  

> [!NOTE]  
> 如果你有一个具有多个网络适配器（也称为多宿主计算机）的计算机，则无法根据网络适配器有选择地启用 Windows 时间服务。  

## <a name="related-information"></a>相关信息  
以下资源包含与此部分相关的其他信息。  

-   IETF RFC 数据库中的 RFC *1305*  

