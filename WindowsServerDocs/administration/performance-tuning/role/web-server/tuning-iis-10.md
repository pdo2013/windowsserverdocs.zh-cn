---
title: 优化 IIS 10.0
description: 性能优化的 IIS 10.0 web 服务器上 Windows Server 16 recommmendations
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 16ea5c857d99e8a69f528e81178911236341dbb8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869848"
---
# <a name="tuning-iis-100"></a>优化 IIS 10.0

Internet 信息服务 (IIS) 10.0 是包含在 Windows ServerÂ 2016。 它使用类似于 IIS 8.5 和 IIS 7.0 进程模型。 内核模式 web 驱动程序 (http.sys) 接收和路由 HTTP 请求，并满足来自其响应的缓存的请求。 工作进程注册 URL 子空间指派并 http.sys 会将请求路由到相应的进程 （或组的应用程序池的进程）。

HTTP.sys 是负责的连接管理和请求处理。 可以从 HTTP.sys 高速缓存中提供或传递给辅助进程进行进一步处理请求。 可以配置多个工作进程，其中提供了隔离较低的成本。 有关详细信息如何请求处理的工作原理，请参阅下图：

![在 iis 10.0 中处理的请求](../../media/perftune-guide-iis-request-handling.png)

HTTP.sys 包括响应缓存。 当请求与响应缓存中的某个条目匹配时，HTTP.sys 将直接从内核模式发送的缓存响应。 某些 web 应用程序平台，如 ASP.NET 中，提供的机制来启用内核模式缓存中缓存的任何动态内容。 IIS 10.0 中的静态文件处理程序会自动缓存在 http.sys 中经常请求的文件。

由于 web 服务器具有内核模式和用户模式组件，这两个组件必须进行调整来获得最佳性能。 因此，针对特定的工作负荷的优化 IIS 10.0 包括以下配置：

-   HTTP.sys 和关联的内核模式缓存

-   工作进程和用户模式 IIS，包括应用程序池配置

-   影响性能的某些优化参数

以下部分介绍如何配置 IIS 10.0 的内核模式和用户模式方面。

## <a name="kernel-mode-settings"></a>内核模式设置

与性能相关的 HTTP.sys 设置分为两大类： 缓存管理和连接和请求的管理。 所有的注册表设置存储在以下注册表项：

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters
```

**请注意** 如果 HTTP 服务已在运行，则必须重新启动它以使更改生效。

Â 

## <a name="cache-management-settings"></a>缓存管理设置

HTTP.sys 提供的一个好处是内核模式缓存。 如果响应是在内核模式缓存中，可以满足完全从内核模式下，这样就大大降低了处理请求的 CPU 开销的 HTTP 请求。 但是，IIS 10.0 的内核模式缓存取决于物理内存，并进入成本是它占用的内存。

仅当使用它时，缓存中的条目是很有帮助。 但是，该条目始终使用物理内存正在使用的项。 你的计算结果必须缓存 （无法从缓存提供节省） 和其成本 （占用的物理内存） 中的项的有效性的条目的生存期内通过考虑可用的资源 （CPU 和物理内存） 和工作负荷系统要求。 HTTP.sys 会尽量使仅有用、 主动访问项中缓存，但可以通过优化特定工作负荷的 HTTP.sys 缓存来提高 web 服务器的性能。

以下是一些有用的设置的 HTTP.sys 内核模式缓存：

-   **UriEnableCache**默认值：1

    一个非零值启用内核模式下响应和分段缓存。 对于大多数工作负荷，缓存应保持启用状态。 请考虑禁用缓存，如果希望非常低的响应和分段缓存。

-   **UriMaxCacheMegabyteCount**默认值：0

    一个非零值，该值指定适用于内核模式缓存的最大内存。 默认值为 0，使系统可以自动调整内存量是可用于缓存。

    **请注意**指定大小设置仅的最大，并且系统可能不允许增长到的最大集大小的缓存。

    Â 

-   **UriMaxUriBytes**默认值： 262144 个字节 (256 KB)

    内核模式缓存中项的最大大小。 不缓存响应或片段比这更多。 如果您有足够的内存，请考虑增加此限制。 如果大型条目围出较小的内存很有限，它可能有助于以较低的限制。

-   **UriScavengerPeriod**默认值：120 秒

    清理程序，定期扫描 HTTP.sys 高速缓存，并删除不访问清理程序扫描之间的条目。 将清理程序周期设置为较高的值可以减少清理程序扫描数。 但是，因为较旧的、 较不经常访问的项可以保留在缓存中，可能会增加缓存内存使用情况。 将周期设置过低导致更频繁地清理程序扫描，并且可以导致过多刷新和缓存改动。

## <a name="request-and-connection-management-settings"></a>请求和连接管理设置

在 Windows Server 2016 中，HTTP.sys 将自动管理的连接。 不能再使用以下注册表设置：

-   **MaxConnections**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\MaxConnections
    ```

-   **IdleConnectionsHighMark**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleConnectionsHighMark
    ```

-   **IdleConnectionsLowMark**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleConnectionsLowMark
    ```

-   **IdleListTrimmerPeriod**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleListTrimmerPeriod
    ```

-   **RequestBufferLookasideDepth**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\RequestBufferLookasideDepth
    ```

-   **InternalRequestLookasideDepth**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\InternalRequestLookasideDepth
    ```


## <a name="user-mode-settings"></a>用户模式设置

在本部分中的设置会影响 IISÂ 10.0 辅助进程行为。 可在下面的 XML 配置文件中找到大部分这些设置：

%SystemRoot%\\system32\\inetsrv\\config\\applicationHost.config

使用 Appcmd.exe、 IIS 10.0 管理控制台、 WebAdministration 或 IISAdministration PowerShell Cmdlet 来更改它们。 自动检测到大多数设置，并且它们不需要重新启动 IIS 10.0 工作进程或 web 应用程序服务器。 ApplicationHost.config 文件的详细信息，请参阅[简介 ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig)。


## <a name="ideal-cpu-setting-for-numa-hardware"></a>可在 NUMA 硬件的理想 CPU 设置

从 Windows 2016 开始，IIS 10.0 支持对线程池线程来提高性能和可伸缩性，可在 NUMA 硬件上的自动理想 CPU 分配。 此功能默认情况下启用，并且可以通过以下注册表项配置：

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\InetInfo\Parameters\ThreadPoolUseIdealCpu
```

启用此功能，IIS 线程管理器使其最大程度地均匀地分布在所有基于其当前加载的 NUMA 节点中的所有 Cpu 到 IIS 的线程池线程。 一般情况下，建议保留此默认设置对 NUMA 硬件不变。

**请注意** 理想的 CPU 设置与不同的辅助进程 NUMA 节点分配设置 （numaNodeAssignment 和 numaNodeAffinityMode） 中引入[应用程序池的 CPU 设置](https://www.iis.net/configreference/system.applicationhost/applicationpools/add/cpu)。 理想的 CPU 设置会影响如何 IIS 分配线程池线程，而辅助角色进程 NUMA 节点分配设置确定在哪个 NUMA 节点上启动工作进程。

## <a name="user-mode-cache-behavior-settings"></a>用户模式缓存行为设置

本部分介绍影响 IISÂ 10.0 中的缓存行为的设置。 用户模式缓存作为一个模块，引发的集成管道的全局缓存事件进行侦听。 若要完全禁用用户模式缓存，请从在 applicationHost.config 中 system.webServer/globalModules 配置节中的已安装模块的列表中删除 FileCacheModule (cachfile.dll) 模块。

**system.webServer/caching**

|特性|描述|默认|
|--- |--- |--- |
|Enabled|禁用用户模式下的 IIS 缓存设置为时**False**。 缓存命中时率是非常短时，您可以禁用缓存，完全以避免与缓存的代码路径相关联的开销。 禁用用户模式缓存不会禁用内核模式缓存。|True|
|enableKernelCache|禁用内核模式缓存时设置为**False**。|True|
|maxCacheSize|限制 IIS 用户模式缓存大小为指定的大小以兆字节为单位。 IIS 调整具体取决于可用内存的默认值。 选择仔细经常基于集的大小的值访问的文件与的 RAM 或 IIS 进程地址空间量。|0|
|maxResponseSize|缓存高达指定大小的文件。 实际值取决于的数量和大小与可用的 RAM 在数据集中的最大的文件。 缓存较大，经常请求的文件可以减少 CPU 使用情况、 磁盘访问和相关联的延迟。|262144|

## <a name="compression-behavior-settings"></a>压缩行为设置

默认情况下，IIS 7.0 从开始压缩静态内容。 此外，动态内容压缩启用默认情况下安装 DynamicCompressionModule 时。 压缩可减少带宽使用情况，但会增加 CPU 使用率。 压缩的内容如有可能缓存在内核模式缓存中。 8.5 从开始，IIS 允许独立控制静态和动态内容压缩。 静态内容通常是指不会更改，如 GIF 或 HTM 文件的内容。 通常由脚本或在服务器上，即 ASP.NET 页面的代码生成动态内容。 您可以自定义的任何特定的扩展为静态或动态的分类。

若要完全禁用压缩，从在 applicationHost.config 中 system.webServer/globalModules 部分中的模块列表中删除 StaticCompressionModule 和 DynamicCompressionModule。

**system.webServer/httpCompression**

|特性|描述|默认|
|--- |--- |--- |
|staticCompression-EnableCpuUsage<br><br>staticCompression-DisableCpuUsage<br><br>dynamicCompression-EnableCpuUsage<br><br>dynamicCompression-DisableCpuUsage|启用或禁用压缩，如果当前的 CPU 使用百分比高于或低于指定的限制。<br><br>从 IIS 7.0 开始，压缩会自动禁用稳定状态 CPU 禁用阈值而增加。 如果 CPU 低于启用阈值，则启用压缩。|50、 100、 50 和 90 分别|
|目录|指定在其中压缩的版本静态文件的临时存储和缓存的目录。 请考虑移动关闭系统驱动器的此目录，如果经常访问它。|%SystemDrive%\inetpub\temp\IIS 临时压缩的文件|
|doDiskSpaceLimiting|指定是否存在限制磁盘空间量的所有压缩的文件可占用。 压缩的文件存储在由指定的压缩目录**directory**属性。|True|
|maxDiskSpaceUsage|压缩目录中指定的压缩的文件可占用的磁盘空间的字节数。<br><br>此设置可能需要增加，如果所有的压缩内容的总大小太大。|100 MB|

**system.webServer/urlCompression**

|特性|描述|默认|
|--- |--- |--- |
|doStaticCompression|指定是否压缩静态内容。|True|
|doDynamicCompression|指定是否压缩动态内容。|True|

**请注意**的服务器运行 IIS 10.0 的平均 CPU 使用率低，请考虑启用压缩动态内容，尤其是当响应很大。 这应首先完成在测试环境来评估对从基线的 CPU 使用情况的影响。


### <a name="tuning-the-default-document-list"></a>优化默认文档列表

默认文档模块处理的目录的根目录的 HTTP 请求，并将其转换成特定的文件，如前面的示例请求。 一般情况下，aroundÂ 25%的 Internet 上的所有请求都需要通过默认文档路径。 此变化较大为单独的站点。 HTTP 请求未指定文件名称，默认文档模块会在文件系统中搜索每个名称的允许的默认文档的列表。 这可以性能产生负面影响，尤其是当到达内容需要使网络往返行程或磁盘接触。

通过有选择地禁用默认文档以及减少或排序的文档列表，你可以避免开销。 对于使用默认文档的网站，应仅使用默认文档类型中减小列表。 此外，排序列表，以便它以开始最常访问的默认文档的文件名。

有选择地通过自定义在 applicationHost.config 中的位置标记内的配置或通过直接在内容目录中插入一个 web.config 文件，可以在特定 Url 中设置的默认文档行为。 这样一种混合方法，这样只有身在何处需要和正确的文件列表命名为每个 URL 集的默认文档。

若要完全禁用默认文档，请从在 applicationHost.config 中 system.webServer/globalModules 部分中的模块的列表中删除 DefaultDocumentModule。

**system.webServer/defaultDocument**

|特性|描述|默认|
|--- |--- |--- |
|已启用|指定启用默认文档。|True|
|&lt;文件&gt;元素|指定文件的配置为默认文档名称。|默认列表是 Default.htm、 Default.asp、 Index.htm、 Index.html、 Iisstart.htm 和 Default.aspx。|

## <a name="central-binary-logging"></a>中央二进制日志记录

当服务器会话具有其下的许多 URL 组时，创建数百个用于单个 URL 组带格式的日志文件和日志数据写入磁盘的过程可能会快速消耗宝贵的 CPU 和内存资源，从而创建性能和可伸缩性问题。 集中式二进制日志记录最小化日志记录，同时为需要它的组织提供详细的日志数据在使用的系统资源的数量。 分析二进制文件格式日志要求的后续处理工具。

您可以通过将 centralLogFileMode 属性设置为 CentralBinary 和设置启用中央二进制日志记录**启用**归于**True**。 请考虑移动中心的日志文件的系统分区和到以避免系统活动和日志记录活动之间的争用的专用的日志驱动器上的位置。

**system.applicationHost/log**

|特性|描述|默认|
|--- |--- |--- |
|centralLogFileMode|指定服务器的日志记录模式。 此值更改为 CentralBinary 启用中央二进制日志记录。|站点|

**system.applicationHost/log/centralBinaryLogFile**

|特性|描述|默认|
|--- |--- |--- |
|已启用|指定是否启用中央二进制日志记录。|False|
|目录|指定写入日志项的位置的目录。|%SystemDrive%\inetpub\logs\LogFiles|


## <a name="application-and-site-tunings"></a>应用程序和网站 tunings

与应用程序池和站点 tunings 相关的以下设置。

**system.applicationHost/applicationPools/applicationPoolDefaults**

|特性|描述|默认|
|--- |--- |--- |
|queueLength|指示向 HTTP.sys 多少个请求排队等待应用程序池才会开始拒绝今后的请求。 当超过此属性的值时，IIS 将拒绝后续请求使用 503 错误。<br><br>请考虑增加此如果观察到 503 错误与高延迟后端数据存储进行通信的应用程序。|1000|
|enable32BitAppOnWin64|为 True 时，可以在具有 64 位处理器的计算机上运行 32 位应用程序。<br><br>请考虑启用 32 位模式下，如果内存消耗是一个问题。 由于指针大小和指令大小较小，32 位应用程序使用的内存少于 64 位应用程序。 在 64 位计算机上运行 32 位应用程序的缺点是用户模式地址空间不能超过 4 GB。|False|

**system.applicationHost/sites/VirtualDirectoryDefault**

|特性|描述|默认|
|--- |--- |--- |
|allowSubDirConfig|指定 IIS 将查找比当前较低的内容目录中的 web.config 文件级别 (True) 还是不会查找低于当前级别 (False) 的内容目录中的 web.config 文件。 通过实施一个简单的限制，允许仅在虚拟目录中的配置，IISÂ 10.0 可以知道的除非 **/&lt;名称&gt;.htm**是虚拟目录，它不应查找配置文件。 正在跳过的其他文件操作可以显著提高性能的一非常大的随机访问的静态内容的网站。|True|

## <a name="managing-iis-100-modules"></a>管理 IIS 10.0 模块

IIS 10.0 具有已分解为多个用户可扩展的模块以支持模块化结构。 此因子分解具有较小的成本。 对于每个模块集成的管道必须针对每个与模块相关的事件调用该模块。 无论是否该模块必须执行任何将发生这种情况的工作。 可以通过删除与特定的网站不相关的所有模块，从而节省 CPU 周期和内存。

适用于简单的静态文件的 web 服务器可能包括以下五个模块：UriCacheModule、 HttpCacheModule、 StaticFileModule、 AnonymousAuthenticationModule 和 HttpLoggingModule。

若要从 applicationHost.config 中删除模块，请删除对该模块除了删除模块声明中 system.webServer/globalModules system.webServer/handlers 和 system.webServer/modules 部分中的所有引用。

## <a name="classic-asp-settings"></a>经典 ASP 设置

经典 ASP 请求的处理的主要成本包括初始化脚本引擎，如何请求的 ASP 脚本编译成的 ASP 模板，并在脚本引擎上执行模板。 模板执行成本取决于所请求的 ASP 脚本的复杂程度，而 IIS 经典 ASP 模块可以缓存在内存和磁盘中的内存和缓存模板中的脚本引擎 （仅在内存中模板缓存发生溢出） 来提高性能CPU 密集型方案。

使用以下设置来配置经典 ASP 模板缓存和脚本引擎缓存，并不会影响 ASP.NET 设置。

**system.webServer/asp/cache**

|特性|描述|默认|
|--- |--- |--- |
|diskTemplateCacheDirectory|ASP 使用内存中缓存发生溢出时，存储已编译的模板的目录的名称。<br><br>建议：设置为不是很大程度的目录使用，例如，不会与操作系统，IIS 日志中，共享的驱动器或其他经常访问的内容。|%SystemDrive%\inetpub\temp\ASP 已编译模板|
|maxDiskTemplateCacheFiles|在磁盘上指定可以缓存的已编译 ASP 模板的最大数目。<br><br>建议：设置为 0x7FFFFFFF 的最大值。|2000|
|scriptFileCacheSize|此属性指定内存中的可缓存的已编译 ASP 模板的最大数目。<br><br>建议：设置为在最多达的频繁请求 ASP 脚本由应用程序池提供服务的数量。 如果可能，将设置为内存限制允许的尽可能多的 ASP 模板。|500|
|scriptEngineCacheMax|指定将保留在内存中缓存的脚本引擎的最大数目。<br><br>建议：设置为在最多达的频繁请求 ASP 脚本由应用程序池提供服务的数量。 如果可能，将设置为任意多个脚本引擎允许内存限制。|250|

**system.webServer/asp/limits**

|特性|描述|默认|
|--- |--- |--- |
|processorThreadMax|指定的最大为每个处理器可以创建 ASP 的工作线程数。 如果当前设置不足以处理负载，这可能会导致错误，它为请求提供服务时或导致的 CPU 资源未得到充分使用情况，则增加。|25|

**system.webServer/asp/comPlus**

|特性|描述|默认|
|--- |--- |--- |
|executeInMta|设置为 **，则返回 True**如果错误或故障检测到 IIS 提供 ASP 内容时。 此问题，例如，托管多个独立的站点，每个站点在辅助角色进程下运行时。 从 COM + 事件查看器中，通常将报告错误。 此设置启用 ASP 中的多线程的单元模型。|False|


## <a name="aspnet-concurrency-setting"></a>ASP.NET 并发设置

### <a name="aspnet-35"></a>ASP.NET 3.5
默认情况下，ASP.NET 将限制请求并发度，从而减少服务器上的稳定的内存消耗。 高并发应用程序可能需要调整一些设置以改善整体性能。 您可以更改此设置在 aspnet.config 文件中：

``` syntax
<system.web>
  <applicationPool maxConcurrentRequestsPerCPU="5000"/>
</system.web>
```

以下设置可用于完全在系统上使用的资源：

-   **maxConcurrentRequestPerCpu**默认值：5000

    此设置将限制并发执行的系统上的 ASP.NET 请求的最大数目。 默认值为保守以减少内存消耗的 ASP.NET 应用程序。 请考虑增加此限制运行的应用程序执行长时间，同步 I/O 操作的系统上。 否则，用户可以体验高延迟，因为由于超出队列限制在高负载下的时使用的默认设置的队列或请求错误。

### <a name="aspnet-46"></a>ASP.NET 4.6
除了 maxConcurrentRequestPerCpu 设置中，ASP.NET 4.7 还提供了设置以提高性能主要依赖于异步操作的应用程序中。 可在 aspnet.config 文件中更改此设置。

``` syntax
<system.web>
  <applicationPool percentCpuLimit="90" percentCpuLimitMinActiveRequestPerCpu="100"/>
</system.web>
```

-   **percentCpuLimit**默认值：（超出的硬件功能） 的巨大负载放在此类方案时，90 异步请求具有一些可伸缩性问题。 问题是由于异步方案上分配的特性。 在这些情况下，分配会异步操作开始，并在完成时将使用它。 通过时，这些操作 s 很有可能对象已被移到第 1 或 2 代的 GC。 在此情况下，增加的负载会增加每秒 (rps) 直到一个点的请求。 后我们将传递给该点，在 GC 上花费的时间将开始成为一个问题，在 rps 将开始下降，具有负的缩放效果。 若要解决此问题，当 cpu 使用率超过 percentCpuLimit 设置时，请求将发送到 ASP.NET 本机队列。
-   **percentCpuLimitMinActiveRequestPerCpu**默认值：不基于 100 CPU 限制 （percentCpuLimit 设置），但它们是代价高低上的请求数。 因此，可能有几个大量占用 CPU 的请求，从而备份导致无法清空除了传入请求的本机队列中。 若要解决此 problme，可以使用 percentCpuLimitMinActiveRequestPerCpu 以确保为最小数量的请求提供服务，就启用阻止中的时，将启动。

## <a name="worker-process-and-recycling-options"></a>工作进程和回收选项

可以配置回收 IIS 工作进程的选项，并提供切实可行的解决方案很严重的情况或事件而无需干预或重置服务或计算机。 此类情况下和事件包括内存泄漏、 不断增加的内存负载或无响应或空闲工作进程。 通常情况下，可能不需要回收选项和回收可以关闭状态或系统可以配置为回收极少。

您可以启用进程回收特定应用程序通过将特性添加到**回收/periodicRestart**元素。 回收事件可以触发的几个事件，包括内存使用率、 固定的数量的请求，以及在固定的时间段。 回收工作进程时，排队和正在执行请求都将断开，并为新请求提供服务同时启动新进程。 **回收/periodicRestart**元素为每个应用程序，这意味着在下表中的每个属性按每个应用程序进行分区。

**system.applicationHost/applicationPools/ApplicationPoolDefaults/recycling/periodicRestart**

|特性|描述|默认|
|--- |--- |--- |
|memory|启用进程回收如果虚拟内存量超出指定的限制，以千字节。 这是有用的 32 位计算机： 具有小写，2 GB 的地址空间设置。 它可以帮助避免因内存不足错误而失败的请求。|0|
|privateMemory|如果专用内存分配超过指定的限制以千字节，则启用进程回收。|0|
|requests|启用进程回收之后一定数量的请求。|0|
|时间|启用进程回收指定的时间段后。|29:00:00|


## <a name="dynamic-worker-process-page-out-tuning"></a>动态工作进程页面调出优化

从 Windows Server 2012 R2 开始，IIS 提供了可以配置工作进程挂起之后已空闲一段 （除了 terminate，IIS 7 自起就存在的选项）。

空闲工作线程进程页面调出和空闲工作线程进程终止功能的主要目的是内存的为了节省内存使用率的服务器上，因为站点会消耗大量，即使它内存的只内存的坐在那儿，侦听。 具体取决于站点中使用的技术 (静态内容而非 ASP.NET 与其他框架)，所使用的内存可以为任意位置从约 10 MB 到数百个 Mb，而这意味着，如果你的服务器配置了多个网站，找出最有效的设置为您的站点可以显著提高性能的活动和挂起的站点。

我们进入细节之前，我们必须请记住，如果没有任何内存约束，则可能是最佳只需设置站点永远不会挂起或终止。 毕竟，可以使用相应 s 终止工作线程中的没有意义处理它是否在计算机上的只有一个。

**请注意** 站点运行的是不稳定的代码，如代码与内存泄漏，或否则为不稳定，设置的站点上终止空闲可以是代码 bug 的修复的应急替代方法的情况下。 这不是我们会建议，但在处理可能会更好的做法作为清理机制使用此功能，而更永久的解决方案正在进行中。\]

Â 

要考虑的另一个因素是，如果该站点使用大量内存，然后挂起进程本身需要收费，因为计算机必须写入到磁盘的工作进程使用的数据。 如果工作进程正在使用大量内存，然后暂停它可能无需等待它开始备份的成本比开销更大。

若要充分利用该辅助角色进程挂起功能，需要查看您的站点中每个应用程序池，并决定应挂起，这应终止，并且其应处于活动状态无限期。 对于每个操作和每个站点，您需要找出理想的超时期限。

理想情况下，将为挂起或终止配置的站点指的是具有访问者每一天，但不足以证明需要使其保持活动状态的时间。 它们通常具有一天的大约 20 唯一访问者的站点或更少。 可以使用站点的日志文件的流量模式进行分析和计算平均每日流量。

请注意，一旦特定用户连接到站点时，它们会通常保留在其上至少一段时间，发出其他请求，并因此只需计算每日请求可能无法准确反映实际流量模式。 若要获取更准确地阅读，您还可用于 Microsoft Excel 等工具，计算请求之间的平均时间。 例如：

||请求 URL|请求时间|增量|
|--- |--- |--- |--- |
|1|/SourceSilverLight/Geosource.web/grosource.html|10:01||
|2|/SourceSilverLight/Geosource.web/sliverlight.js|10:10|0:09|
|3|/SourceSilverLight/Geosource.web/clientbin/geo/1.aspx|10:11|0:01|
|4|/lClientAccessPolicy.xml|10:12|0:01|
|5|/ SourceSilverLight/GeosourcewebService/Service.asmx|10:23|0:11|
|6|/ SourceSilverLight/Geosource.web/GeoSearchServer...¦.|11:50|1:27|
|7|/rest/Services/CachedServices/Silverlight_load_la...¦|12:50|1:00|
|8|/rest/Services/CachedServices/Silverlight_basemap...¦.|12:51|0:01|
|9|/ rest/Services/DynamicService/Silverlight_basemap......。|12:59|0:08|
|10|/rest/Services/CachedServices/Ortho_2004_cache.as...|13:40|0:41|
|11|/rest/Services/CachedServices/Ortho_2005_cache.js|13:40|0:00|
|12|/rest/Services/CachedServices/OrthoBaseEngine.aspx|13:41|0:01|

不过，困难的部分，确定要应用于有意义的设置。 在本例中，该站点的用户，获取一系列请求和上表显示了 4 个唯一的会话的总出现一段时间的 4 个小时。 使用辅助角色进程挂起的应用程序池默认设置，该站点会终止后的默认超时的 20 分钟，这意味着每个这些用户将遇到站点启动周期。 这使得辅助进程挂起的理想候选项的大多数情况下，站点处于空闲状态，因为，因此暂停它会保留资源，并允许用户几乎可以立即访问站点。

最后一个，也是非常重要的说明这是此功能的关键在于磁盘性能。 由于挂起和唤醒过程涉及写入和读取大量数据到硬盘驱动器时，我们强烈建议为此使用较快的磁盘。 固态硬盘 (Ssd) 是理想之选，强烈建议为此，并应确保其上存储 Windows 页面文件 （如果操作系统本身未安装在 SSD 上，配置要将页面文件移到它的操作系统）。

是否使用 SSD 或不，我们还建议修复的页面文件大小以适应向其中写入页面调出数据，但不调整文件大小。 页面文件大小调整时可能会出现在操作系统需要将数据存储在页文件中，因为默认情况下，配置 Windows 以自动调整其大小，根据需要。 通过设置为一个固定大小，可以防止重设大小和大量提高性能。

若要配置预先固定的页文件大小，您需要计算其理想大小，将暂停这取决于您的站点数，和它们所占用的内存量。 如果平均为 200 MB 的活动工作进程和有 500 站点上的服务器，将会挂起，则至少应为页面文件 (200 \* 500) 的基大小通过 MB 的页面文件 (在本示例中的因此基 + 100 GB)。

**请注意**时站点将被挂起，它们将占用大约 6 MB，因此在本例中，内存使用量的所有站点都被都挂起，如果将约 3 GB。 实际上，不过，你可能永远不会让它们全部同时挂起 re。

 
## <a name="transport-layer-security-tuning-parameters"></a>传输层安全性优化参数

使用传输层安全 (TLS) 对施加更多的 CPU 开销。 TLS 的开销最大组件是因为它涉及到完整的握手建立会话建立的成本。 重新连接、 加密和解密还将添加到成本。 更好的 TLS 性能，请执行以下操作：

-   TLS 会话启用 HTTP keep-alive。 这消除了会话建立成本。

-   重复使用会话在适当的时候，尤其是在使用非保持连接的流量。

-   有选择地加密仅适用于页或需要它，而不是整个站点的站点的部分。

**注意**
-   更大的密钥提供更高的安全性，但它们也使用更多的 CPU 时间。

-   所有组件可能不需要进行加密。 但是，混合使用普通 HTTP 和 HTTPS 可能会导致警告页面上的不是所有内容都安全的一个弹出窗口。

 
## <a name="internet-server-application-programming-interface-isapi"></a>Internet 服务器应用程序编程接口 (ISAPI)

ISAPI 应用程序不需要任何特殊的优化参数。 如果编写专用 ISAPI 扩展插件，请确保它面向性能和资源使用情况。

## <a name="managed-code-tuning-guidelines"></a>托管代码优化指南

IIS 10.0 中的集成的管道模型实现高度的灵活性和可扩展性。 在本机或托管代码中实现的自定义模块可以插入到管道中，或它们可以替换现有模块。 虽然此扩展性模型提供了方便使用和简单起见，应当小心之前插入全局事件挂接的新托管的模块。 添加全局托管的模块意味着必须涉及所有请求，包括静态文件请求托管代码。 自定义模块很容易受到如垃圾收集的事件。 此外，自定义模块添加显著的 CPU 成本由于本机和托管代码之间封送处理数据。 如果可能，应设置为 managedHandler 为托管模块不满足前提条件。

若要获取更好的冷启动性能，请确保预编译应用程序预热的 ASP.NET web 站点或利用 IIS 应用程序初始化功能。

如果不需要会话状态，请确保您关闭它对于每一页。

如果有多个 I/O 绑定操作，请尝试使用异步版本相关的 Api，这将让你更好的可伸缩性。

此外正确使用输出缓存还将提升您的网站的性能。

当运行包含在独立模式下 （即一个应用程序池，每个站点） 的 ASP.NET 脚本的多个主机时，监视内存使用情况。 请确保该服务器具有足够的 RAM 预期数量的并发运行的应用程序池。 请考虑使用多个应用程序域，而不多个独立的进程。


## <a name="other-issues-that-affect-iis-performance"></a>其他问题该影响 IIS 性能

以下问题可能会影响 IIS 性能：

-   不缓存感知的筛选器的安装

    不是可识别 HTTP 缓存的筛选器的安装会导致 IIS 完全禁用缓存，这会导致性能不佳。 之前 IISÂ 6.0 编写的 ISAPI 筛选器可能会导致此行为。

-   通用网关接口 (CGI) 请求

    出于性能原因，iis 不建议使用 CGI 应用程序请求提供服务。 频繁地创建和删除 CGI 进程涉及到很大的开销。 更好的备用方法包括使用 FastCGI，ISAPI 应用程序脚本和 ASP 或 ASP.NET 的脚本。 隔离是可用于每个选项。

# <a name="see-also"></a>请参阅
- [Web 服务器性能优化](index.md) 
- [HTTP 1.1/2 优化](http-performance.md)
