---
title: 优化 IIS 10。0
description: Windows Server 16 上的 IIS 10.0 web 服务器的性能优化 recommmendations
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9563a3f3628851a0cf7b3cb79990db8c2141faa4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384946"
---
# <a name="tuning-iis-100"></a>优化 IIS 10。0

Windows Server 2016 随附 Internet Information Services （IIS）10.0。 它使用类似于 IIS 8.5 和 IIS 7.0 的进程模型。 内核模式 web 驱动程序（http.sys）接收并路由 HTTP 请求，并满足来自其响应缓存的请求。 工作进程注册 URL subspaces，http.sys 将请求路由到适当的进程（或应用程序池的进程集）。

HTTP.SYS 负责连接管理和请求处理。 可以通过 HTTP.SYS 缓存来处理请求，或将请求传递到工作进程进行进一步处理。 可以配置多个工作进程，从而以降低的成本提供隔离。 有关请求处理的工作原理的详细信息，请参阅下图：

![iis 10.0 中的请求处理](../../media/perftune-guide-iis-request-handling.png)

HTTP.SYS 包含响应缓存。 当请求与响应缓存中的条目相匹配时，http.sys 会直接从内核模式发送缓存响应。 某些 web 应用程序平台（如 ASP.NET）提供了一些机制，可以在内核模式缓存中缓存任何动态内容。 IIS 10.0 中的静态文件处理程序在 http.sys 中自动缓存经常请求的文件。

由于 web 服务器具有内核模式和用户模式组件，因此必须对这两个组件进行优化，以获得最佳性能。 因此，为特定工作负荷优化 IIS 10.0 包括配置以下各项：

-   HTTP.SYS 和关联的内核模式缓存

-   工作进程和用户模式 IIS，包括应用程序池配置

-   影响性能的某些优化参数

以下部分介绍了如何配置 IIS 10.0 的内核模式和用户模式方面。

## <a name="kernel-mode-settings"></a>内核模式设置

与性能相关的 HTTP.SYS 设置分为两大类：缓存管理和连接和请求管理。 所有注册表设置都存储在以下注册表项下：

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters
```

**请注意**  如果 HTTP 服务已经在运行，则必须重新启动才能使更改生效。

Â 

## <a name="cache-management-settings"></a>缓存管理设置

Http.sys 提供的一个优点是内核模式缓存。 如果响应在内核模式缓存中，则可以完全从内核模式满足 HTTP 请求，这会大幅降低处理请求所需的 CPU 开销。 但是，IIS 10.0 的内核模式缓存基于物理内存，项的开销是其占用的内存。

缓存中的条目只有在使用时才有用。 但是，无论是否正在使用该条目，该条目始终使用物理内存。 通过考虑可用资源（CPU 和物理内存）和工作负荷，你必须评估缓存中某个项的有用性（通过缓存对其提供服务的节省）及其在该条目生存期内的成本（占用的物理内存）。要求. HTTP.SYS 尝试在缓存中仅保留有用的活动访问项，但您可以通过为特定工作负荷优化 http.sys 缓存来提高 web 服务器的性能。

下面是 HTTP.SYS 内核模式缓存的一些有用设置：

-   **UriEnableCache**默认值：1

    如果值为非零值，则启用内核模式响应和片段缓存。 对于大多数工作负荷，缓存应保持启用状态。 如果需要很低的响应和碎片缓存，请考虑禁用缓存。

-   **UriMaxCacheMegabyteCount**默认值：0

    一个非零值，该值指定可用于内核模式缓存的最大内存。 默认值为0，使系统能够自动调整可用于缓存的内存量。

    **注意**指定大小仅设置最大值，系统可能不允许缓存大小增长到最大集大小。

    Â 

-   **UriMaxUriBytes**默认值： 262144字节（256 KB）

    内核模式缓存中条目的最大大小。 不会缓存大于此的响应或碎片。 如果有足够的内存，请考虑增加限制。 如果内存有限，且大项 crowding 较小的项，则可能会降低限制。

-   **UriScavengerPeriod**默认值：120 秒

    HTTP.SYS 缓存由清除程序定期扫描，并删除清除程序扫描之间未访问的条目。 将清除周期设置为较高的值将减少清除清理的次数。 但是，缓存内存使用量可能会增加，因为在缓存中可以保留较旧、不经常访问的条目。 将该时间段设置得过低会导致清除清理次数过多，并可能导致刷新和缓存改动过多。

## <a name="request-and-connection-management-settings"></a>请求和连接管理设置

在 Windows Server 2016 中，http.sys 自动管理连接。 不再使用以下注册表设置：

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

本部分中的设置将影响 IISÂ10.0 工作进程的行为。 其中的大多数设置都可以在下面的 XML 配置文件中找到：

% SystemRoot% \\system32 @ no__t-1inetsrv @ no__t-2config @ no__t-3applicationHost

使用 Appcmd.exe、IIS 10.0 管理控制台、WebAdministration 或 IISAdministration PowerShell Cmdlet 来更改它们。 大多数设置是自动检测的，它们不需要重启 IIS 10.0 工作进程或 web 应用程序服务器。 有关 Applicationhost.config 文件的详细信息，请参阅[Applicationhost.config 简介](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig)。


## <a name="ideal-cpu-setting-for-numa-hardware"></a>NUMA 硬件的理想 CPU 设置

从 Windows 2016 开始，IIS 10.0 支持其线程池线程的自动理想 CPU 分配，以提高 NUMA 硬件的性能和可伸缩性。 此功能在默认情况下处于启用状态，可通过以下注册表项进行配置：

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\InetInfo\Parameters\ThreadPoolUseIdealCpu
```

启用此功能后，IIS 线程管理器将尽最大努力基于当前负载在所有 NUMA 节点的所有 Cpu 之间平均分配 IIS 线程池线程。 通常情况下，建议将 NUMA 硬件的默认设置保持不变。

**请注意**  理想 cpu 设置不同于在[应用程序池的 CPU 设置](https://www.iis.net/configreference/system.applicationhost/applicationpools/add/cpu)中引入的工作进程 NUMA 节点分配设置（numaNodeAssignment 和 numaNodeAffinityMode）。 理想的 CPU 设置会影响 IIS 分发其线程池线程的方式，而工作进程 NUMA 节点分配设置确定工作进程在哪个 NUMA 节点上启动。

## <a name="user-mode-cache-behavior-settings"></a>用户模式缓存行为设置

本部分介绍影响 IISÂ10.0 中的缓存行为的设置。 用户模式缓存是作为一个模块来实现的，该模块侦听集成管道引发的全局缓存事件。 若要完全禁用用户模式缓存，请从 Applicationhost.config 中的 System.webserver/globalModules 配置节中的已安装模块列表中删除 FileCacheModule （cachfile）模块。

**system.webserver/缓存**

|特性|描述|默认|
|--- |--- |--- |
|Enabled|当设置为**False**时，禁用用户模式的 IIS 缓存。 如果缓存命中率非常小，则可以完全禁用缓存，以避免与缓存代码路径相关联的开销。 禁用用户模式缓存不会禁用内核模式缓存。|True|
|enableKernelCache|当设置为**False**时禁用内核模式缓存。|True|
|maxCacheSize|将 IIS 用户模式缓存大小限制为指定的大小（以 Mb 为单位）。 IIS 根据可用内存调整默认值。 根据经常访问的文件集的大小以及 RAM 或 IIS 进程地址空间的大小，仔细选择值。|0|
|maxResponseSize|将文件缓存到指定大小。 实际值取决于数据集中最大文件的数量和大小，以及可用 RAM。 缓存大型、频繁请求的文件可以降低 CPU 使用量、磁盘访问和相关的延迟。|262144|

## <a name="compression-behavior-settings"></a>压缩行为设置

默认情况下，从7.0 开始的 IIS 压缩静态内容。 此外，在安装 DynamicCompressionModule 时，会默认启用动态内容压缩。 压缩可减少带宽使用量，但会增大 CPU 使用率。 如果可能，压缩内容缓存在内核模式缓存中。 从8.5 开始，IIS 允许为静态和动态内容单独控制压缩。 静态内容通常是指不会更改的内容，如 GIF 或 .HTM 文件。 动态内容通常由脚本或服务器上的代码生成，即 ASP.NET 页面。 您可以自定义任何特定扩展的分类为静态或动态。

若要完全禁用压缩，请从 Applicationhost.config 中的 System.webserver/globalModules 节中的模块列表中删除 StaticCompressionModule 和 DynamicCompressionModule。

**system.webserver/httpCompression**

|特性|描述|默认|
|--- |--- |--- |
|staticCompression-EnableCpuUsage<br><br>staticCompression-DisableCpuUsage<br><br>dynamicCompression-EnableCpuUsage<br><br>dynamicCompression-DisableCpuUsage|如果当前百分比 CPU 使用率高于或低于指定的限制，则启用或禁用压缩。<br><br>从 IIS 7.0 开始，如果稳定状态的 CPU 高于禁用阈值，则会自动禁用压缩。 如果 CPU 低于启用阈值，则启用压缩。|分别为50、100、50和90|
|文件夹|指定临时存储和缓存压缩版本的静态文件的目录。 如果经常访问此目录，请考虑将其移出系统驱动器。|%SystemDrive%\inetpub\temp\IIS 临时压缩文件|
|doDiskSpaceLimiting|指定是否对所有压缩文件所占用的磁盘空间量有限制。 压缩的文件存储在**目录**属性指定的压缩目录中。|True|
|maxDiskSpaceUsage|指定压缩目录中压缩文件可以占用的磁盘空间字节数。<br><br>如果所有压缩内容的总大小太大，则可能需要增加此设置。|100 MB|

**system.webserver/Urlcompression>**

|特性|描述|默认|
|--- |--- |--- |
|doStaticCompression|指定是否压缩静态内容。|True|
|doDynamicCompression|指定是否压缩动态内容。|True|

**注意**对于运行 IIS 10.0 且 CPU 使用率较低的服务器，请考虑为动态内容启用压缩功能，尤其是在响应很大的情况下。 应该首先在测试环境中完成此操作，以评估基准的 CPU 使用情况。


### <a name="tuning-the-default-document-list"></a>优化默认文档列表

默认文档模块处理对目录的根的 HTTP 请求，并将其转换为对特定文件（如 index.htm 或 index.htm）的请求。 平均而言，Internet 上的所有请求的 25% 会经历默认的文档路径。 对于单独的站点，这种方式会有所不同。 如果 HTTP 请求未指定文件名，则默认的文档模块会在文件系统中搜索每个名称允许的默认文档的列表。 这会对性能产生负面影响，特别是在到达内容时，需要进行网络往返或触摸磁盘。

可以通过有选择地禁用默认文档和减少或排序文档列表来避免开销。 对于使用默认文档的网站，应将列表缩小为仅使用所使用的默认文档类型。 此外，对列表排序，使其以最常访问的默认文档文件名开头。

通过自定义 Applicationhost.config 中的位置标记内的配置或直接在内容目录中插入 web.config 文件，可以有选择地为特定 Url 设置默认文档行为。 这允许采用混合方法，这种方法仅在需要时才启用默认文档，并将列表设置为每个 URL 的正确文件名。

若要完全禁用默认文档，请从 Applicationhost.config 中的 System.webserver/globalModules 部分的模块列表中删除 DefaultDocumentModule。

**system.webserver/defaultDocument**

|特性|描述|默认|
|--- |--- |--- |
|enabled|指定启用默认文档。|True|
|&lt;files @ no__t-1 元素|指定配置为默认文档的文件名。|默认列表为默认值 .htm、默认 .asp、index.htm、Index、Iisstart.png、和 default.aspx。|

## <a name="central-binary-logging"></a>中央二进制日志记录

当服务器会话在其下有许多 URL 组时，为单独的 URL 组创建数百个格式化日志文件并将日志数据写入到磁盘的过程可以快速消耗宝贵的 CPU 和内存资源，从而提高性能和可伸缩性问题。 集中式二进制日志记录可将用于日志记录的系统资源的数量降至最低，同时为需要它的组织提供详细的日志数据。 分析二进制格式的日志需要后处理工具。

可以通过将 centralLogFileMode 属性设置为 CentralBinary 并将**enabled**属性设置为**True**来启用中心二进制日志记录。 请考虑将中心日志文件的位置移出系统分区并拖到专用日志记录驱动器上，以避免系统活动和日志记录活动之间的争用。

**system.web/log**

|特性|描述|默认|
|--- |--- |--- |
|centralLogFileMode|指定服务器的日志记录模式。 将此值更改为 CentralBinary 以启用中心二进制日志记录。|站点|

**system.web/log/centralBinaryLogFile**

|特性|描述|默认|
|--- |--- |--- |
|enabled|指定是否启用中心二进制日志记录。|False|
|文件夹|指定写入日志项的目录。|%SystemDrive%\inetpub\logs\LogFiles|


## <a name="application-and-site-tunings"></a>应用程序和站点 tunings

以下设置与应用程序池和站点 tunings 相关。

**system.web/applicationPools/applicationPoolDefaults**

|特性|描述|默认|
|--- |--- |--- |
|queueLength|向 HTTP.SYS 指示在以后的请求被拒绝之前，对应用程序池排队的请求数。 超出此属性的值时，IIS 将拒绝后续请求，并出现503错误。<br><br>如果观察到503错误，请考虑为与高延迟后端数据存储通信的应用程序增加此项。|1000|
|enable32BitAppOnWin64|如果为 True，则使32位应用程序能够在具有64位处理器的计算机上运行。<br><br>如果需要考虑内存消耗，请考虑启用32位模式。 由于指针大小和指令大小较小，32位应用程序使用的内存少于64位应用程序。 在64位计算机上运行32位应用程序的缺点是，用户模式地址空间限制为 4 GB。|False|

**system.web/sites/VirtualDirectoryDefault**

|特性|描述|默认|
|--- |--- |--- |
|了 allowsubdirconfig|指定 IIS 在内容目录中查找低于当前级别的 web.config 文件（True），还是在内容目录中查找低于当前级别的 web.config 文件（False）。 通过强制使用简单的限制（仅允许在虚拟目录中进行配置），IISÂ10.0 可以知道，除非 **/ @ no__t-2name&gt;.htm**是虚拟目录，否则它不会查找配置文件。 跳过其他文件操作可以显著提高包含大量随机访问的静态内容的网站的性能。|True|

## <a name="managing-iis-100-modules"></a>管理 IIS 10.0 模块

IIS 10.0 已分解成多个用户可扩展模块，以支持模块化结构。 此因子分解的成本较低。 对于每个模块，集成管道必须为与模块相关的每个事件调用该模块。 无论模块是否必须执行任何操作，都将发生这种情况。 可以通过删除与特定网站无关的所有模块来节省 CPU 周期和内存。

优化简单静态文件的 web 服务器可能仅包含以下五个模块：UriCacheModule、HttpCacheModule、StaticFileModule、AnonymousAuthenticationModule 和 HttpLoggingModule。

若要从 Applicationhost.config 中删除模块，请从 system.webserver/处理程序和 System.webserver/module 节中删除对模块的所有引用，还可删除 system.webserver/globalModules 中的模块声明。

## <a name="classic-asp-settings"></a>经典 ASP 设置

处理经典 ASP 请求的主要成本包括初始化脚本引擎、将所请求的 ASP 脚本编译为 ASP 模板以及对脚本引擎执行模板。 尽管模板执行成本取决于所请求 ASP 脚本的复杂性，但 IIS 经典 ASP 模块可以在内存和磁盘中缓存脚本引擎（仅在内存中模板缓存溢出时），以提高性能CPU 绑定方案。

以下设置用于配置经典 ASP 模板缓存和脚本引擎缓存，而不影响 ASP.NET 设置。

**system.webserver/asp/cache**

|特性|描述|默认|
|--- |--- |--- |
|diskTemplateCacheDirectory|当内存中缓存溢出时，ASP 用于存储已编译模板的目录的名称。<br><br>建议：将设置为一个不会频繁使用的目录，例如，未与操作系统、IIS 日志或其他经常访问的内容共享的驱动器。|%SystemDrive%\inetpub\temp\ASP 编译的模板|
|maxDiskTemplateCacheFiles|指定可在磁盘上缓存的已编译 ASP 模板的最大数目。<br><br>建议：设置为0x7FFFFFFF 的最大值。|2000|
|scriptFileCacheSize|此属性指定可在内存中缓存的已编译 ASP 模板的最大数目。<br><br>建议：至少设置为应用程序池提供服务的频繁请求 ASP 脚本的数量。 如果可能，将设置为 "内存限制允许的任意数量的 ASP 模板"。|500|
|scriptEngineCacheMax|指定将缓存在内存中的脚本引擎的最大数量。<br><br>建议：至少设置为应用程序池提供服务的频繁请求 ASP 脚本的数量。 如果可能，将设置为内存限制所允许的最大脚本引擎数。|250|

**system.webserver/asp/限制**

|特性|描述|默认|
|--- |--- |--- |
|processorThreadMax|指定 ASP 可以为每个处理器创建的最大工作线程数。 如果当前设置不足以处理负载，则增加，这可能会导致在处理请求时出现错误，或者导致 CPU 资源使用不足。|25|

**system.webserver/asp/comPlus**

|特性|描述|默认|
|--- |--- |--- |
|executeInMta|如果在 IIS 为 ASP 内容提供服务时检测到错误或失败，则设置为**True** 。 例如，在托管多个独立的站点时，每个站点都在其自己的工作进程中运行。 错误通常从事件查看器中的 COM + 中报告。 此设置在 ASP 中启用多线程单元模型。|False|


## <a name="aspnet-concurrency-setting"></a>ASP.NET 并发设置

### <a name="aspnet-35"></a>ASP.NET 3。5
默认情况下，ASP.NET 限制请求并发以降低服务器上稳定状态的内存消耗。 高并发性应用程序可能需要调整一些设置以提高整体性能。 你可以在 aspnet .config 文件中更改此设置：

``` syntax
<system.web>
  <applicationPool maxConcurrentRequestsPerCPU="5000"/>
</system.web>
```

以下设置对于完全使用系统资源非常有用：

-   **maxConcurrentRequestPerCpu**默认值：5000

    此设置限制系统上同时执行的 ASP.NET 请求的最大数量。 默认值为保守以减少 ASP.NET 应用程序的内存占用。 考虑在运行执行长时间同步 i/o 操作的应用程序的系统上增加此限制。 否则，用户可能会遇到高延迟，因为在使用默认设置时，由于队列限制超出了队列限制，导致队列或请求失败。

### <a name="aspnet-46"></a>ASP.NET 4.6
除了 maxConcurrentRequestPerCpu 设置外，ASP.NET 4.7 还提供了一些设置，以提高严重依赖于异步操作的应用程序的性能。 可在 aspnet .config 文件中更改此设置。

``` syntax
<system.web>
  <applicationPool percentCpuLimit="90" percentCpuLimitMinActiveRequestPerCpu="100"/>
</system.web>
```

-   **percentCpuLimit**默认值：90在对这种情况施加大量负载（超出硬件功能）时，异步请求会出现一些可伸缩性问题。 此问题是由对异步方案进行分配的性质导致的。 在这些情况下，将在异步操作启动时进行分配，并且在完成时将使用它。 在这段时间，itâs 非常可能将对象移动到第1代或第2代垃圾回收器。 发生这种情况时，增加负载将显示每秒请求增加数（rps），直到达到某个点。 传递该点后，GC 中花费的时间会开始成为一个问题，rps 将开始进行 dip，这会造成负面影响。 若要解决此问题，当 cpu 使用率超出 percentCpuLimit 设置时，请求将发送到 ASP.NET 本机队列。
-   **percentCpuLimitMinActiveRequestPerCpu**默认值：100 CPU 限制（percentCpuLimit 设置）不基于请求数，而是取决于请求的开销。 因此，可能只需要几个占用 CPU 的请求，这会导致在本机队列中进行备份，使其不会从传入请求中清空。 若要解决此 problme，可以使用 percentCpuLimitMinActiveRequestPerCpu 来确保在限制开始之前提供最少数量的请求。

## <a name="worker-process-and-recycling-options"></a>工作进程和回收选项

你可以配置用于回收 IIS 工作进程的选项，并向锐音符的情况或事件提供切实可行的解决方案，而无需干预或重置服务或计算机。 此类情况和事件包括内存泄漏、增加内存负载或无响应或空闲工作进程。 在普通条件下，可能不需要回收选项，并且可以关闭回收，或者可以将系统配置为很少回收。

您可以通过将属性添加到**回收/periodicRestart**元素来为特定应用程序启用进程回收。 回收事件可以由多个事件触发，其中包括内存使用情况、固定的请求数和固定时间段。 回收工作进程后，排队和正在执行的请求将耗尽，并同时启动一个新进程来处理新请求。 **回收/periodicRestart**元素为每个应用程序，这意味着下表中的每个属性都按应用程序进行分区。

**system.web/applicationPools/ApplicationPoolDefaults/回收/periodicRestart**

|特性|描述|默认|
|--- |--- |--- |
|memory|如果虚拟内存消耗超过指定的限制（以 kb 为单位），则启用进程回收。 对于具有小型 2 GB 地址空间的32位计算机，这是一项有用的设置。 由于内存不足错误，此方法可帮助避免失败的请求。|0|
|privateMemory|如果专用内存分配超过指定的限制（以 kb 为单位），则启用进程回收。|0|
|requests|在特定数量的请求后启用进程回收。|0|
|时间|在指定的时间段后启用进程回收。|29:00:00|


## <a name="dynamic-worker-process-page-out-tuning"></a>动态辅助进程-处理页面输出优化

从 Windows Server 2012 R2 开始，IIS 提供了一个选项，可将工作进程配置为在空闲一段时间后挂起（除了选项 "终止"，因为 IIS 7 已存在）。

空闲工作进程页面输出和空闲工作进程终止功能的主要目的是为了节省服务器上的内存使用率，因为即使只是坐在那里，站点也会消耗大量内存。 根据站点上使用的技术（静态内容 vs ASP.NET 与其他框架），使用的内存可以从大约 10 MB 到数百 Mb，这意味着，如果你的服务器配置了多个站点，则需要确定最有效的设置对于网站，可以显著提高活动和挂起网站的性能。

在深入了解之前，必须记住，如果不存在内存限制，最好将站点设置为从不暂停或终止。 毕竟，如果只是计算机上只有一个工作进程，则终止该工作进程的 thereâs 很少。

**请注意**  以防站点运行不稳定的代码（例如，内存泄漏的代码或不稳定），则将站点设置为在空闲时终止可能是修复代码错误的快速而又不稳定的替代方法。 这并不是我们要鼓励的事情，但在捕获中，将此功能用作干净的机制可能更好，而在工作中使用更永久性的解决方案。 \]

Â 

要考虑的另一个因素是，如果站点确实使用大量内存，则挂起过程本身会耗费大量时间，因为计算机必须将辅助进程使用的数据写入磁盘。 如果工作进程正在使用大量内存，则挂起它的成本可能比不得不等待它启动备份的成本要高得多。

若要充分利用工作进程挂起功能，需要查看每个应用程序池中的站点，并决定应该暂停的站点，哪些应终止，哪一个应终止，哪些应处于活动状态。 对于每个操作和每个站点，都需要找出理想的超时期限。

理想情况下，将配置为挂起或终止的站点是指每天都有访客的站点，但并不能保证始终保持活动状态。 通常，这些站点的站点数大约为20个，每个唯一的访问者为一天。 您可以使用站点的日志文件分析流量模式，并计算平均每日流量。

请记住，一旦特定用户连接到该站点，这些用户通常会保留一段时间，发出附加请求，因此只计算每日请求可能无法准确反映实际的流量模式。 若要获得更准确的阅读，还可以使用工具（如 Microsoft Excel）来计算请求之间的平均时间。 例如：

||请求 URL|请求时间|Delta|
|--- |--- |--- |--- |
|1|/SourceSilverLight/Geosource.web/grosource.html|10:01||
|2|/SourceSilverLight/Geosource.web/sliverlight.js|10:10|0:09|
|3|/SourceSilverLight/Geosource.web/clientbin/geo/1.aspx|10:11|0:01|
|4|/lClientAccessPolicy.xml|10:12|0:01|
|5|/SourceSilverLight/GeosourcewebService/服务|10:23|0:11|
|6|/SourceSilverLight/Geosource/GeoSearchServer ¦。|11:50|1:27|
|7|/rest/Services/CachedServices/Silverlight_load_la... ¦|12:50|1:00|
|8|/rest/Services/CachedServices/Silverlight_basemap... ¦。|12:51|0:01|
|9|/ rest/Services/DynamicService/Silverlight_basemap...¦.|12:59|0:08|
|10|/rest/Services/CachedServices/Ortho_2004_cache.as...|13:40|0:41|
|11|/rest/Services/CachedServices/Ortho_2005_cache.js|13:40|0:00|
|12|/rest/Services/CachedServices/OrthoBaseEngine.aspx|13:41|0:01|

不过，硬部分却要确定要应用哪些设置才能合理。 在我们的示例中，该站点从用户那里获取一组请求，上表显示在4小时内总共发生了4个唯一会话。 对于应用程序池的工作进程挂起的默认设置，在默认的20分钟超时后，将终止该站点，这意味着，其中每个用户都将遇到站点启动循环。 这使它成为工作进程挂起的理想候选项，因为在大多数情况下，该站点处于空闲状态，因此暂停它将节省资源，并使用户几乎可以即时访问该站点。

有关此功能的最后一个非常重要的注意事项是磁盘性能对于此功能至关重要。 由于挂起和唤醒过程涉及到在硬盘驱动器上写入和读取大量数据，因此我们强烈建议使用快速磁盘进行此操作。 固态硬盘（Ssd）非常适合用于此操作，因此，你应该确保 Windows 页面文件存储在它上（如果 SSD 上未安装操作系统本身，请将操作系统配置为将页面文件移动到该文件中）。

无论是否使用 SSD，我们还建议修复页面文件的大小，以便在不进行文件大小调整的情况下向其写入页面输出数据。 如果操作系统需要在页面文件中存储数据，则可能会发生页面文件大小调整，因为默认情况下，Windows 配置为根据需要自动调整其大小。 通过将大小设置为固定的大小，可以防止大量调整大小和提高性能。

若要配置固定页面文件大小，需要计算其理想大小，具体取决于要暂停的站点数以及它们消耗的内存量。 如果活动工作进程的平均大小为 200 MB，并且在要挂起的服务器上有500个站点，则页面文件的基本大小至少应为（200 \* 500） MB （如此示例中的基 + 100 GB）。

**注意**当站点被挂起时，它们会消耗大约 6 MB 的内存，因此在我们的示例中，如果所有站点都处于挂起状态，则内存使用率大约为 3 GB。 但实际上，youâre 可能永远不会同时挂起它们。

 
## <a name="transport-layer-security-tuning-parameters"></a>传输层安全优化参数

使用传输层安全性（TLS）会产生额外的 CPU 开销。 TLS 的最昂贵的组件是建立会话建立的开销，因为它涉及到完全握手。 重新连接、加密和解密还会增加成本。 为获得更好的 TLS 性能，请执行以下操作：

-   为 TLS 会话启用 HTTP keep-alive。 这将消除会话建立成本。

-   适当时重用会话，尤其是在非 keep-alive 流量中使用。

-   选择性地将加密应用于需要它的页面或部分，而不是整个站点。

**注意**
-   较大的密钥可提供更高的安全性，但也会占用更多的 CPU 时间。

-   所有组件都可能无需加密。 但是，混合使用纯 HTTP 和 HTTPS 可能会导致弹出警告，而不是页面上的所有内容都是安全的。

 
## <a name="internet-server-application-programming-interface-isapi"></a>Internet 服务器应用程序编程接口（ISAPI）

ISAPI 应用程序不需要任何特殊的优化参数。 如果你编写了一个专用 ISAPI 扩展，请确保它是为性能和资源使用而编写的。

## <a name="managed-code-tuning-guidelines"></a>托管代码优化指南

IIS 10.0 中的集成管道模式实现了高度的灵活性和可扩展性。 在本机代码或托管代码中实现的自定义模块可以插入管道中，也可以替换现有模块。 尽管此扩展性模型提供便利和简易性，但在插入挂接到全局事件的新托管模块之前应小心。 添加全局托管模块意味着所有请求（包括静态文件请求）都必须触及托管代码。 自定义模块容易发生垃圾回收等事件。 此外，自定义模块由于在本机代码和托管代码之间封送数据而增加了大量的 CPU 开销。 如果可能，应将 "managedHandler" 设置为 "托管模块" 的 ""。

若要获得更好的冷启动性能，请确保预编译 ASP.NET 网站或利用 IIS 应用程序初始化功能来使应用程序预热。

如果不需要会话状态，请确保为每个页面禁用会话状态。

如果有很多 i/o 绑定操作，请尝试使用异步版本的相关 Api，这将为你提供更好的可伸缩性。

同时，正确使用输出缓存还会提高网站的性能。

当你在隔离模式下运行包含 ASP.NET 脚本的多个主机（每个站点一个应用程序池），则监视内存使用情况。 请确保服务器具有足够的 RAM 来容纳预期数量的同时运行的应用程序池。 请考虑使用多个应用程序域，而不是多个独立进程。


## <a name="other-issues-that-affect-iis-performance"></a>影响 IIS 性能的其他问题

以下问题可能会影响 IIS 性能：

-   安装不能识别缓存的筛选器

    如果安装的筛选器不能识别 HTTP 缓存，将导致 IIS 完全禁用缓存，从而导致性能不佳。 在 IISÂ6.0 之前编写的 ISAPI 筛选器会导致此行为。

-   通用网关接口（CGI）请求

    出于性能方面的考虑，不建议在 IIS 中使用 CGI 应用程序来处理请求。 经常创建和删除 CGI 进程涉及到很大的开销。 更好的替代方法包括使用 FastCGI、ISAPI 应用程序脚本、ASP 或 ASP.NET 脚本。 每个选项都提供隔离。

# <a name="see-also"></a>请参阅
- [Web 服务器性能优化](index.md) 
- [HTTP 1.1/2 优化](http-performance.md)
