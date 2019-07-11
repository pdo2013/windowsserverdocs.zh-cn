---
title: Active Directory 域服务的容量规划
description: 若要在 AD DS 的容量规划过程中考虑的因素的详细的讨论。
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: 5a9e2d39d4eedd1e8fdb4bfeaf267ad4cb4c596a
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799829"
---
# <a name="capacity-planning-for-active-directory-domain-services"></a>Active Directory 域服务的容量规划

本主题最初由 Ken Brumfield，在 Microsoft，高级首席现场工程师编写，并提供了建议的容量规划 Active Directory 域服务 (AD DS)。

## <a name="goals-of-capacity-planning"></a>目标的容量规划

容量规划本身是不与故障排除性能事件相同。 它们是密切相关，但大不相同。 容量规划的目标是：  

- 正确实现并运行环境 
- 最大程度减少所用时间的故障排除性能问题。  
  
在容量规划中，一个组织可能具有在高峰期间为了满足客户端性能要求并适应升级在数据中心中的硬件所需的时间的 40%处理器利用率的基线目标。 而得到的异常性能事件通知，请监视警报阈值可能会设置在 90 %5 分钟间隔内。

区别是不断超出容量管理阈值 （一次性事件不是关键因素），添加容量 （即，添加更多或更快的处理器中） 将是一个解决方案或将跨多个服务器缩放服务解决方案。 性能警报阈值指示客户端体验当前遇到问题并且需要立即采取的措施来解决问题。

打个比方： 容量管理是指防止 （防御性推动，从而确保结果正在正确，依次类推） 汽车意外而性能故障排除是警察、 消防部门和紧急的医疗专业人员执行的操作后意外。 这是有关"防御性 driving"Active Directory 样式。

过去几年中，对于向上缩放系统的容量规划指南发生了重大变化。 系统体系结构中的以下更改有要求有关设计和缩放服务的基本假设：

- 64 位服务器平台  
- 虚拟化  
- 增加注意过能源消耗  
- SSD 存储  
- 云方案  

此外，方法从基于服务器的容量规划到基于服务的容量规划练习练习转移。 Active Directory 域服务 (AD DS)、 成熟的分布式服务，许多 Microsoft 和第三方产品使用作为后端，将成为一个最关键的产品进行正确规划以确保其他应用程序运行所必需的能力。

### <a name="baseline-requirements-for-capacity-planning-guidance"></a>容量计划指南的基本要求

在整篇文章中，预期的以下基本要求：

- 读取器具有读取和使用者熟悉[性能优化准则的 Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85))。
- Windows Server 平台是 x64 基于体系结构。 但即使 Active Directory 环境中 （现在不支持生命周期的结束） 的 Windows Server 2003 x86 上安装并具有目录信息树 (DIT)，它是更少 1.5 GB 的大小，可以轻松地保存在内存中，从该准则文章都仍适用。
- 容量规划是一个持续过程，您应定期查看环境满足期望的程度。
- 优化会随着硬件的成本变化通过多个硬件生命周期。 例如，内存变得更便宜、 每个核心的成本会降低，或不同的存储的价格选项更改。
- 在一天的峰值忙碌时段的计划。 建议看看这 30 分钟或小时内的时间间隔。 更高版本的任何内容可能会隐藏实际峰值以及任何小于可能会扭曲通过"暂时性的峰值。"
- 在过去的适用于企业的硬件生命周期规划的增长。 这可能包括升级或添加硬件以交错的方式或完全刷新每隔三到五年中的策略。 如何将会增长得多 Active Directory 上的负载，二者都需要"猜测"。 历史数据，如果收集，可帮助进行此评估。 
- 规划容错能力。 一次估计*N*派生的计划的方案包括*N* &ndash; 1， *N* &ndash; 2 *N* &ndash; *x*。
  - 根据组织需要确保单个或多个服务器的丢失不超过最大的峰值容量估计值的其他服务器中添加。
  - 此外请考虑，增长计划和错误容差计划需要集成。 例如，如果一个 DC 需要支持负载，但在评估是负载将在下一年中加倍，并且需要两个 Dc 总，将不会有足够的容量来支持容错。 解决方案是使用三个域控制器启动。 一个可以计划以添加第三个 DC 3 或 6 个月后，如果预算是有些紧张。

    >[!NOTE]
    >添加 Active Directory 感知应用程序可能具有 DC 负载中，会受到明显影响是否加载来自应用程序服务器或客户端。

### <a name="three-step-process-for-the-capacity-planning-cycle"></a>容量规划周期的三步骤过程

在容量规划中，首先确定需要哪些质量的服务。 例如，核心数据中心支持更高级别的并发，且需要更一致的体验为用户和使用方应用程序，需要更高版本注意冗余和最大程度减少系统和基础结构的瓶颈。 与此相反，具有少量的用户的附属位置不需要相同级别的并发或容错能力。 因此，分支机构可能不需要太过关注优化基础硬件和基础结构，这可能会导致成本节约。 所有建议和指南均是为了获得最佳性能，并可以有选择地允许对于使用更少的苛刻需求的方案。

下一个问题是： 虚拟或物理？ 从容量规划角度来看，是不正确或错误的答案;没有仅不同要使用的变量集。 虚拟化方案会涉及到两个选项之一：

- "直接映射"，与每个所在的主机 （虚拟化来抽象物理硬件从服务器） 的一个来宾
- "共享的主机"

测试和生产方案中指示的"直接映射"方案可以视为相同到物理主机。 "共享的主机，"但是，引入了一些更高版本拼写更多详细信息中的注意事项。 "共享的主机"方案意味着 AD DS 也争用资源，并有损失和优化注意事项用于执行此操作。

出于上述考虑，容量规划周期是一个迭代的三步骤过程：

1. 度量值的现有环境、 确定系统瓶颈当前位置，并获取环境的基础知识需计划的所需的容量。
1. 确定需要根据步骤 1 中所述的标准的硬件。
1. 监视和验证基础结构实现在规范内操作。 在此步骤中收集某些数据将基线成为下一个周期的容量规划。

### <a name="applying-the-process"></a>应用过程

若要优化性能，请确保正确选择和调到该应用程序加载这些主要组件：

1. 内存
1. 网络
1. 存储
1. 处理器
1. Net Logon

AD DS 的基本存储要求和精心编写的客户端软件的常规行为允许具有多达 10000 到 20000 个用户放弃中容量规划与物理硬件，几乎任何现代服务器相关的大量投资的环境类系统将处理负载。 话虽如此，但下表总结了如何评估现有环境，以便选择合适的硬件。 每个组件会分析中的后续部分来帮助评估他们使用的基准建议和特定于环境的主体的基础结构的 AD DS 管理员的详细信息。

通常情况下：

- 任何基于当前数据的大小调整仅将当前环境的准确。
- 对任何估计值，预期需求增长的硬件生命周期内。
- 确定是否过大今天和发展到更大的环境中，或对生命周期中增加容量。
- 对于虚拟化，所有相同容量规划主体和方法应用，不同之处在于的虚拟化开销需要相关的域添加到的任何内容。
- 容量规划，像尝试预测，不是精确的科学。 不希望要计算以 100%的准确率完全和事项。 指导原则是在 leanest 建议;添加更多安全性的容量中，并持续验证环境保留在目标上。

### <a name="data-collection-summary-tables"></a>数据集合摘要表

#### <a name="new-environment"></a>新的环境

| 组件 | 估计值 |
|-|-|
|存储/数据库大小|40 KB 至 60 KB 的每个用户|
|RAM|数据库大小<br />基本操作系统的建议<br />第三方应用程序|
|网络|1 GB|
|CPU|每个核心的 1000 个并发用户|

#### <a name="high-level-evaluation-criteria"></a>高级评估条件

| 组件 | 评估条件 | 规划注意事项 |
|-|-|-|
|存储/数据库大小|标题为的部分"若要激活的碎片整理释放磁盘空间的日志记录"中[存储限制](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|存储 / 数据库性能|<ul><li>"逻辑磁盘 ( *\<NTDS 数据库驱动器\>* ) \Avg Disk sec/Read，""逻辑磁盘使用情况 ( *\<NTDS 数据库驱动器\>* ) \Avg Disk sec/Write，""逻辑磁盘 ( *\<NTDS 数据库驱动器\>* ) \Avg Disk sec/Transfer"</li><li>"LogicalDisk( *\<NTDS Database Drive\>* )\Reads/sec," "LogicalDisk( *\<NTDS Database Drive\>* )\Writes/sec," "LogicalDisk( *\<NTDS Database Drive\>* )\Transfers/sec"</li></ul>|<ul><li>存储有两个问题需要解决<ul><li>可用空间，基于存储大小为今天的基于主轴和 SSD 的不相关，大多数 AD 环境。</li> <li>可用 – 在许多环境中的输入/输出 (IO) 操作，这通常是被忽略。 但是，必须评估只有环境不足够的 RAM 来将整个 NTDS 数据库加载到内存。</li></ul><li>存储可以是一个复杂的主题，涉及方应该包括适当的调整大小的硬件供应商专业知识。 尤其是对于如 SAN、 NAS 和 iSCSI 方案更复杂的方案。 但是，一般情况下，每个千兆字节成本通常是存储的直接对立以每个 IO 的成本：<ul><li>RAID 5 具有每十亿字节成本低于 Raid 1，但 Raid 1 具有每个 IO 的降低成本</li><li>基于您的硬盘驱动器都具有较低成本每十亿字节，但 SSDs 具有每个 IO 较低的成本</li></ul><li>在计算机或 Active Directory 域服务服务重新启动之后, 可扩展存储引擎 (ESE) 缓存为空，性能将会绑定而预热缓存磁盘。</li><li>在大多数环境中 AD 读取密集型 I/O 以随机的方式写入磁盘上，从而抵消的许多缓存的优势，并读取优化策略。  此外，AD 具有比大多数存储系统缓存方式更大内存中的缓存。</li></ul>
|RAM|<ul><li>数据库大小</li><li>基本操作系统的建议</li><li>第三方应用程序</li></ul>|<ul><li>存储是计算机中最慢的组件。 越多，可以将驻留在 RAM，越少就必须转到磁盘。</li><li>确保分配足够的 RAM 用于存储操作系统，代理 （防病毒、 备份、 监视），NTDS 数据库和随时间增大。</li><li>其中最大化的 RAM 量不是的环境的成本 （如卫星的位置） 有效或不可行 （DIT 是太大）、 存储部分，以确保该存储适当调整大小的引用。</li></ul>|
|网络|<ul><li>“Network Interface(\*)\Bytes Received/sec”</li><li>“Network Interface(\*)\Bytes Sent/sec”|<ul><li>一般情况下，流量发送从 DC 远远超出了发送到 DC 的流量。</li><li>随着交换以太网连接是全双工，入站和出站网络流量必须独立调整大小。</li><li>Dc 的整合数将增加使用要为每个 DC，发送回客户端请求的响应的带宽量但将笔悬停于线性作为一个整体的站点。</li><li>如果删除附属位置 Dc，请不要忘记添加附属 DC 的中心域控制器的带宽，以及使用它来评估将有多少 WAN 流量。</li></ul>|
|CPU"逻辑磁盘 ( *\<NTDS 数据库驱动器\>* ) \Avg Disk sec/Read"descripti"Process(lsass)\\处理器时间百分比"tors to consider during capacity planning for AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
---

# Capacity planning for Active Directory Domain Services

This topic is originally written by Ken Brumfield, Senior Premier Field Engineer at Microsoft, and provides recommendations for capacity planning for Active Directory Domain Services (AD DS).

## Goals of capacity planning

Capacity planning is not the same as troubleshooting performance incidents. They are closely related, but quite different. The goals of capacity planning are:  

- Properly implement and operate an environment 
- Minimize the time spent troubleshooting performance issues.  
  
In capacity planning, an organization might have a baseline target of 40% processor utilization during peak periods in order to meet client performance requirements and accommodate the time necessary to upgrade the hardware in the datacenter. Whereas, to be notified of abnormal performance incidents, a monitoring alert threshold might be set at 90% over a 5 minute interval.

The difference is that when a capacity management threshold is continually exceeded (a one-time event is not a concern), adding capacity (that is, adding in more or faster processors) would be a solution or scaling the service across multiple servers would be a solution. Performance alert thresholds indicate that client experience is currently suffering and immediate steps are needed to address the issue.

As an analogy: capacity management is about preventing a car accident (defensive driving, making sure the brakes are working properly, and so on) whereas performance troubleshooting is what the police, fire department, and emergency medical professionals do after an accident. This is about “defensive driving,” Active Directory-style.

Over the last several years, capacity planning guidance for scale-up systems has changed dramatically. The following changes in system architectures have challenged fundamental assumptions about designing and scaling a service:

- 64-bit server platforms  
- Virtualization  
- Increased attention to power consumption  
- SSD storage  
- Cloud scenarios  

Additionally, the approach is shifting from a server-based capacity planning exercise to a service-based capacity planning exercise. Active Directory Domain Services (AD DS), a mature distributed service that many Microsoft and third-party products use as a backend, becomes one the most critical products to plan correctly to ensure the necessary capacity for other applications to run.

### Baseline requirements for capacity planning guidance

Throughout this article, the following baseline requirements are expected:

- Readers have read and are familiar with [Performance Tuning Guidelines for Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- The Windows Server platform is an x64 based architecture. But even if your Active Directory environment is installed on Windows Server 2003 x86 (now beyond the end of the support lifecycle) and has a directory information tree (DIT) that is less 1.5 GB in size and that can easily be held in memory, the guidelines from this article are still applicable.
- Capacity planning is a continuous process and you should regularly review how well the environment is meeting expectations.
- Optimization will occur over multiple hardware lifecycles as hardware costs change. For example, memory becomes cheaper, the cost per core decreases, or the price of different storage options change.
- Plan for the peak busy period of the day. It is recommended to look at this in either 30 minute or hour intervals. Anything greater may hide the actual peaks and anything less may be distorted by “transient spikes.”
- Plan for growth over the course of the hardware lifecycle for the enterprise. This may include a strategy of upgrading or adding hardware in a staggered fashion, or a complete refresh every three to five years. Each will require a “guess” as how much the load on Active Directory will grow. Historical data, if collected, will help with this assessment. 
- Plan for fault tolerance. Once an estimate *N* is derived, plan for scenarios that include *N* &ndash; 1, *N* &ndash; 2, *N* &ndash; *x*.
  - Add in additional servers according to organizational need to ensure that the loss of a single or multiple servers does not exceed maximum peak capacity estimates.
  - Also consider that the growth plan and fault tolerance plan need to be integrated. For example, if one DC is required to support the load, but the estimate is that the load will be doubled in the next year and require two DCs total, there will not be enough capacity to support fault tolerance. The solution would be to start with three DCs. One could also plan to add the third DC after 3 or 6 months if budgets are tight.

    >[!NOTE]
    >Adding Active Directory-aware applications might have a noticeable impact on the DC load, whether the load is coming from the application servers or clients.

### Three-step process for the capacity planning cycle

In capacity planning, first decide what quality of service is needed. For example, a core datacenter supports a higher level of concurrency and requires more consistent experience for users and consuming applications, which requires greater attention to redundancy and minimizing system and infrastructure bottlenecks. In contrast, a satellite location with a handful of users does not need the same level of concurrency or fault tolerance. Thus, the satellite office might not need as much attention to optimizing the underlying hardware and infrastructure, which may lead to cost savings. All recommendations and guidance herein are for optimal performance, and can be selectively relaxed for scenarios with less demanding requirements.

The next question is: virtualized or physical? From a capacity planning perspective, there is no right or wrong answer; there is only a different set of variables to work with. Virtualization scenarios come down to one of two options:

- “Direct mapping” with one guest per host (where virtualization exists solely to abstract the physical hardware from the server)
- “Shared host”

Testing and production scenarios indicate that the “direct mapping” scenario can be treated identically to a physical host. “Shared host,” however, introduces a number of considerations spelled out in more detail later. The “shared host” scenario means that AD DS is also competing for resources, and there are penalties and tuning considerations for doing so.

With these considerations in mind, the capacity planning cycle is an iterative three-step process:

1. Measure the existing environment, determine where the system bottlenecks currently are, and get environmental basics necessary to plan the amount of capacity needed.
1. Determine the hardware needed according to the criteria outlined in step 1.
1. Monitor and validate that the infrastructure as implemented is operating within specifications. Some data collected in this step becomes the baseline for the next cycle of capacity planning.

### Applying the process

To optimize performance, ensure these major components are correctly selected and tuned to the application loads:

1. Memory
1. Network
1. Storage
1. Processor
1. Net Logon

The basic storage requirements of AD DS and the general behavior of well written client software allow environments with as many as 10,000 to 20,000 users to forego heavy investment in capacity planning with regards to physical hardware, as almost any modern server class system will handle the load. That said, the following table summarizes how to evaluate an existing environment in order to select the right hardware. Each component is analyzed in detail in subsequent sections to help AD DS administrators evaluate their infrastructure using baseline recommendations and environment-specific principals.

In general:

- Any sizing based on current data will only be accurate for the current environment.
- For any estimates, expect demand to grow over the lifecycle of the hardware.
- Determine whether to oversize today and grow into the larger environment, or add capacity over the lifecycle.
- For virtualization, all the same capacity planning principals and methodologies apply, except that the overhead of the virtualization needs to be added to anything domain related.
- Capacity planning, like anything that attempts to predict, is NOT an accurate science. Do not expect things to calculate perfectly and with 100% accuracy. The guidance here is the leanest recommendation; add in capacity for additional safety and continuously validate that the environment remains on target.

### Data collection summary tables

#### New environment

| Component | Estimates |
|-|-|
|Storage/Database Size|40 KB to 60 KB for each user|
|RAM|Database Size<br />Base operating system recommendations<br />Third-party applications|
|Network|1 GB|
|CPU|1000 concurrent users for each core|

#### High-level evaluation criteria

| Component | Evaluation criteria | Planning considerations |
|-|-|-|
|Storage/Database size|The section entitled “To activate logging of disk space that is freed by defragmentation” in [Storage Limits](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Storage/ Database performance|<ul><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer"</li><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Reads/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Writes/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec"</li></ul>|<ul><li>Storage has two concerns to address<ul><li>Space available, which with the size of today’s spindle based and SSD based storage is irrelevant for most AD environments.</li> <li>Input/Output (IO) Operations available – In many environments, this is often overlooked. But it is important to evaluate only environments where there is not enough RAM to load the entire NTDS Database into memory.</li></ul><li>Storage can be a complex topic and should involve hardware vendor expertise for proper sizing. Particularly with more complex scenarios such as SAN, NAS, and iSCSI scenarios. However, in general, cost per Gigabyte of storage is often in direct opposition to cost per IO:<ul><li>RAID 5 has lower cost per Gigabyte than Raid 1, but Raid 1 has lower cost per IO</li><li>Spindle-based hard drives have lower cost per Gigabyte, but SSDs have a lower cost per IO</li></ul><li>After a restart of the computer or the Active Directory Domain Services service, the Extensible Storage Engine (ESE) cache is empty and performance will be disk bound while the cache warms.</li><li>In most environments AD is read intensive I/O in a random pattern to disks, negating much of the benefit of caching and read optimization strategies.  Plus, AD has a way larger cache in memory than most storage system caches.</li></ul>
|RAM|<ul><li>Database size</li><li>Base operating system recommendations</li><li>Third-party applications</li></ul>|<ul><li>Storage is the slowest component in a computer. The more that can be resident in RAM, the less it is necessary to go to disk.</li><li>Ensure enough RAM is allocated to store the operating system, Agents (antivirus, backup, monitoring), NTDS Database and growth over time.</li><li>For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.</li></ul>|
|Network|<ul><li>“Network Interface(\*)\Bytes Received/sec”</li><li>“Network Interface(\*)\Bytes Sent/sec”|<ul><li>In general, traffic sent from a DC far exceeds traffic sent to a DC.</li><li>As a switched Ethernet connection is full-duplex, inbound and outbound network traffic need to be sized independently.</li><li>Consolidating the number of DCs will increase the amount of bandwidth used to send responses back to client requests for each DC, but will be close enough to linear for the site as a whole.</li><li>If removing satellite location DCs, don’t forget to add the bandwidth for the satellite DC into the hub DCs as well as use that to evaluate how much WAN traffic there will be.</li></ul>|
|CPU|<ul><li>“Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read”</li><li>“Process(lsass)\\% Processor Time”</li></ul>|<ul><li>消除存储成为瓶颈之后, 解决所需的计算能力的大小。</li><li>尽管不完全线性的使用特定的作用域 （如网站） 中的所有服务器上的处理器内核数可以用于仪表有多少处理器支持的总客户端负载所需。 添加最小必需跨多个作用域内的所有系统维护服务的当前级别。</li><li>处理器速度，包括电源管理中的更改相关更改，派生自当前环境的影响数字。 通常情况下，不可能准确地评估如何从 2.5 GHz 处理器转到 3 GHz 处理器会减少所需的 Cpu 数。</li></ul>|
|NetLogon|<ul><li>“Netlogon(\*)\Semaphore Acquires”</li><li>“Netlogon(\*)\Semaphore Timeouts”</li><li>“Netlogon(\*)\Average Semaphore Hold Time”</li></ul>|<ul><li>Net Logon 安全通道/MaxConcurrentAPI 只会影响使用 NTLM 身份验证和/或 PAC 验证环境。 PAC 验证是在默认情况下，在 Windows Server 2008 之前的操作系统版本。 这是一个客户端设置，因此在域控制器将受到影响，直到关闭所有客户端系统上的此。</li><li>如果未适当调整大小，则具有大量跨信任身份验证，其中包括内部林信任，环境具有更大的风险。</li><li>服务器合并也会增加并发运行的跨信任身份验证。</li><li>激增需要容纳，例如群集的故障转移，因为用户重新进行身份验证组策略为新的群集节点。</li><li>单个客户端系统 （例如群集） 可能需要过优化。</li></ul>|

## <a name="planning"></a>计划

较长时间，为调整 AD DS 大小的社区建议一直以"放入作为数据库大小一样多 RAM。" 大多数情况下，建议的大多数环境所需关心如何。 但是，使用 AD DS 的生态系统的过程已经变得更大，具有 AD DS 环境本身，1999 年以来。 尽管增加计算能力和交换机从 x86 到 x64 体系结构进行了调整大小的性能相关的大型数据集的虚拟化增长的物理硬件上运行 AD DS 的客户具有的微妙的方面的体系结构重新引入比之前更大的受众的优化问题。

以下指南因此是有关如何确定和计划 Active Directory 的需求作为一项服务而不考虑是否部署在物理、 虚拟/物理混合使用或完全虚拟化的方案。 在这种情况下，我们将细分到四个主要组件的每个评估： 存储、 内存、 网络和处理器。 简单地说，为了获得最佳性能对 AD DS，目标是处理器绑定尽可能接近。

## <a name="ram"></a>RAM

越少，可以在 RAM 中，缓存的越多，只需是需转到磁盘。 若要最大程度提高服务器的可伸缩性的最小 RAM 量应的当前数据库大小、 总 SYSVOL 大小、 操作系统和推荐金额和代理 （防病毒、 监视、 备份、 等的供应商建议). 应添加额外数量以适应增长的整个生命周期的服务器。 这将是宗旨主观根据环境更改的数据库提高估计的。

其中最大化的 RAM 量不是的环境的成本 （如卫星的位置） 有效或不可行 （DIT 是太大）、 存储部分，以确保该存储设计合理的引用。

传入的常规上下文中调整大小的内存的必然结果页面文件的大小调整。 与所有其他内容相同的上下文中与内存，目标是尽量减少转到有很大速度变慢磁盘。 因此，问题应转化，"如何应页面文件调整的大小？" 为"RAM 量需要以最大程度减少分页？" 后一种问题的答案是在本部分的其余部分所述。 这样，最多为调整大小的常规操作系统建议和需要配置内存转储，AD DS 的性能与无关的系统领域的页文件的话题。

### <a name="evaluating"></a>评估

域控制器 (DC) 所需的 RAM 量出于这些原因是实际的复杂练习：

- 需要高可能出现的错误时尝试使用现有系统以测量 RAM 量，因为 LSASS 将剪裁内存压力情况下，人为地以需放气。
- 单个 DC 仅需要缓存什么是"有趣的"向其客户端主观事实。 这意味着需要缓存在具有 Exchange 服务器的站点中的 DC 上的数据将与需要在上的 DC，仅对用户进行身份验证缓存的数据非常不同。
- 要针对每个 DC 的案例的基础上计算 RAM 的人工成本很高和环境的变化而发生变化。
- 建议后面的条件将有助于做出明智的决策： 
- 可在内存中缓存的越多，越少就必须转到磁盘。 
- 存储到目前为止是计算机的最慢的组件。 访问基于心轴上的数据和 SSD 存储介质是 1000000 x 低于在 RAM 中对数据的访问顺序。

因此，为了最大程度提高服务器的最小 RAM 量的可伸缩性的当前数据库大小、 总 SYSVOL 大小、 操作系统和建议金额和代理 （防病毒软件，监视、 备份、 供应商建议等等）。 添加附加的金额，以适应增长的整个生命周期的服务器。 这将是增长的宗旨主观基于估计的数据库。 但是，对于使用少量的最终用户的附属位置，这些要求可以放宽因为这些站点将不需要缓存到像大多数请求提供服务。

其中最大化的 RAM 量不是的环境的成本 （如卫星的位置） 有效或不可行 （DIT 是太大）、 存储部分，以确保该存储适当调整大小的引用。

> [!NOTE]
> 时内存调整大小的必然结果页面文件的大小调整。 目标是尽量减少转到有很大速度变慢磁盘，因为问题是由从"如何应页面文件调整的大小？" 为"RAM 量需要以最大程度减少分页？" 后一种问题的答案是在本部分的其余部分所述。 这样，最多为调整大小的常规操作系统建议和需要配置内存转储，AD DS 的性能与无关的系统领域的页文件的话题。

### <a name="virtualization-considerations-for-ram"></a>RAM 的虚拟化注意事项

避免在主机上的内存过度提交。 优化的 RAM 量的基本目标是时间的尽量转到磁盘所用量。 在虚拟化方案中，内存过度提交的概念存在其中更多的 RAM 分配给来宾，则存在于物理计算机上。 这本身并不是问题。 服务器当前使用的所有来宾的内存总量超出主机上的 RAM 量，并且基础主机开始分页时，它将成为问题。 性能将成为绑定磁盘 NTDS.dit 获取数据，将域控制器或域控制器会将页面文件获取数据，或将主机转至磁盘以获取来宾认为是在 RAM 中的数据。

### <a name="calculation-summary-example"></a>计算摘要示例

|组件|估计的内存 （示例）|
|-|-|
|基本操作系统建议的 RAM (Windows Server 2008)|2 GB|
|LSASS 内部任务|200 MB|
|监视代理|100 MB|
|防病毒|100 MB|
|数据库 （全局目录）|8.5 GB 是否确实???|
|要在运行，管理员可以登录而不会影响备份缓冲|1 GB|
|总计|12 GB|

**建议：16 GB**

随着时间推移，可以进行假设更多的数据将添加到数据库和服务器可能会在生产中 3 到 5 年。 根据估计值的 33%的增长，16 GB 会在合理的 RAM 以放入一台物理服务器。 在虚拟机中，给定的易用性，通过它可以修改设置，并且 RAM 可以添加到 VM，开始制定计划，监视和升级将来 12 GB 是合理的。

## <a name="network"></a>网络

### <a name="evaluating"></a>评估
本部分是有关评估复制通信，它侧重于遍历 WAN 流量并且中全面介绍了有关的需求更少[Active Directory 复制流量](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10))、 有关评估总比包括客户端查询、 组策略应用程序等所需的带宽和网络容量。 对于现有的环境，这可以通过来收集性能计数器"网络接口 (\*) \Bytes Received/sec"和"网络接口 (\*) \Bytes Sent/sec。" 网络接口以 15、 30 或 60 分钟为单位的计数器的采样间隔。 任何小于通常会为准确地衡量; 太易失性更高版本的任何内容将过消除每日查看。

> [!NOTE]
> 通常情况下，大部分在 DC 上的网络流量是出站的因为 DC 响应客户端的查询。 这是对出站流量，关注的原因，尽管建议还评估每个环境的入站流量。 相同的方法可以用于解决并查看入站的网络流量要求。 有关详细信息，请参阅知识库文章[929851:在 Windows Vista 和 Windows Server 2008 中 TCP/IP 的默认动态端口范围已更改](http://support.microsoft.com/kb/929851)。

### <a name="bandwidth-needs"></a>带宽需求

规划网络可伸缩性包括两个不同类别： 的流量和 CPU 负载的网络流量。 每种方案是与本文中的其他主题的一些比较简单直接。

在评估必须支持多少流量，有两种唯一类别的容量规划 AD DS 在网络流量方面。 第一种是复制流量的域控制器之间遍历并参考中全面涵盖[Active Directory 复制流量](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10))和仍适用于当前版本的 AD DS。 第二个是站点内客户端到服务器的通信。 从相对于大量的数据发送回客户端的客户端接收的较小请求进行主要计划用于站点内通信的更简单的方案之一。 通常会在环境中最多 5,000 个用户每个服务器，站点中有足够 100 MB。 使用 1 GB 网络适配器和接收方缩放 (RSS) 支持为 5,000 个用户上的任何建议。 若要验证此方案中的，尤其对于服务器合并方案，请查看网络接口 (\*) \Bytes/sec 跨站点中的所有域控制器将它们相加，并将域控制器，以确保存在的目标数是足够的容量。 若要执行此操作的最简单方法是为了使用 Windows 可靠性和性能监视器 （以前称为 Perfmon） 中的"堆积面积图"视图，以确保所有计数器都缩放相同。

请考虑下面的示例 (也称为，实际上，真正复杂的方式来验证一般规则是适用于特定的环境)。 进行以下假设：

- 目标是尽可能少的服务器到减少占用的空间。 理想情况下，一台服务器将携带负载，并以实现冗余部署另一台服务器 (*N* + 1 的方案)。 
- 在此方案中，当前的网络适配器支持只有 100 MB，是在交换环境中。  
  最大目标网络带宽利用率为 N 方案 （丢失的 DC） 中的 60%。
- 每个服务器都有大约 10,000 个客户端连接到它。

从图表中数据所获得的知识 (网络接口 (\*) \Bytes Sent/sec):

1. 在工作日开始上升为大约 5:30 和即将结束时，向下晚上 7:00。
1. 高峰最繁忙时段是从上午 8:00 到上午 8:15，与大于 25 发送的字节/秒的最繁忙的 DC 上。  
   > [!NOTE]
   > 所有性能数据都是历史数据。 因此在 8:15 的峰值数据点表示为 8:15 8:00 负载。
1. 前 4:00 AM，超过 20 个字节发送每秒最繁忙的 dc 中，这可能指示从不同的时区任一负载或后台基础结构的活动，例如备份有峰值。 上午 8:00 峰值超过此活动，因为它不相关。
1. 在站点中有五个域控制器。
1. 最大负载是每个 DC，它表示的 100 MB 连接 44%的大约 5.5 MB/秒。 使用此数据，它可以估计上午 8:00 和上午 8:15 之间所需的总带宽为 28 MB/秒。
   >[!NOTE]
   >要注意这一事实，网络接口发送/接收计数器以字节为单位的网络带宽以位为单位。 100 MB &divide; 8 = 12.5 MB，1GB &divide; 8 = 128 MB。
  
结论：

1. 此环境中当前无法满足目标利用率为 60%的容错能力的 N + 1 级别。 将一个系统离线将移动每个服务器的带宽从大约 5.5 MB/秒 （44%)大约 7 MB/秒 （56%)。
1. 基于前面所述整合到一台服务器的目标，这样既超出了最大目标利用率和理论上可能使用的 100 MB 连接。
1. 具有 1 GB 连接，这将表示 22%的总容量。
1. 在正常操作中的条件*N* + 1 种情况下，客户端负载将较平均地分散在每个服务器或 11%的总容量的大约 14 MB/秒。
1. 若要确保容量足以期间不可用的 DC，每个服务器的正常操作目标是约 30%的网络使用率或每个服务器 38 MB/秒。 故障转移目标将是 60%的网络利用率或 72 MB/秒每台服务器。  
  
简单地说，系统最终部署必须具有 1 GB 网络适配器，并且连接到将支持上述的负载的网络基础结构。 进一步注意是，给定生成网络流量的量，从网络通信的 CPU 负载可以产生显著的影响并限制 AD DS 的最大可伸缩性。 这一过程可以用于估计的到 DC 的入站通信。 但有了相对于入站流量的出站流量的优势，是大多数环境的学术练习。 确保硬件支持 rss 大于 5,000 个用户每个服务器的环境中至关重要。 对于网络流量很高的情况，中断负载平衡可能成为瓶颈。 这可以由处理器检测到 (\*)\%均匀分散到 Cpu 中断时间。 启用了 RSS 的 Nic 可以减轻此限制并提高可伸缩性。

> [!NOTE]
> 可以使用类似的方法来合并数据中心时估计需要的额外容量或停用附属位置中的域控制器。 只需收集到客户端的出站和入站流量，而且将是现在将出现在 WAN 链接上的流量的量。  
>  
> 在某些情况下，可能会遇到更多流量不是预期情况，因为流量会慢些，如证书检查失败时以满足过长超时而在广域网上。 出于此原因，WAN 大小调整和利用率应为迭代的持续过程。

### <a name="virtualization-considerations-for-network-bandwidth"></a>虚拟化网络带宽注意事项

它很容易进行物理服务器的建议：支持大于 5000 个用户的服务器的 1 GB。 后多个来宾启动共享基础的虚拟交换机基础结构，请多加注意有必要确保主机有足够的网络带宽，可以在系统上，支持所有来宾，并需要更多的严格程度。 这就是确保网络基础结构到主机的扩展。 这是无论网络是否包括在主机上作为虚拟机来宾运行超出一个虚拟交换机的网络流量使用的域控制器，还是直接连接到物理交换机。 虚拟交换机是只有一个详细组件上行链路需要支持的数据传输量。 因此，链接到该交换机的物理主机物理网络适配器应该能够支持 DC 负载，以及共享连接到物理网络适配器的虚拟交换机的所有其他来宾。

### <a name="calculation-summary-example"></a>计算摘要示例

|系统|峰值带宽|
|-|-|
DC 1|6.5 MB/秒|
DC 2|6.25 MB/秒|
|DC 3|6.25 MB/秒|
|DC 4|5.75 MB/秒|
|DC 5|4.75 MB/秒|
|总计|28.5 MB/秒|

**建议：72 MB/秒**（28.5 MB/秒除以 40%）

|目标系统计数|从上面的总带宽|
|-|-|
|2|28.5 MB/秒|
|生成的正常行为|28.5 &divide; 2 = 14.25 MB/秒|

与往常一样，随着时间的推移假设可以进行客户端负载将增加，并且这种增长应尽可能最佳的规划。 要规划的建议的量就为 50%的估计网络流量增长。

## <a name="storage"></a>存储

规划存储构成了两个组件：

- 存储大小或容量，
- 性能

规划容量，使性能通常完全忽略是所花费了大量的时间和文档。 与当前硬件成本，大多数环境不是大足够的其中一种情况下，实际问题，并且系统建议以"放入作为数据库大小一样多 RAM"通常涵盖其余部分中，尽管它可能会影响附属位置中更大环境。

### <a name="sizing"></a>大小调整

#### <a name="evaluating-for-storage"></a>存储在为评估

13 年前引入 Active Directory 时，未将最常见的驱动器大小 4 GB 和为 9 GB 的驱动器的时间与相比，Active Directory 的大小调整不甚至所有的考虑因素，但最大的环境。 最小可用硬盘中 180 GB 范围、 整个操作系统、 SYSVOL、 和 NTDS.dit 的大小可以方便地放在一个驱动器上。 在这种情况下，建议不推荐使用此区域中的大量投资。

不考虑的唯一推荐方法是确保 NTDS.dit 大小的 110%可用才能启用碎片整理。 此外，应进行调整的硬件生命周期的增长。

第一个也是最重要的考虑因素评估如何大型 NTDS.dit 和 SYSVOL 将为。 这些度量值将导致大小固定磁盘和内存分配。 由于 （） 成本相对低廉的这些组件，不需要严格且精确数学计算。 有关如何为现有和新的环境可在计算此内容[数据存储](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10))系列的文章。 具体而言，请参阅以下文章：

- **对于现有环境&ndash;** 部分中标题为"若要激活的碎片整理释放磁盘空间的日志记录"一文[存储限制](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))。
- **为新环境&ndash;** 文[Active Directory 用户和组织单位的增长估计](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10))。

  > [!NOTE]
  > 文章基于版本的 Windows 2000 中的 Active Directory 时所做的数据大小估计值。 使用对象大小，以反映你的环境中的对象的实际大小。

在检查时具有多个域的现有环境，可能存在的数据库大小的变化。 这为 true 时，使用最小全局编录 (GC) 和非 GC 大小。  

数据库大小可以不同的操作系统版本而异。 运行早期版本的操作系统，如 Windows Server 2003 具有较小的数据库大小比的 DC，运行更高版本的操作系统，例如 Windows Server 2008 R2，尤其是在功能这种 Active Directory 回收站 Bin 或凭据漫游启用的 Dc。

> [!NOTE]  
  >
>- 对于新环境，请注意，在 Active Directory 用户和组织单位的估计增长的估算值指示 100000 个用户 （在相同域中） 占用大约 450 MB 的空间。 请注意填充的属性可以总量具有很大的影响。 将由第三方和 Microsoft 产品，包括 Microsoft Exchange Server 和 Lync 的许多对象填充属性。 评估基于在环境中的产品的组合是首选方法，但该练习的数学计算出详细说明和测试所有的精确估计值，但最大的环境实际上不必对其进行大量时间和精力。
>- 请确保 110%的 NTDS.dit 大小是可用空间作为可用的以便启用脱机碎片整理，并通过三到五年硬件生命周期规划的增长。 给定多么便宜的存储是的估计存储在 DIT 的大小随着存储分配是安全以适应增长和可能需要脱机碎片整理的 300%。

#### <a name="virtualization-considerations-for-storage"></a>存储的虚拟化注意事项

正在分配一个卷的多个虚拟硬盘 (VHD) 文件的方案中使用固定大小至少为 210%（DIT + 110%可用空间的 100%) 的磁盘以确保有足够的空间保留的 dit 大小。  

#### <a name="calculation-summary-example"></a>计算摘要示例

|从评估阶段收集的数据| |
|-|-|
|NTDS.dit 大小|35 GB|
|修饰符以允许脱机碎片整理|2.1|
|所需的总存储空间|73.5 GB|

> [!NOTE]
> 需要此存储不包括 SYSVOL、 操作系统、 页面文件、 临时文件、 本地缓存的数据 （如安装程序文件） 和应用程序所需的存储。

### <a name="storage-performance"></a>存储性能

#### <a name="evaluating-performance-of-storage"></a>评估存储性能

在任何计算机中最慢的组件，作为存储会对客户端体验的最大的负面影响。 对于那些大型的环境足够的 RAM 大小调整建议为不可行，则后果的忽视规划性能的存储可能极具破坏性。  此外的复杂性和各种存储技术进一步增加失败的风险非常有用的方案中有限的长期的合作的最佳做法的单独的物理磁盘上的"put 操作的系统、 日志和数据库"相关性。  这是因为长期的合作的最佳做法基于是"磁盘"是专用的心轴，这允许 I/O 隔离的假设。  将此项，则返回 true 此假设不再相关的引入：

- 新的存储类型和虚拟化和共享存储方案
- 共享轴上存储区域网络 (SAN)
- 在 SAN 或网络附加存储的 VHD 文件
- 固态硬盘
- 分层的存储体系结构 （即 SSD 存储层缓存较大的心轴基于存储空间）

简单地说，所有的存储性能工作，而不考虑基础存储体系结构和设计，最终目标是确保所需的数量的输入/输出操作每秒 (IOPS) 可用，并且可接受的时间范围内发生这些 IOPS. 本部分介绍如何评估正确设计的基础存储哪些 AD DS 需求，以确保存储解决方案。  鉴于当今的存储技术的可变性，最好使用存储供应商，以确保有足够的 IOPS。  对于与本地附加存储这些情况下，参考，如何设计传统的本地存储方案中的基础知识附录 C。  此主体通常适用于更复杂的存储层，并支持后端存储解决方案供应商还有助于在对话框中。

- 提供广泛可用的存储选项，它被建议吸引硬件支持团队或供应商以确保特定的解决方案满足 AD DS 需要的专业知识。 以下数字是将存储专家提供的信息。

对于太大，无法在 RAM 中保留数据库所在的环境，请使用性能计数器来确定多少 I/O 需要支持：

- 逻辑磁盘使用情况 (\*) \Avg Disk sec/Read (例如，如果 NTDS.dit 存储位于 d: /驱动器，完整路径将为逻辑磁盘 （d:） \Avg Disk sec/Read)
- LogicalDisk(\*)\Avg Disk sec/Write
- 逻辑磁盘使用情况 (\*) \Avg Disk sec/Transfer
- LogicalDisk(\*)\Reads/sec
- LogicalDisk(\*)\Writes/sec
- LogicalDisk(\*)\Transfers/sec

这些应在 15/30/60 分钟的时间间隔指定基准当前环境的需求进行采样。

#### <a name="evaluating-the-results"></a>评估结果

> [!NOTE]
> 因为这通常是最严苛的组件，重点是从数据库读取时，相同的逻辑可以通过用逻辑磁盘使用情况应用于写入日志文件 ( *\<NTDS 日志\>* ) \Avg Disk sec/Write逻辑磁盘使用情况和 ( *\<NTDS 日志\>* ) \Writes/sec):
>  
> - 逻辑磁盘 ( *\<NTDS\>* ) \Avg Disk sec/Read 指示是否当前存储足够的大小。  如果结果为磁盘类型，逻辑磁盘的磁盘访问时间大致相同 ( *\<NTDS\>* ) \Reads/sec 是有效的度量值。  检查后端上存储的制造商规格，但很好的逻辑磁盘使用情况的范围 ( *\<NTDS\>* ) \Avg Disk sec/Read 将大致为：
>   - 7200 – 9 到 12.5 毫秒 (ms)
>   - 10,000 – 6 到 10 毫秒
>   - ms 15,000 – 4 至 6  
>   - SSD – 1 到 3 毫秒  
>   - >[!NOTE]
>     >建议存在指出存储性能下降在 （具体取决于源） 时间为 20 毫秒。  上述值和其他指南的区别是上述值是正常操作范围。  其他建议故障排除指南来确定客户端体验显著降低，以及会变得明显。  有关更深入说明参考附录 C。
> - 逻辑磁盘 ( *\<NTDS\>* ) \Reads/sec 是正在执行的 I/O 量。
>   - 如果逻辑磁盘 ( *\<NTDS\>* ) 的后端存储，逻辑磁盘使用情况的最佳范围内是 \Avg Disk sec/Read ( *\<NTDS\>* ) \Reads/可以使用秒直接进行调整。
>   - 如果逻辑磁盘 ( *\<NTDS\>* ) \Avg Disk sec/Read 不是在后端存储的最佳范围内，额外的 I/O 需要根据以下公式：
>     > (逻辑磁盘 ( *\<NTDS\>* ) \Avg Disk sec/Read) &divide; （物理媒体磁盘访问时间） &times; (逻辑磁盘使用情况 ( *\<NTDS\>* )\Avg disk sec/Read)

注意事项：

- 请注意，是否将服务器配置具有非最优的 RAM 量，这些值将为进行规划不准确。  它们将会错误地高端，并且仍可用作坏的情况。
- 添加/优化 RAM 专门将驱动器读取 I/O 量减少 (逻辑磁盘 ( *\<NTDS\>* ) \Reads/Sec。 这意味着存储解决方案可能没有为不如最初计算可靠。  遗憾的是，任何内容不是此通用声明为宗旨依赖于客户端负载和通用准则的更多特定于无法提供。  最好是调整存储大小调整后优化 RAM。

#### <a name="virtualization-considerations-for-performance"></a>虚拟化的性能注意事项

与前面的虚拟化讨论的所有类似，此处的关键是确保基础共享基础结构可支持 DC 负载以及其他资源使用基础共享媒体和它的所有途径。 这是 true 物理域控制器是否共享相同的基础媒体 SAN、 NAS 或 iSCSI 基础结构上与其他服务器或应用程序，无论它是使用传递到共享的 SAN、 NAS 或 iSCSI 基础结构的访问的来宾基础媒体，或者如果来宾使用本地共享的媒体或 SAN、 NAS 或 iSCSI 的基础结构上驻留的 VHD 文件。 规划练习就是让确保基础媒体能够支持所有使用者的总负载。

此外，从来宾的角度，因为有额外的代码必须遍历的路径不会对无需完成整个主机可以访问任何存储的性能影响。 毫无疑问，存储性能测试表明，虚拟化是一个主观问题到主机系统的处理器使用率的吞吐量会影响 （请参阅附录 a:CPU 大小调整条件），这显然会受到来宾所需的主机的资源。 这可能会导致虚拟化注意事项处理需求在虚拟化方案中 (请参阅[处理的虚拟化注意事项](#virtualization-considerations-for-processing))。

为了使这更复杂就有了各种不同的存储选项可用，所有具有不同的性能影响。 作为安全估计从物理到虚拟迁移时，使用 1.10 的乘数调整为不同的存储选项对于虚拟化来宾上的 HYPER-V，例如传递存储 SCSI 适配器或 IDE。 不同的存储方案之间传输时需要进行的调整是关于是否为本地存储、 SAN、 NAS 或 iSCSI 不相关。

#### <a name="calculation-summary-example"></a>计算摘要示例

确定系统在正常操作情况下正常运行所需的 I/O 量：

- 逻辑磁盘 ( *\<NTDS 数据库驱动器\>* ) \Transfers/sec 高峰时段 15 分钟的时间 
- 若要确定所需的存储位置超出基础存储的容量的 I/O 量：
  >*Needed IOPS* = (LogicalDisk( *\<NTDS Database Drive\>* )\Avg Disk sec/Read &divide; *\<Target Avg Disk sec/Read\>* ) &times; LogicalDisk( *\<NTDS Database Drive\>* )\Read/sec

|计数器|ReplTest1|
|-|-|
|实际逻辑磁盘 ( *\<NTDS 数据库驱动器\>* ) \Avg Disk sec/Transfer|.02 秒 （20 毫秒）|
|面向逻辑磁盘 ( *\<NTDS 数据库驱动器\>* ) \Avg Disk sec/Transfer|.01 秒|
|乘数中可用的 IO 的行为更改|0.02 &divide; 0.01 = 2|  
  
|值名称|值|
|-|-|
|LogicalDisk( *\<NTDS Database Drive\>* )\Transfers/sec|400|
|乘数中可用的 IO 的行为更改|2|
|在高峰期期间所需的总 IOPS|800|

若要确定的缓存想要上做好准备的速率：

- 确定的最大可接受的时间来预热缓存。 需要从磁盘加载整个数据库的时间量或对于整个数据库不能加载在 RAM 中的位置的情况下，这将是填充 RAM 的最长时间。
- 确定数据库，不包括空白区域的大小。  有关详细信息，请参阅[评估存储](#evaluating-for-storage)。  
- 数据库大小除以 8 KB;将加载数据库所需的总 Io。
- 定义的时间范围中的秒数除以总 Io。

请注意因为以前加载的页被逐出如果 ESE 未配置为具有固定的高速缓存的大小，并且默认情况下 AD DS 使用变量的缓存大小不会精确计算，而准确的速率。

|若要收集的数据点|值
|-|-|
|最大可接受的时间来预热|10 分钟 （600 秒）
|数据库大小|2 GB|  
  
|计算步骤|公式|结果|
|-|-|-|
|计算页中的数据库的大小|(2 GB &times; 1024年&times;1024年) =*以 kb 为单位的数据库的大小*|2,097,152 KB|
|计算数据库中的页数|不能超过 2,097,152 KB &divide; 8KB =*的页数*|262,144 页|
|计算完全预热高速缓存所需的 IOPS|262,144 页面&divide;600 秒 =*所需的 IOPS*|437 IOPS|

## <a name="processing"></a>正在处理

### <a name="evaluating-active-directory-processor-usage"></a>评估 Active Directory 处理器使用情况

对于大多数环境中的规划部分中，所述正确优化存储、 RAM 和网络后管理的处理容量将是值得我们大多数的关注的组件。 在评估所需的 CPU 容量有两个难题：

- 是否在环境中的应用程序正在良好在共享的服务基础结构，以及一文中标题为"跟踪成本和效率低下的搜索"部分中讨论创建更多有效 Microsoft 活动已启用目录的应用程序或从下层 SAM 迁移调用 LDAP 调用。  
  
  在大型环境中，这一点很重要的原因是效果不佳编码应用程序可以驱动器中的 CPU 负载的波动性、"窃取"从其他应用程序的 CPU 时间很长，人为地提高容量需求和均匀分发负载在域控制器。  
- AD DS 是分布式的环境中使用大量不同的潜在客户端，如估计费用"单个客户端"是宗旨主观由于使用模式的类型或利用 AD DS 的应用程序的数量。 简单地说，类似于网络部分为广泛的适用性，这是更好地被解决从评估环境中所需的总容量的角度来看。

对于现有环境中，存储大小调整已讨论过，因为假设是进行存储现在适当调整大小，因此有关处理器负载数据是有效的。 重申一下，务必确保系统中的瓶颈不是存储区的性能。 当存在瓶颈和等待处理器时，有一旦删除瓶颈会消失的空闲状态。  删除了处理器等待状态，则根据定义，因为它不再具有要等待的数据会增加 CPU 使用率。 因此，收集性能计数器"逻辑磁盘 ( *\<NTDS 数据库驱动器\>* ) \Avg Disk sec/Read"和"Process(lsass)\\%Processor Time"。 中的数据"Process(lsass)\\%Processor Time"将是较低如果"逻辑磁盘 ( *\<NTDS 数据库驱动器\>* ) \Avg Disk sec/Read"超过了 10 到 15 毫秒，这是一个常规的阈值，Microsoft 支持部门使用与存储相关的性能问题进行疑难解答。 与前面一样，建议采样间隔为 15、 30 或 60 分钟。 任何小于通常会为准确地衡量; 太易失性更高版本的任何内容将过消除每日查看。

### <a name="introduction"></a>简介

规划域控制器的容量规划，以便处理能力要求最大关注和理解。 调整系统以确保最大性能的大小时，没有始终是瓶颈的组件，在适当大小的域控制器，这将是处理器。

类似于其中的环境需查看基于站点的站点的网络部分，必须实现相同的所需的计算能力。 与不同的网络部分，其中可用的网络技术远远超过了正常的需求，更注重调整大小的 CPU 容量。  作为任何环境甚至适中的大小;通过几个数千个并发用户的任何内容可以在 CPU 上放置大量负载。

遗憾的是，由于利用 AD 客户端应用程序的巨大变化，一般估计每个 CPU 的用户是了解针对一些不适用于所有环境。 具体而言，计算需求受到用户行为和应用程序配置文件。 因此，每个环境需要单独进行大小调整。

#### <a name="target-site-behavior-profile"></a>目标站点的行为配置文件

如前所述，规划容量为整个站点时，目标是为目标是图案中的带有*N* + 1 容量设计，如此一来，高峰时段的某个系统故障的可实现的服务，一个合理的延续质量级别。 这意味着，在"*N*"方案中，跨所有框负载应小于 100%（更确切地说，小于 80%）在高峰时段中。

此外，如果应用程序和站点中的客户端查找域控制器使用的最佳做法 (即，使用[DsGetDcName 函数](http://msdn.microsoft.com/en-us/library/windows/desktop/ms675983(v=vs.85).aspx))，应使用次要较平均地分布客户端任意数量的因素由于暂时性的峰值。

在下一步的示例中，进行以下假设：

- 每个站点中的五个域控制器具有四个 Cpu。
- 总目标在营业时间内的 CPU 使用率为 40%，在正常操作状态 ("*N* + 1")，否则为 60%("*N*")。 在非工作时间，因为备份软件和其他维护应使用所有可用资源，目标 CPU 使用情况为 80%。

![CPU 使用情况图表](media/capacity-planning-considerations-cpu-chart.png)

分析图表中的数据 (处理器 Information(_Total)\%处理器实用程序) 为每个域控制器：

- 大多数情况下，负载较平均地分布这是当客户端使用 DC 定位器和具有写得好的搜索将预期的方式。 
- 有数的 10%，某些尽可能大的五分钟高峰为 20%。 通常情况下，除非它们会导致超过容量计划目标，调查这些不是物有所值。  
- 所有系统的高峰时段为大约上午 8:00 到上午 9:15 点。 从大约上午 5:00 通过大约下午 5:00 的平滑转换，这是通常表示业务周期。 更随机的下午 5:00 和 4:00 AM 之间的框中的框方案上的 CPU 使用率峰值是外部产能规划问题。

  >[!NOTE]
  >在管理良好的系统上所述的高峰可能是备份软件正在运行、 完整系统防病毒扫描、 硬件或软件清单、 软件或修补程序部署等。 因为它们归属峰值用户业务周期之外，未超过目标。

- 因为每个系统为大约 40%，并且所有系统具有相同数量的 Cpu，应一个失败或使其脱机，剩余的系统会运行在估计的 53%（系统 D 的 40%负载均匀拆分，添加到系统 A 和系统 C 的现有的 40%负载）。 多种原因，对于此线性假设不是非常精确，但提供了足够的准确度来衡量。  

  **备用方案 –** 运行优惠 40%的两个域控制器：一个域控制器失败，对剩余的一个估计 CPU 为估计的 80%。 这远远超出了上述容量计划的阈值，同时还启动将会严重限制的净空间的 10%到 20%的负载配置文件中更高版本，这意味着，峰值带来的 DC 到 90%到 100%期间看到"*N*"方案，肯定会降低响应能力。

### <a name="calculating-cpu-demands"></a>计算 CPU 需求

"进程\\%Processor Time"性能对象计数器计算的总金额之和的所有应用程序的线程在 CPU 上支出和系统的总数量除以时间已传递的时间。 此操作的效果是多个 CPU 的系统上的多线程应用程序可能会超过 100%的 CPU 时间，并会比有很大不同解释"处理器信息\\%处理器实用程序"。 在实践中"Process(lsass)\\%Processor Time"可以查看运行时都支持该进程的需求所需的 100%的 Cpu 数。 值为 200%意味着需要 2 个 Cpu，每个 100%，以支持完整的 AD DS 负载。 尽管在 100%的容量运行的 CPU 最经济高效从在 Cpu 上所花费的成本的角度来看，而且系统时发生功率和能耗情况，很多原因详见附录 A： 多线程系统上更好地响应能力未运行在 100%。

为了适应在客户端负载暂时性的峰值，建议为目标的峰值时间段 CPU 中的 40%到 60%的系统容量。 使用上面的示例中，这意味着，3.33 （60%的目标） 和 5 （40%的目标） 之间的 Cpu 所需的 AD DS （lsass 进程） 负载。 应根据基本操作系统和其他代理 （如防病毒、 备份、 监视、 等） 所需的需求中添加更多的容量。 尽管需要在每个环境的基础上进行评估的代理的影响，但可以进行 5%到 10%的单个 CPU 的估计值。 本示例中，这会建议，3.43 （60%的目标） 和 5.1 （40%的目标） 之间的 Cpu 都有必要在高峰期。

若要执行此操作的最简单方法是为了使用 Windows 可靠性和性能监视器 (perfmon) 中的"堆积面积图"视图，以确保所有计数器都缩放相同。

假设：

- 目标是尽可能减少到尽可能少的服务器的占用空间。 理想情况下，一台服务器将执行负载和其他服务器添加为冗余 (*N* + 1 的方案)。

![（通过所有处理器） 的 lsass 进程的处理器时间图表](media/capacity-planning-considerations-proc-time-chart.png)

从图表中数据所获得的知识 (Process(lsass)\\处理器时间百分比):

- 工作日开始向上大约 7:00 让流量和时，下午 5:00。
- 在高峰最繁忙期是从上午 9:30 到上午 11:00。 
  > [!NOTE]
  > 所有性能数据都是历史数据。 在 9:15 的峰值数据点表示为 9:15 9:00 负载。
- 7:00 AM 之前是从不同的时区的负载或后台基础结构活动，例如，备份可能指示有峰值。 在上午 9:30 峰值超过此活动，因为它不相关。
- 在站点中有三个域控制器。

在最大加载、 lsass 占用大约 485%的一个 CPU 或 4.85 运行 100%的 Cpu。 根据数学计算更早版本，这意味着站点需要大约 12.25 Cpu 用于 AD DS。 在上述建议的 5%到 10%的后台进程中添加，这意味着现在替换服务器都需要大约 12.30 到 12.35 Cpu，以支持相同的负载。 环境增长现在估计值需要一项考虑因素。

### <a name="when-to-tune-ldap-weights"></a>何时优化 LDAP 权重

有几种方案的优化[LdapSrvWeight](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10))应考虑。 这会在容量规划的上下文中，完成分配不均衡的应用程序或用户负载，或在功能方面不均匀地平衡的基础系统时。 若要执行此操作超出容量规划的原因是本文的范围之外。

有两个常见的原因来优化 LDAP 权重：

- PDC 仿真器是会影响对哪些用户或应用程序加载行为不均匀分布的每个环境的一个示例。 由于某些工具和操作面向 PDC 仿真器，例如组策略管理工具、 身份验证失败、 建立信任关系等，对于第二个尝试在 PDC 仿真器上的 CPU 资源可能会在比多地要求中的其他位置站点。
  - 仅它可用于在 CPU 使用率，以便减少 PDC 仿真器上的负载并提高其他域控制器上的负载中明显的差异将允许更平均分配负载是否对此做出调整。
  - 在这种情况下，PDC 模拟器为 50%到 75 之间设置 LDAPSrvWeight。
- 服务器使用不同的站点中的 Cpu （和速度） 的计数。  例如，假设有两个八核服务器和一台四核服务器。  最后一个服务器具有其他两个服务器的一半的处理器。  这意味着也分布式客户端负载将增加到八核框的大约两倍，四核框上的平均 CPU 负载。
  - 例如，两个八核框将运行优惠 40%和四个核心框将运行在 80%。
  - 此外，请考虑在此方案中，特别是现在将重载的四个核心框的事实的一个八核框丢失的影响。

#### <a name="example-1---pdc"></a>示例 1-PDC

| |使用默认值的利用率|New LdapSrvWeight|估计新利用率|
|-|-|-|-|
|DC 1 （PDC 仿真器）|53%|57|40%|
|DC 2|33%|100|40%|
|DC 3|33%|100|40%|

在这里要注意是，如果传输或占用，尤其是对另一个域控制器在站点中，PDC 仿真器角色将会有新 PDC 仿真器上的显著增长。

使用部分中的示例[目标站点的行为配置文件](#target-site-behavior-profile)，假设已在站点中的所有三个域控制器有四个 Cpu。 应执行的操作，正常情况下，如果一个域控制器有八个 Cpu？ 将有两个域控制器以 40%的利用率，另一个以 20%的利用率。 尽管这不是错误的没有以平衡负载有点更好的机会。 利用 LDAP 权重来实现此目的。  是一个示例方案：

#### <a name="example-2---differing-cpu-counts"></a>示例 2-级别不同的 CPU 计数

| |处理器信息\\ %&nbsp;处理器 Utility(_Total)<br />使用默认值的利用率|New LdapSrvWeight|估计新利用率|
|-|-|-|-|
|4 个 CPU DC 1|40|100|30%|
|4-CPU DC 2|40|100|30%|
|8 个 CPU DC 3|20|200|30%|

但为实现这些方案非常小心。 如上文所见，真的很好和美观纸上所呈现的数学计算。 在整篇文章，规划，但"*N* + 1" 方案是极其重要。 必须为每个方案计算一个 DC 进入脱机状态的影响。 在其中的负载分布为偶数，为了确保 60%的前面紧邻方案中加载期间"*N*"方案中，所有服务器之间均匀地平衡负载分布就可以按比例保持一致。 在优化方案中，PDC 仿真器和常规任何方案的用户或应用程序负载不均衡，效果是大不相同：

| |优化的使用率|New LdapSrvWeight|估计新利用率|
|-|-|-|-|
|DC 1 （PDC 仿真器）|40%|85|47%|
|DC 2|40%|100|53%|
|DC 3|40%|100|53%|

### <a name="virtualization-considerations-for-processing"></a>处理的虚拟化注意事项

存在两个层的容量规划，需要在虚拟化环境中。 在主机级别，类似于业务周期为域控制器之前，处理所述的标识需要标识高峰时段的阈值。 因为基础主体是相同的计划来宾线程与物理计算机上在 CPU 上获取 AD DS 线程在 CPU 上的主机，建议使用相同的目标 40%到 60%的基础的主机上。 在下一层上，来宾层的主体的线程计划以来未进行更改，该来宾将保持在 60%到 40%范围内的目标。

在直接映射方案中，每个主机，一个来宾到目前为止完成的所有容量规划都需求添加到基础主机操作系统的要求 （RAM、 磁盘、 网络）。 在共享的宿主方案中，测试表明基础处理器的效率是 10%的影响。 这意味着如果站点需要在目标的 40%，建议的虚拟 Cpu 以跨所有分配量的 10 个 Cpu"*N*"来宾将等于 11。 在使用混合分布的物理服务器和虚拟服务器站点中，修饰符仅适用于 Vm。 例如，如果站点具有"*N* + 1" 的方案，有 10 个 Cpu 的一台物理或直接映射服务器是有关等效于 11 cpu 的主机上为域控制器保留的 11 个 Cpu 使用一个来宾。

在整个分析和必要的 CPU 数量的计算来支持 AD DS 负载映射到什么可购买的条款物理硬件的 Cpu 数不一定是映射完全。 虚拟化消除了需要向上舍入。 虚拟化会降低计算能力添加到站点，则给出与 CPU 可以添加到 VM 的易用性所需的工作量。 它不会消除需要准确地评估，以便基础硬件都可供更多的 Cpu 使用要添加到来宾所需的计算能力。  与往常一样，请记住，计划和监视需求增长时出现的登录。

### <a name="calculation-summary-example"></a>计算摘要示例

|系统|峰值 CPU|
|-|-|-|
|DC 1|120%|
|DC 2|147%|
|Dc 3|218%|
|正在使用的总 CPU|485%|  
  
|目标系统计数|从上面的总带宽|
|-|-|
|在 40%的目标所需的 Cpu|4.85 &divide; .4 = 12.25|

由于此点的重要性重复*记住所规划增长*。 假设 50%的增长下一步的三年来，此环境将需要 18.375 Cpu (12.25 &times; 1.5) 在三年标记处。 是一个备用计划第一年后，检查并根据需要添加更多的容量中。

### <a name="cross-trust-client-authentication-load-for-ntlm"></a>Ntlm 跨信任客户端身份验证负载

#### <a name="evaluating-cross-trust-client-authentication-load"></a>评估跨信任客户端身份验证负载

许多环境中可能有一个或多个域通过信任连接。 在不使用 Kerberos 身份验证的另一个域中基于标识的身份验证请求需要遍历信任使用到另一个域控制器在目标域或在路径中的下一个域中域控制器的安全通道到目标域中。 由称为设置控制使用的域控制器可以对受信任的域中的域控制器进行的安全通道的并发调用数**MaxConcurrentAPI**。 对于域控制器，确保安全通道可以处理的负载量通过两种方法之一： 优化**MaxConcurrentAPI**或林中创建快捷方式信任。 若要通过单个信任仪表的流量，请参阅[如何执行性能调整为 NTLM 身份验证使用 MaxConcurrentApi 设置](https://support.microsoft.com/kb/2688798)。

数据收集过程，与所有其他情况下，必须收集在高峰期间忙碌的一天的数据很有用。

> [!NOTE]
> 林内和林间方案可能会导致遍历多个信任的身份验证和每个阶段可能需要进行调整。

#### <a name="planning"></a>计划

有多种默认情况下，使用 NTLM 身份验证或使用它在某些方案中配置的应用程序。 应用程序服务器在容量增大且越来越多的活动客户端提供服务。 此外，还有一种趋势，客户端保持会话在有限时间中打开，并定期 （例如电子邮件请求同步） 而不是重新连接。 针对高 NTLM 负载的另一个常见示例是要求身份验证进行 Internet 访问的 web 代理服务器。

这些应用程序可能会导致大量负载进行 NTLM 身份验证，可以将重点强调放置在域控制器，，尤其是当用户和资源处于不同域中。

有多种方法管理跨信任负载，用于在实践中结合使用，而不是在排他要么 / 或方案。 可能的选项为：

- 通过查找用户的用户是驻留在同一域中使用的服务减少跨信任客户端身份验证。
- 增加安全通道可用的数量。 这是与在林内和跨林通信相关，名为快捷方式信任。
- 优化的默认设置**MaxConcurrentAPI**。

有关优化**MaxConcurrentAPI**等式是现有的服务器上：

> *New_MaxConcurrentApi_setting* &ge; (*semaphore_acquires* + *semaphore_time-outs*) &times; *average_semaphore_hold_time* &divide; *time_collection_length*

有关详细信息，请参阅[知识库文章 2688798:如何执行性能调整为 NTLM 身份验证使用 MaxConcurrentApi 设置](http://support.microsoft.com/kb/2688798)。

## <a name="virtualization-considerations"></a>虚拟化注意事项

None，这是操作系统优化设置。

### <a name="calculation-summary-example"></a>计算摘要示例

|数据类型|值|
|-|-|
|信号量获取 （最低配置）|6,161|
|信号量获取 （最大值）|6,762|
|信号量超时|0|
|平均信号量的保留时间|0.012|
|收集持续时间 （秒）|1:11 分钟 （71 秒）|
|（从 KB 2688798) 的公式|((6762 &ndash; 6161) + 0) &times; 0.012 /|
|最小值**MaxConcurrentAPI**|((6762 &ndash; 6161) + 0) &times; 0.012 &divide; 71 = .101|

对于此时间段内此系统中，默认值为可接受。

## <a name="monitoring-for-compliance-with-capacity-planning-goals"></a>监视与容量规划的目标的符合性

在整篇文章，它具有已讨论过如何规划和缩放转至利用率目标。 下面是摘要图表，必须进行监视以确保系统操作在足够的容量的阈值内的建议阈值。 请记住，这些不是性能阈值，但容量规划的阈值。 操作超过这些阈值的服务器将起作用，但现在应该开始验证所有应用程序也仍然能够。 如果说是应用程序都运行正常，就可以开始评估硬件升级或其他配置更改。

|Category|性能计数器|采样间隔 /|目标|警告|
|-|-|-|-|-|
|处理器|Processor Information(_Total)\\% Processor Utility|60 min|40%|60%|
|RAM (Windows Server 2008 R2 or earlier)|Memory\Available MB|< 100 MB|N/A|< 100 MB|
|RAM (Windows Server 2012)|Memory\Long-Term Average Standby Cache Lifetime(s)|30 min|Must be tested|Must be tested|
|Network|Network Interface(\*)\Bytes Sent/sec<br /><br />Network Interface(\*)\Bytes Received/sec|30 min|40%|60%|
|Storage|LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read<br /><br />LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write|60 min|10 ms|15 ms|
|AD Services|Netlogon(\*)\Average Semaphore Hold Time|60 min|0|1 second|

## Appendix A: CPU sizing criteria

### Definitions

**Processor (microprocessor) –** a component that reads and executes program instructions  

**CPU –** Central Processing Unit  

**Multi-Core processor –** multiple CPUs on the same integrated circuit  

**Multi-CPU –** multiple CPUs, not on the same integrated circuit  

**Logical Processor –** one logical computing engine from the perspective of the operating system  

This includes hyper-threaded, one core on multi-core processor, or a single core processor.  

As today’s server systems have multiple processors, multiple multi-core processors, and hyper-threading, this information is generalized to cover both scenarios. As such, the term logical processor will be used as it represents the operating system and application perspective of the available computing engines.

### Thread-level parallelism

Each thread is an independent task, as each thread has its own stack and instructions. Because AD DS is multi-threaded and the number of available threads can be tuned by using [How to view and set LDAP policy in Active Directory by using Ntdsutil.exe](http://support.microsoft.com/kb/315071), it scales well across multiple logical processors.

### Data-level parallelism

This involves sharing data across multiple threads within one process (in the case of the AD DS process alone) and across multiple threads in multiple processes (in general). With concern to over-simplifying the case, this means that any changes to data are reflected to all running threads in all the various levels of cache (L1, L2, L3) across all cores running said threads as well as updating shared memory. Performance can degrade during write operations while all the various memory locations are brought consistent before instruction processing can continue.

### CPU speed vs. multiple-core considerations

The general rule of thumb is faster logical processors reduce the duration it takes to process a series of instructions, while more logical processors means that more tasks can be run at the same time. These rules of thumb break down as the scenarios become inherently more complex with considerations of fetching data from shared-memory, waiting on data-level parallelism, and the overhead of managing multiple threads. This is also why scalability in multi-core systems is not linear.

Consider the following analogies in these considerations: think of a highway, with each thread being an individual car, each lane being a core, and the speed limit being the clock speed.

1. If there is only one car on the highway, it doesn’t matter if there are two lanes or 12 lanes. That car is only going to go as fast as the speed limit will allow.
1. Assume that the data the thread needs is not immediately available. The analogy would be that a segment of road is shutdown. If there is only one car on the highway, it doesn’t matter what the speed limit is until the lane is reopened (data is fetched from memory).
1. As the number of cars increase, the overhead to manage the number of cars increases. Compare the experience of driving and the amount of attention necessary when the road is practically empty (such as late evening) versus when the traffic is heavy (such as mid-afternoon, but not rush hour). Also, consider the amount of attention necessary when driving on a two-lane highway, where there is only one other lane to worry about what the drivers are doing, versus a six-lane highway where one has to worry about what a lot of other drivers are doing.
   > [!NOTE]
   > The analogy about the rush hour scenario is extended in the next section: Response Time/How the System Busyness Impacts Performance.

As a result, specifics about more or faster processors become highly subjective to application behavior, which in the case of AD DS is very environmentally specific and even varies from server to server within an environment. This is why the references earlier in the article do not invest heavily in being overly precise, and a margin of safety is included in the calculations. When making budget-driven purchasing decisions, it is recommended that optimizing usage of the processors at 40% (or the desired number for the environment) occurs first, before considering buying faster processors. The increased synchronization across more processors reduces the true benefit of more processors from the linear progression (2&times; the number of processors provides less than 2&times; available additional compute power).

> [!NOTE]
> Amdahl’s Law and Gustafson’s Law are the relevant concepts here.

### Response time/How the system busyness impacts performance

Queuing theory is the mathematical study of waiting lines (queues). In queuing theory, the Utilization Law is represented by the equation:

*U* k = *B* &divide; *T*

Where *U* k is the utilization percentage, *B* is the amount of time busy, and *T* is the total time the system was observed. Translated into the context of Windows, this means the number of 100-nanosecond (ns) interval threads that are in a Running state divided by how many 100-ns intervals were available in given time interval. This is exactly the formula for calculating % Processor Utility (reference [Processor Object](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10)) and [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))).

Queuing theory also provides the formula: *N* = *U* k &divide; (1 &ndash; *U* k) to estimate the number of waiting items based on utilization ( *N* is the length of the queue). Charting this over all utilization intervals provides the following estimates to how long the queue to get on the processor is at any given CPU load.

![Queue length](media/capacity-planning-considerations-queue-length.png)

It is observed that after 50% CPU load, on average there is always a wait of one other item in the queue, with a noticeably rapid increase after about 70% CPU utilization.

Returning to the driving analogy used earlier in this section:

- The busy times of “mid-afternoon” would, hypothetically, fall somewhere into the 40% to 70% range. There is enough traffic such that one’s ability to pick any lane is not majorly restricted, and the chance of another driver being in the way, while high, does not require the level of effort to “find” a safe gap between other cars on the road.
- One will notice that as traffic approaches rush hour, the road system approaches 100% capacity. Changing lanes can become very challenging because cars are so close together that increased caution must be exercised to do so.

This is why the long term averages for capacity conservatively estimated at 40% allows for head room for abnormal spikes in load, whether said spikes transitory (such as poorly coded queries that run for a few minutes) or abnormal bursts in general load (the morning of the first day after a long weekend).

The above statement regards % Processor Time calculation being the same as the Utilization Law is a bit of a simplification for the ease of the general reader. For those more mathematically rigorous:  
- Translating the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = The number of 100-ns intervals “Idle” thread spends on the logical processor. The change in the “*X*” variable in the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calculation
  - *T* = the total number of 100-ns intervals in a given time range. The change in the “*Y*” variable in the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calculation.
  - *U* k = The utilization percentage of the logical processor by the “Idle Thread” or % Idle Time.  
- Working out the math:
  - *U* k = 1 – %Processor Time
  - %Processor Time = 1 – *U* k
  - %Processor Time = 1 – *B* / *T*
  - %Processor Time = 1 – *X1* – *X0* / *Y1* – *Y0*

### Applying the concepts to capacity planning

The preceding math may make determinations about the number of logical processors needed in a system seem overwhelmingly complex. This is why the approach to sizing the systems is focused on determining maximum target utilization based on current load and calculating the number of logical processors required to get there. Additionally, while logical processor speeds will have a significant impact on performance, cache efficiencies, memory coherence requirements, thread scheduling and synchronization, and imperfectly balanced client load will all have significant impacts on performance that will vary on a server-by-server basis. With the relatively cheap cost of compute power, attempting to analyze and determine the perfect number of CPUs needed becomes more an academic exercise than it does provide business value.

Forty percent is not a hard and fast requirement, it is a reasonable start. Various consumers of Active Directory require various levels of responsiveness. There may be scenarios where environments can run at 80% or 90% utilization as a sustained average, as the increased wait times for access to the processor will not noticeably impact client performance. It is important to re-iterate that there are many areas in the system that are much slower than the logical processor in the system, including access to RAM, access to disk, and transmitting the response over the network. All of these items need to be tuned in conjunction. Examples:

- Adding more processors to a system running 90% that is disk-bound is probably not going to significantly improve performance. Deeper analysis of the system will probably identify that there are a lot of threads that are not even getting on the processor because they are waiting on I/O to complete.
- Resolving the disk-bound issues potentially means that threads that were previously spending a lot of time in a waiting state will no longer be in a waiting state for I/O and there will be more competition for CPU time, meaning that the 90% utilization in the previous example will go to 100% (because it can not go higher). Both components need to be tuned in conjunction.
  > [!NOTE]
  > Processor Information(*)\\% Processor Utility can exceed 100% with systems that have a "Turbo" mode.  This is where the CPU exceeds the rated processor speed for short periods.  Reference CPU manufacturers documentation and description of the counter for greater insight.  

Discussing whole system utilization considerations also brings into the conversation domain controllers as virtualized guests. [Response time/How the system busyness impacts performance](#response-timehow-the-system-busyness-impacts-performance) applies to both the host and the guest in a virtualized scenario. This is why in a host with only one guest, a domain controller (and generally any system) has near the same performance it does on physical hardware. Adding additional guests to the hosts increases the utilization of the underlying host, thereby increasing the wait times to get access to the processors as explained previously. In short, logical processor utilization needs to be managed at both the host and at the guest levels.

Extending the previous analogies, leaving the highway as the physical hardware, the guest VM will be analogized with a bus (an express bus that goes straight to the destination the rider wants). Imagine the following four scenarios:

- It is off hours, a rider gets on a bus that is nearly empty, and the bus gets on a road that is also nearly empty. As there is no traffic to contend with, the rider has a nice easy ride and gets there just as fast as if the rider had driven instead. The rider’s travel times are still constrained by the speed limit.
- It is off hours so the bus is nearly empty but most of the lanes on the road are closed, so the highway is still congested. The rider is on an almost-empty bus on a congested road. While the rider does not have a lot of competition in the bus for where to sit, the total trip time is still dictated by the rest of the traffic outside.
- It is rush hour so the highway and the bus are congested. Not only does the trip take longer, but getting on and off the bus is a nightmare because people are shoulder to shoulder and the highway is not much better. Adding more busses (logical processors to the guest) does not mean they can fit on the road any more easily, or that the trip will be shortened.
- The final scenario, though it may be stretching the analogy a little, is where the bus is full, but the road is not congested. While the rider will still have trouble getting on and off the bus, the trip will be efficient after the bus is on the road. This is the only scenario where adding more busses (logical processors to the guest) will improve guest performance.

From there it is relatively easy to extrapolate that there are a number of scenarios in between the 0%-utilized and the 100%-utilized state of the road and the 0%- and 100%-utilized state of the bus that have varying degrees of impact.

Applying the principals above of 40% CPU as reasonable target for the host as well as the guest is a reasonable start for the same reasoning as above, the amount of queuing.

## Appendix B: Considerations regarding different processor speeds, and the effect of processor power management on processor speeds

Throughout the sections on processor selection the assumption is made that the processor is running at 100% of clock speed the entire time the data is being collected and that the replacement systems will have the same speed processors. Despite both assumptions in practice being false, particularly with Windows Server 2008 R2 and later, where the default power plan is **Balanced**, the methodology still stands as it is the conservative approach. While the potential error rate may increase, it only increases the margin of safety as processor speeds increase.

- For example, in a scenario where 11.25 CPUs are demanded, if the processors were running at half speed when the data was collected, the more accurate estimate might be 5.125 &divide; 2.
- It is impossible to guarantee that doubling the clock speeds would double the amount of processing that happens for a given time period. This is due to the fact the amount of time that the processors spend waiting on RAM or other system components could stay the same. The net effect is that the faster processors might spend a greater percentage of the time idle while waiting on data to be fetched. Again, it is recommended to stick with the lowest common denominator, being conservative, and avoid trying to calculate a potentially false level of accuracy by assuming a linear comparison between processor speeds.

Alternatively, if processor speeds in replacement hardware are lower than current hardware, it would be safe to increase the estimate of processors needed by a proportionate amount. For example, it is calculated that 10 processors are needed to sustain the load in a site, and the current processors are running at 3.3 Ghz and replacement processors will run at 2.6 Ghz, this is a 21% decrease in speed. In this case, 12 processors would be the recommended amount.

That said, this variability would not change the Capacity Management processor utilization targets. As processor clock speeds will be adjusted dynamically based on the load demanded, running the system under higher loads will generate a scenario where the CPU spends more time in a higher clock speed state, making the ultimate goal to be at 40% utilization in a 100% clock speed state at peak. Anything less than that will generate power savings as CPU speeds will be throttled back during off peak scenarios.

> [!NOTE]
> An option would be to turn off power management on the processors (setting the power plan to **High Performance**) while data is collected. That would give a more accurate representation of the CPU consumption on the target server.

To adjust estimates for different processors, it used to be safe, excluding other system bottlenecks outlined above, to assume that doubling processor speeds doubled the amount of processing that could be performed.  Today, the internal architecture of processors is different enough between processors, that a safer way to gauge the effects of using different processors than data was taken from is to leverage the SPECint_rate2006 benchmark from Standard Performance Evaluation Corporation.

1. Find the SPECint_rate2006 scores for the processor that are in use and that plan to be used.
    1. On the website of the Standard Performance Evaluation Corporation, select **Results**, highlight **CPU2006**, and select **Search all SPECint_rate2006 results**.
    1. Under **Simple Request**, enter the search criteria for the target processor, for example **Processor Matches E5-2630 (baselinetarget)** and **Processor Matches E5-2650 (baseline)**.
    1. Find the server and processor configuration to be used (or something close, if an exact match is not available) and note the value in the **Result** and **# Cores** columns.
1. To determine the modifier use the following equation:
   >((*Target platform per-core score value*) &times; (*MHz per-core of baseline platform*)) &divide; ((*Baseline per-core score value*) &times; (*MHz per-core of target platform*))  

    Using the above example:
   >(35.83 &times; 2000) &divide; (33.75 &times; 2300) = 0.92
1. Multiply the estimated number of processors by the modifier.  In the above case to go from the E5-2650 processor to the E5-2630 processor multiply the calculated 11.25 CPUs &times; 0.92 = 10.35 processors needed.

## Appendix C: Fundamentals regarding the operating system interacting with storage

The queuing theory concepts outlined in [Response time/How the system busyness impacts performance](#response-timehow-the-system-busyness-impacts-performance) are also applicable to storage. Having a familiarity of how the operating system handles I/O is necessary to apply these concepts. In the Microsoft Windows operating system, a queue to hold the I/O requests is created for each physical disk. However, a clarification on physical disk needs to be made. Array controllers and SANs present aggregations of spindles to the operating system as single physical disks. Additionally, array controllers and SANs can aggregate multiple disks into one array set and then split this array set into multiple “partitions”, which is in turn presented to the operating system as multiple physical disks (ref. figure).

![Block spindles](media/capacity-planning-considerations-block-spindles.png)  

In this figure the two spindles are mirrored and split into logical areas for data storage (Data 1 and Data 2). These logical areas are viewed by the operating system as separate physical disks.

Although this can be highly confusing, the following terminology is used throughout this appendix to identify the different entities:

- **Spindle –** the device that is physically installed in the server.
- **Array –** a collection of spindles aggregated by controller.
- **Array partition –** a partitioning of the aggregated array
- **LUN –** an array, used when referring to SANs
- **Disk –** What the operating system observes to be a single physical disk.
- **Partition –** a logical partitioning of what the operating system perceives as a physical disk.

### Operating system architecture considerations

The operating system creates a First In/First Out (FIFO) I/O queue for each disk that is observed; this disk may be representing a spindle, an array, or an array partition. From the operating system perspective, with regard to handling I/O, the more active queues the better. As a FIFO queue is serialized, meaning that all I/Os issued to the storage subsystem must be processed in the order the request arrived. By correlating each disk observed by the operating system with a spindle/array, the operating system now maintains an I/O queue for each unique set of disks, thereby eliminating contention for scarce I/O resources across disks and isolating I/O demand to a single disk. As an exception, Windows Server 2008 introduces the concept of I/O prioritization, and applications designed to use the “Low” priority fall out of this normal order and take a back seat. Applications not specifically coded to leverage the “Low” priority default to “Normal.”

### Introducing simple storage subsystems

Starting with a simple example (a single hard drive inside a computer) a component-by-component analysis will be given. Breaking this down into the major storage subsystem components, the system consists of:

- **1 –** 10,000 RPM Ultra Fast SCSI HD (Ultra Fast SCSI has a 20 MB/s transfer rate)
- **1 –** SCSI Bus (the cable)
- **1 –** Ultra Fast SCSI Adapter
- **1 –** 32-bit 33 MHz PCI bus

Once the components are identified, an idea of how much data can transit the system, or how much I/O can be handled, can be calculated. Note that the amount of I/O and quantity of data that can transit the system is correlated, but not the same. This correlation depends on whether the disk I/O is random or sequential and the block size. (All data is written to the disk as a block, but different applications using different block sizes.) On a component-by-component basis:

- **The hard drive –** The average 10,000-RPM hard drive has a 7-millisecond (ms) seek time and a 3 ms access time. Seek time is the average amount of time it takes the read/write head to move to a location on the platter. Access time is the average amount of time it takes to read or write the data to disk, once the head is in the correct location. Thus, the average time for reading a unique block of data in a 10,000-RPM HD constitutes a seek and an access, for a total of approximately 10 ms (or .010 seconds) per block of data.

  When every disk access requires movement of the head to a new location on the disk, the read/write behavior is referred to as “random.” Thus, when all I/O is random, a 10,000-RPM HD can handle approximately 100 I/O per second (IOPS) (the formula is 1000 ms per second divided by 10 ms per I/O or 1000/10=100 IOPS).

  Alternatively, when all I/O occurs from adjacent sectors on the HD, this is referred to as sequential I/O. Sequential I/O has no seek time because when the first I/O is complete, the read/write head is at the start of where the next block of data is stored on the HD. Thus a 10,000-RPM HD is capable of handling approximately 333 I/O per second (1000 ms per second divided by 3 ms per I/O).

  >[!NOTE]
  >This example does not reflect the disk cache, where the data of one cylinder is typically kept. In this case, the 10 ms are needed on the first I/O and the disk reads the whole cylinder. All other sequential I/O is satisfied from the cache. As a result, in-disk caches might improve sequential I/O performance.
  
  So far, the transfer rate of the hard drive has been irrelevant. Whether the hard drive is 20 MB/s Ultra Wide or an Ultra3 160 MB/s, the actual amount of IOPS the can be handled by the 10,000-RPM HD is ~100 random or ~300 sequential I/O. As block sizes change based on the application writing to the drive, the amount of data that is pulled per I/O is different. For example, if the block size is 8 KB, 100 I/O operations will read from or write to the hard drive a total of 800 KB. However, if the block size is 32 KB, 100 I/O will read/write 3,200 KB (3.2 MB) to the hard drive. As long as the SCSI transfer rate is in excess of the total amount of data transferred, getting a “faster” transfer rate drive will gain nothing. See the following tables for comparison.

  | |7200 RPM 9ms seek, 4ms access|10,000 RPM 7ms seek, 3ms access|15,000 RPM 4ms seek, 2ms access
  |-|-|-|-|
  |Random I/O|80|100|150|
  |Sequential I/O|250|300|500|  
  
  |10,000 RPM drive|8 KB block size (Active Directory Jet)|
  |-|-|
  |Random I/O|800 KB/s|
  |Sequential I/O|2400 KB/s|

- **SCSI backplane (bus) –** Understanding how the “SCSI backplane (bus)”, or in this scenario the ribbon cable, impacts throughput of the storage subsystem depends on knowledge of the block size. Essentially the question would be, how much I/O can the bus handle if the I/O is in 8 KB blocks? In this scenario, the SCSI bus is 20 MB/s, or 20480 KB/s. 20480 KB/s divided by 8 KB blocks yields a maximum of approximately 2500 IOPS supported by the SCSI bus.

  >[!NOTE]
  >The figures in the following table represent an example. Most attached storage devices currently use PCI Express, which provides much higher throughput.  
  
  |I/O supported by SCSI bus per block size|2 KB block size|8 KB block size (AD Jet) (SQL Server 7.0/SQL Server 2000)
  |-|-|-|
  |20 MB/s|10,000|2,500|
  |40 MB/s|20,000|5,000|
  |128 MB/s|65,536|16,384|
  |320 MB/s|160,000|40,000|

  As can be determined from this chart, in the scenario presented, no matter what the use, the bus will never be a bottleneck, as the spindle maximum is 100 I/O, well below any of the above thresholds.

  >[!NOTE]
  >This assumes that the SCSI bus is 100% efficient.
  
- **SCSI adapter –** For determining the amount of I/O that this can handle, the manufacturer’s specifications need to be checked. Directing I/O requests to the appropriate device requires processing of some sort, thus the amount of I/O that can be handled is dependent on the SCSI adapter (or array controller) processor.

  In this example, the assumption that 1,000 I/O can be handled will be made.

- **PCI bus –** This is an often overlooked component. In this example, this will not be the bottleneck; however as systems scale up, it can become a bottleneck. For reference, a 32 bit PCI bus operating at 33Mhz can in theory transfer 133 MB/s of data. Following is the equation:  
  > 32 bits &divide; 8 bits per byte &times; 33 MHz = 133 MB/s.  

  Note that is the theoretical limit; in reality only about 50% of the maximum is actually reached, although in certain burst scenarios, 75% efficiency can be obtained for short periods.

  A 66Mhz 64-bit PCI bus can support a theoretical maximum of (64 bits &divide; 8 bits per byte &times; 66 Mhz) = 528 MB/sec. Additionally, any other device (such as the network adapter, second SCSI controller, and so on) will reduce the bandwidth available as the bandwidth is shared and the devices will contend for the limited resources.

After analysis of the components of this storage subsystem, the spindle is the limiting factor in the amount of I/O that can be requested, and consequently the amount of data that can transit the system. Specifically, in an AD DS scenario, this is 100 random I/O per second in 8 KB increments, for a total of 800 KB per second when accessing the Jet database. Alternatively, the maximum throughput for a spindle that is exclusively allocated to log files would suffer the following limitations: 300 sequential I/O per second in 8 KB increments, for a total of 2400 KB (2.4 MB) per second.

Now, having analyzed a simple configuration, the following table demonstrates where the bottleneck will occur as components in the storage subsystem are changed or added.

|Notes|Bottleneck analysis|Disk|Bus|Adapter|PCI bus|
|-|-|-|-|-|-|
|This is the domain controller configuration after adding a second disk. The disk configuration represents the bottleneck at 800 KB/s.|Add 1 disk (Total=2)<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD|200 I/Os total<br />800 KB/s total.| | | |
|After adding 7 disks, the disk configuration still represents the bottleneck at 3200 KB/s.|**Add 7 disks (Total=8)**  <br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD|800 I/Os total.<br />3200 KB/s total| | | |
|After changing I/O to sequential, the network adapter becomes the bottleneck because it is limited to 1000 IOPS.|Add 7 disks (Total=8)<br /><br />**I/O is sequential**<br /><br />4 KB block size<br /><br />10,000 RPM HD| | |2400 I/O sec can be read/written to disk, controller limited to 1000 IOPS| |
|After replacing the network adapter with a SCSI adapter that supports 10,000 IOPS, the bottleneck returns to the disk configuration.|Add 7 disks (Total=8)<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />**Upgrade SCSI adapter (now supports 10,000 I/O)**|800 I/Os total.<br />3,200 KB/s total| | | |
|After increasing the block size to 32 KB, the bus becomes the bottleneck because it only supports 20 MB/s.|Add 7 disks (Total=8)<br /><br />I/O is random<br /><br />**32 KB block size**<br /><br />10,000 RPM HD| |800 I/Os total. 25,600 KB/s (25 MB/s) can be read/written to disk.<br /><br />The bus only supports 20 MB/s| | |
|After upgrading the bus and adding more disks, the disk remains the bottleneck.|**Add 13 disks (Total=14)**<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />**Upgrade to 320 MB/s SCSI bus**|2800 I/Os<br /><br />11,200 KB/s (10.9 MB/s)| | | |
|After changing I/O to sequential, the disk remains the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI Adapter with 14 disks<br /><br />**I/O is sequential**<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />Upgrade to 320 MB/s SCSI bus|8,400 I/Os<br /><br />33,600 KB\s<br /><br />(32.8 MB\s)| | | |
|After adding faster hard drives, the disk remains the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is sequential<br /><br />4 KB block size<br /><br />**15,000 RPM HD**<br /><br />Upgrade to 320 MB/s SCSI bus|14,000 I/Os<br /><br />56,000 KB/s<br /><br />(54.7 MB/s)| | | |
|After increasing the block size to 32 KB, the PCI bus becomes the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is sequential<br /><br />**32 KB block size**<br /><br />15,000 RPM HD<br /><br />Upgrade to 320 MB/s SCSI bus| | | |14,000 I/Os<br /><br />448,000 KB/s<br /><br />(437 MB/s) is the read/write limit to the spindle.<br /><br />The PCI bus supports a theoretical maximum of 133 MB/s (75% efficient at best).|

### Introducing RAID

The nature of a storage subsystem does not change dramatically when an array controller is introduced; it just replaces the SCSI adapter in the calculations. What does change is the cost of reading and writing data to the disk when using the various array levels (such as RAID 0, RAID 1, or RAID 5).

In RAID 0, the data is striped across all the disks in the RAID set. This means that during a read or a write operation, a portion of the data is pulled from or pushed to each disk, increasing the amount of data that can transit the system during the same time period. Thus, in one second, on each spindle (again assuming 10,000-RPM drives), 100 I/O operations can be performed. The total amount of I/O that can be supported is N spindles times 100 I/O per second per spindle (yields 100*N I/O per second).

![Logical d: drive](media/capacity-planning-considerations-logical-d-drive.png)

In RAID 1, the data is mirrored (duplicated) across a pair of spindles for redundancy. Thus, when a read I/O operation is performed, data can be read from both of the spindles in the set. This effectively makes the I/O capacity from both disks available during a read operation. The caveat is that write operations gain no performance advantage in a RAID 1. This is because the same data needs to be written to both drives for the sake of redundancy. Though it does not take any longer, as the write of data occurs concurrently on both spindles, because both spindles are occupied duplicating the data, a write I/O operation in essence prevents two read operations from occurring. Thus, every write I/O costs two read I/O. A formula can be created from that information to determine the total number of I/O operations that are occurring:  

> *Read I/O* + 2 &times; *Write I/O* = *Total available disk I/O consumed*  

When the ratio of reads to writes and the number of spindles are known, the following equation can be derived from the above equation to identify the maximum I/O that can be supported by the array:  

> *Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] = *Total IOPS*

RAID 1+ 0, behaves exactly the same as RAID 1 regarding the expense of reading and writing. However, the I/O is now striped across each mirrored set. If  

> *Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] = *Total I/O*  

in a RAID 1 set, when a multiplicity (*N*) of RAID 1 sets are striped, the Total I/O that can be processed becomes N &times; I/O per RAID 1 set:  

> *N* &times; {*Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] } = *Total IOPS*

In RAID 5, sometimes referred to as *N* + 1 RAID, the data is striped across *N* spindles and parity information is written to the “+ 1” spindle. However, RAID 5 is much more expensive when performing a write I/O than RAID 1 or 1 + 0. RAID 5 performs the following process every time a write I/O is submitted to the array:

1. Read the old data
1. Read the old parity
1. Write the new data
1. Write the new parity

As every write I/O request that is submitted to the array controller by the operating system requires four I/O operations to complete, write requests submitted take four times as long to complete as a single read I/O. To derive a formula to translate I/O requests from the operating system perspective to that experienced by the spindles:  

> *Read I/O* + 4 &times; *Write I/O* = *Total I/O*  

Similarly in a RAID 1 set, when the ratio of reads to writes and the number of spindles are known, the following equation can be derived from the above equation to identify the maximum I/O that can be supported by the array (Note that total number of spindles does not include the “drive” lost to parity):  

> *IOPS per spindle* &times; (*Spindles* – 1) &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 4 &times; *%Writes*)] = *Total IOPS*

### Introducing SANs

Expanding the complexity of the storage subsystem, when a SAN is introduced into the environment, the basic principles outlined do not change, however I/O behavior for all of the systems connected to the SAN needs to be taken into account. As one of the major advantages in using a SAN is an additional amount of redundancy over internally or externally attached storage, capacity planning now needs to take into account fault tolerance needs. Also, more components are introduced that need to be evaluated. Breaking a SAN down into the component parts:

- SCSI or Fibre Channel hard drive
- Storage unit channel backplane
- Storage units
- Storage controller module
- SAN switch(es)
- HBA(s)
- The PCI bus

When designing any system for redundancy, additional components are included to accommodate the potential of failure. It is very important, when capacity planning, to exclude the redundant component from available resources. For example, if the SAN has two controller modules, the I/O capacity of one controller module is all that should be used for total I/O throughput available to the system. This is due to the fact that if one controller fails, the entire I/O load demanded by all connected systems will need to be processed by the remaining controller. As all capacity planning is done for peak usage periods, redundant components should not be factored into the available resources and planned peak utilization should not exceed 80% saturation of the system (in order to accommodate bursts or anomalous system behavior). Similarly, the redundant SAN switch, storage unit, and spindles should not be factored into the I/O calculations.

When analyzing the behavior of the SCSI or Fibre Channel hard drive, the method of analyzing the behavior as outlined previously does not change. Although there are certain advantages and disadvantages to each protocol, the limiting factor on a per disk basis is the mechanical limitation of the hard drive.

Analyzing the channel on the storage unit is exactly the same as calculating the resources available on the SCSI bus, or bandwidth (such as 20 MB/s) divided by block size (such as 8 KB). Where this deviates from the simple previous example is in the aggregation of multiple channels. For example, if there are 6 channels, each supporting 20 MB/s maximum transfer rate, the total amount of I/O and data transfer that is available is 100 MB/s (this is correct, it is not 120 MB/s). Again, fault tolerance is a major player in this calculation, in the event of the loss of an entire channel, the system is only left with 5 functioning channels. Thus, to ensure continuing to meet performance expectations in the event of failure, total throughput for all of the storage channels should not exceed 100 MB/s (this assumes load and fault tolerance is evenly distributed across all channels). Turning this into an I/O profile is dependent on the behavior of the application. In the case of Active Directory Jet I/O, this would correlate to approximately 12,500 I/O per second (100 MB/s &divide; 8 KB per I/O).

Next, obtaining the manufacturer’s specifications for the controller modules is required in order to gain an understanding of the throughput each module can support. In this example, the SAN has two controller modules that support 7,500 I/O each. The total throughput of the system may be 15,000 IOPS if redundancy is not desired. In calculating maximum throughput in the case of failure, the limitation is the throughput of one controller, or 7,500 IOPS. This threshold is well below the 12,500 IOPS (assuming 4 KB block size) maximum that can be supported by all of the storage channels, and thus, is currently the bottleneck in the analysis. Still for planning purposes, the desired maximum I/O to be planned for would be 10,400 I/O.

When the data exits the controller module, it transits a Fibre Channel connection rated at 1 GB/s (or 1 Gigabit per second). To correlate this with the other metrics, 1 GB/s turns into 128 MB/s (1 GB/s &divide; 8 bits/byte). As this is in excess of the total bandwidth across all channels in the storage unit (100 MB/s), this will not bottleneck the system. Additionally, as this is only one of the two channels (the additional 1 GB/s Fibre Channel connection being for redundancy), if one connection fails, the remaining connection still has enough capacity to handle all the data transfer demanded.

En route to the server, the data will most likely transit a SAN switch. As the SAN switch has to process the incoming I/O request and forward it out the appropriate port, the switch will have a limit to the amount of I/O that can be handled, however, manufacturers specifications will be required to determine what that limit is. For example, if there are two switches and each switch can handle 10,000 IOPS, the total throughput will be 20,000 IOPS. Again, fault tolerance being a concern, if one switch fails, the total throughput of the system will be 10,000 IOPS. As it is desired not to exceed 80% utilization in normal operation, using no more than 8000 I/O should be the target.

Finally, the HBA installed in the server would also have a limit to the amount of I/O that it can handle. Usually, a second HBA is installed for redundancy, but just like with the SAN switch, when calculating maximum I/O that can be handled, the total throughput of *N* &ndash; 1 HBAs is what the maximum scalability of the system is.

### Caching considerations

Caches are one of the components that can significantly impact the overall performance at any point in the storage system. Detailed analysis about caching algorithms is beyond the scope of this article; however, some basic statements about caching on disk subsystems are worth illuminating:

- Caching does improved sustained sequential write I/O as it can buffer many smaller write operations into larger I/O blocks and de-stage to storage in fewer, but larger block sizes. This will reduce total random I/O and total sequential I/O, thus providing more resource availability for other I/O.
- Caching does not improve sustained write I/O throughput of the storage subsystem. It only allows for the writes to be buffered until the spindles are available to commit the data. When all the available I/O of the spindles in the storage subsystem is saturated for long periods, the cache will eventually fill up. In order to empty the cache, enough time between bursts, or extra spindles, need to be allotted in order to provide enough I/O to allow the cache to flush.

  Larger caches only allow for more data to be buffered. This means longer periods of saturation can be accommodated.

  In a normally operating storage subsystem, the operating system will experience improved write performance as the data only needs to be written to cache. Once the underlying media is saturated with I/O, the cache will fill and write performance will return to disk speed.

- When caching read I/O, the scenario where the cache is most advantageous is when the data is stored sequentially on the disk, and the cache can read-ahead (it makes the assumption that the next sector contains the data that will be requested next).
- When read I/O is random, caching at the drive controller is unlikely to provide any enhancement to the amount of data that can be read from the disk. Any enhancement is non-existent if the operating system or application-based cache size is greater than the hardware-based cache size.

  In the case of Active Directory, the cache is only limited by the amount of RAM.

### SSD considerations

SSDs are a completely different animal than spindle-based hard disks. Yet the two key criteria remain: “How many IOPS can it handle?” and “What is the latency for those IOPS?” In comparison to spindle-based hard disks, SSDs can handle higher volumes of I/O and can have lower latencies. In general and as of this writing, while SSDs are still expensive in a cost-per-Gigabyte comparison, they are very cheap in terms of cost-per-I/O and deserve significant consideration in terms of storage performance.

Considerations:

- Both IOPS and latencies are very subjective to the manufacturer designs and in some cases have been observed to be poorer performing than spindle based technologies. In short, it is more important to review and validate the manufacturer specs drive by drive and not assume any generalities.
- IOPS types can have very different numbers depending on whether it is read or write. AD DS services, in general, being predominantly read-based, will be less affected than some other application scenarios.
- “Write endurance” – this is the concept that SSD cells will eventually wear out. Various manufacturers deal with this challenge different fashions. At least for the database drive, the predominantly read I/O profile allows for downplaying the significance of this concern as the data is not highly volatile.

### Summary

One way to think about storage is picturing household plumbing. Imagine the IOPS of the media that the data is stored on is the household main drain. When this is clogged (such as roots in the pipe) or limited (it is collapsed or too small), all the sinks in the household back up when too much water is being used (too many guests). This is perfectly analogous to a shared environment where one or more systems are leveraging shared storage on an SAN/NAS/iSCSI with the same underlying media. Different approaches can be taken to resolve the different scenarios:

- A collapsed or undersized drain requires a full scale replacement and fix. This would be similar to adding in new hardware or redistributing the systems using the shared storage throughout the infrastructure.
- A “clogged” pipe usually means identification of one or more offending problems and removal of those problems. In a storage scenario this could be storage or system level backups, synchronized antivirus scans across all servers, and synchronized defragmentation software running during peak periods.

In any plumbing design, multiple drains feed into the main drain. If anything stops up one of those drains or a junction point, only the things behind that junction point back up. In a storage scenario, this could be an overloaded switch (SAN/NAS/iSCSI scenario), driver compatibility issues (wrong driver/HBA Firmware/storport.sys combination), or backup/antivirus/defragmentation. To determine if the storage “pipe” is big enough, IOPS and I/O size needs to be measured. At each joint add them together to ensure adequate “pipe diameter.”

## Appendix D - Discussion on storage troubleshooting - Environments where providing at least as much RAM as the database size is not a viable option

It is helpful to understand why these recommendations exist so that the changes in storage technology can be accommodated. These recommendations exist for two reasons. The first is isolation of IO, such that performance issues (that is, paging) on the operating system spindle do not impact performance of the database and I/O profiles. The second is that log files for AD DS (and most databases) are sequential in nature, and spindle-based hard drives and caches have a huge performance benefit when used with sequential I/O as compared to the more random I/O patterns of the operating system and almost purely random I/O patterns of the AD DS database drive. By isolating the sequential I/O to a separate physical drive, throughput can be increased. The challenge presented by today’s storage options is that the fundamental assumptions behind these recommendations are no longer true. In many virtualized storage scenarios, such as iSCSI, SAN, NAS, and Virtual Disk image files, the underlying storage media is shared across multiple hosts, thus completely negating both the “isolation of IO” and the “sequential I/O optimization” aspects. In fact these scenarios add an additional layer of complexity in that other hosts accessing the shared media can degrade responsiveness to the domain controller.

In planning storage performance, there are three categories to consider: cold cache state, warmed cache state, and backup/restore. The cold cache state occurs in scenarios such as when the domain controller is initially rebooted or the Active Directory service is restarted and there is no Active Directory data in RAM. Warm cache state is where the domain controller is in a steady state and the database is cached. These are important to note as they will drive very different performance profiles, and having enough RAM to cache the entire database does not help performance when the cache is cold. One can consider performance design for these two scenarios with the following analogy, warming the cold cache is a “sprint” and running a server with a warm cache is a “marathon.”

For both the cold cache and warm cache scenario, the question becomes how fast the storage can move the data from disk into memory. Warming the cache is a scenario where, over time, performance improves as more queries reuse data, the cache hit rate increases, and the frequency of needing to go to disk decreases. As a result the adverse performance impact of going to disk decreases. Any degradation in performance is only transient while waiting for the cache to warm and grow to the maximum, system-dependent allowed size. The conversation can be simplified to how quickly the data can be gotten off of disk, and is a simple measure of the IOPS available to Active Directory, which is subjective to IOPS available from the underlying storage. From a planning perspective, because warming the cache and backup/restore scenarios happen on an exceptional basis, normally occur off hours, and are subjective to the load of the DC, general recommendations do not exist except in that these activities be scheduled for non-peak hours.

AD DS, in most scenarios, is predominantly read IO, usually a ratio of 90% read/10% write. Read I/O often tends to be the bottleneck for user experience, and with write IO, causes write performance to degrade. As I/O to the NTDS.dit is predominantly random, caches tend to provide minimal benefit to read IO, making it that much more important to configure the storage for read I/O profile correctly.

For normal operating conditions, the storage planning goal is minimize the wait times for a request from AD DS to be returned from disk. This essentially means that the number of outstanding and pending I/O is less than or equivalent to the number of pathways to the disk. There are a variety of ways to measure this. In a performance monitoring scenario, the general recommendation is that LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read be less than 20 ms. The desired operating threshold must be much lower, preferably as close to the speed of the storage as possible, in the 2 to 6 millisecond (.002 to .006 second) range depending on the type of storage.

Example:

![Storage latency chart](media/capacity-planning-considerations-storage-latency.png)

Analyzing the chart:

- **Green oval on the left –** The latency remains consistent at 10 ms. The load increases from 800 IOPS to 2400 IOPS. This is the absolute floor to how quickly an I/O request can be processed by the underlying storage. This is subject to the specifics of the storage solution.
- **Burgundy oval on the right –** The throughput remains flat from the exit of the green circle through to the end of the data collection while the latency continues to increase. This is demonstrating that when the request volumes exceed the physical limitations of the underlying storage, the longer the requests spend sitting in the queue waiting to be sent out to the storage subsystem.

Applying this knowledge:

- **Impact to a user querying membership of a large group –** Assume this requires reading 1 MB of data from the disk, the amount of I/O and how long it takes can be evaluated as follows:
  - Active Directory database pages are 8 KB in size. 
  - A minimum of 128 pages need to be read in from disk. 
  - Assuming nothing is cached, at the floor (10 ms) this is going to take a minimum 1.28 seconds to load the data from disk in order to return it to the client. At 20 ms, where the throughput on storage has long since maxed out and is also the recommended maximum, it will take 2.5 seconds to get the data from disk in order to return it to the end user.  
- **At what rate will the cache be warmed –** Making the assumption that the client load is going to maximize the throughput on this storage example, the cache will warm at a rate of 2400 IOPS &times; 8 KB per IO. Or, approximately 20 MB/s per second, loading about 1 GB of database into RAM every 53 seconds.

> [!NOTE]
> It is normal for short periods to observe the latencies climb when components aggressively read or write to disk, such as when the system is being backed up or when AD DS is running garbage collection. Additional head room on top of the calculations should be provided to accommodate these periodic events. The goal being to provide enough throughput to accommodate these scenarios without impacting normal function.

As can be seen, there is a physical limit based on the storage design to how quickly the cache can possibly warm. What will warm the cache are incoming client requests up to the rate that the underlying storage can provide. Running scripts to “pre-warm” the cache during peak hours will provide competition to load driven by real client requests. That can adversely affect delivering data that clients need first because, by design, it will generate competition for scarce disk resources as artificial attempts to warm the cache will load data that is not relevant to the clients contacting the DC.处理器 Information(_Total)\\处理器实用程序 Domain Services
description: Detailed discussion of the factors to consider during capacity planning for AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
---

# Capacity planning for Active Directory Domain Services

This topic is originally written by Ken Brumfield, Senior Premier Field Engineer at Microsoft, and provides recommendations for capacity planning for Active Directory Domain Services (AD DS).

## Goals of capacity planning

Capacity planning is not the same as troubleshooting performance incidents. They are closely related, but quite different. The goals of capacity planning are:  

- Properly implement and operate an environment 
- Minimize the time spent troubleshooting performance issues.  
  
In capacity planning, an organization might have a baseline target of 40% processor utilization during peak periods in order to meet client performance requirements and accommodate the time necessary to upgrade the hardware in the datacenter. Whereas, to be notified of abnormal performance incidents, a monitoring alert threshold might be set at 90% over a 5 minute interval.

The difference is that when a capacity management threshold is continually exceeded (a one-time event is not a concern), adding capacity (that is, adding in more or faster processors) would be a solution or scaling the service across multiple servers would be a solution. Performance alert thresholds indicate that client experience is currently suffering and immediate steps are needed to address the issue.

As an analogy: capacity management is about preventing a car accident (defensive driving, making sure the brakes are working properly, and so on) whereas performance troubleshooting is what the police, fire department, and emergency medical professionals do after an accident. This is about “defensive driving,” Active Directory-style.

Over the last several years, capacity planning guidance for scale-up systems has changed dramatically. The following changes in system architectures have challenged fundamental assumptions about designing and scaling a service:

- 64-bit server platforms  
- Virtualization  
- Increased attention to power consumption  
- SSD storage  
- Cloud scenarios  

Additionally, the approach is shifting from a server-based capacity planning exercise to a service-based capacity planning exercise. Active Directory Domain Services (AD DS), a mature distributed service that many Microsoft and third-party products use as a backend, becomes one the most critical products to plan correctly to ensure the necessary capacity for other applications to run.

### Baseline requirements for capacity planning guidance

Throughout this article, the following baseline requirements are expected:

- Readers have read and are familiar with [Performance Tuning Guidelines for Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- The Windows Server platform is an x64 based architecture. But even if your Active Directory environment is installed on Windows Server 2003 x86 (now beyond the end of the support lifecycle) and has a directory information tree (DIT) that is less 1.5 GB in size and that can easily be held in memory, the guidelines from this article are still applicable.
- Capacity planning is a continuous process and you should regularly review how well the environment is meeting expectations.
- Optimization will occur over multiple hardware lifecycles as hardware costs change. For example, memory becomes cheaper, the cost per core decreases, or the price of different storage options change.
- Plan for the peak busy period of the day. It is recommended to look at this in either 30 minute or hour intervals. Anything greater may hide the actual peaks and anything less may be distorted by “transient spikes.”
- Plan for growth over the course of the hardware lifecycle for the enterprise. This may include a strategy of upgrading or adding hardware in a staggered fashion, or a complete refresh every three to five years. Each will require a “guess” as how much the load on Active Directory will grow. Historical data, if collected, will help with this assessment. 
- Plan for fault tolerance. Once an estimate *N* is derived, plan for scenarios that include *N* &ndash; 1, *N* &ndash; 2, *N* &ndash; *x*.
  - Add in additional servers according to organizational need to ensure that the loss of a single or multiple servers does not exceed maximum peak capacity estimates.
  - Also consider that the growth plan and fault tolerance plan need to be integrated. For example, if one DC is required to support the load, but the estimate is that the load will be doubled in the next year and require two DCs total, there will not be enough capacity to support fault tolerance. The solution would be to start with three DCs. One could also plan to add the third DC after 3 or 6 months if budgets are tight.

    >[!NOTE]
    >Adding Active Directory-aware applications might have a noticeable impact on the DC load, whether the load is coming from the application servers or clients.

### Three-step process for the capacity planning cycle

In capacity planning, first decide what quality of service is needed. For example, a core datacenter supports a higher level of concurrency and requires more consistent experience for users and consuming applications, which requires greater attention to redundancy and minimizing system and infrastructure bottlenecks. In contrast, a satellite location with a handful of users does not need the same level of concurrency or fault tolerance. Thus, the satellite office might not need as much attention to optimizing the underlying hardware and infrastructure, which may lead to cost savings. All recommendations and guidance herein are for optimal performance, and can be selectively relaxed for scenarios with less demanding requirements.

The next question is: virtualized or physical? From a capacity planning perspective, there is no right or wrong answer; there is only a different set of variables to work with. Virtualization scenarios come down to one of two options:

- “Direct mapping” with one guest per host (where virtualization exists solely to abstract the physical hardware from the server)
- “Shared host”

Testing and production scenarios indicate that the “direct mapping” scenario can be treated identically to a physical host. “Shared host,” however, introduces a number of considerations spelled out in more detail later. The “shared host” scenario means that AD DS is also competing for resources, and there are penalties and tuning considerations for doing so.

With these considerations in mind, the capacity planning cycle is an iterative three-step process:

1. Measure the existing environment, determine where the system bottlenecks currently are, and get environmental basics necessary to plan the amount of capacity needed.
1. Determine the hardware needed according to the criteria outlined in step 1.
1. Monitor and validate that the infrastructure as implemented is operating within specifications. Some data collected in this step becomes the baseline for the next cycle of capacity planning.

### Applying the process

To optimize performance, ensure these major components are correctly selected and tuned to the application loads:

1. Memory
1. Network
1. Storage
1. Processor
1. Net Logon

The basic storage requirements of AD DS and the general behavior of well written client software allow environments with as many as 10,000 to 20,000 users to forego heavy investment in capacity planning with regards to physical hardware, as almost any modern server class system will handle the load. That said, the following table summarizes how to evaluate an existing environment in order to select the right hardware. Each component is analyzed in detail in subsequent sections to help AD DS administrators evaluate their infrastructure using baseline recommendations and environment-specific principals.

In general:

- Any sizing based on current data will only be accurate for the current environment.
- For any estimates, expect demand to grow over the lifecycle of the hardware.
- Determine whether to oversize today and grow into the larger environment, or add capacity over the lifecycle.
- For virtualization, all the same capacity planning principals and methodologies apply, except that the overhead of the virtualization needs to be added to anything domain related.
- Capacity planning, like anything that attempts to predict, is NOT an accurate science. Do not expect things to calculate perfectly and with 100% accuracy. The guidance here is the leanest recommendation; add in capacity for additional safety and continuously validate that the environment remains on target.

### Data collection summary tables

#### New environment

| Component | Estimates |
|-|-|
|Storage/Database Size|40 KB to 60 KB for each user|
|RAM|Database Size<br />Base operating system recommendations<br />Third-party applications|
|Network|1 GB|
|CPU|1000 concurrent users for each core|

#### High-level evaluation criteria

| Component | Evaluation criteria | Planning considerations |
|-|-|-|
|Storage/Database size|The section entitled “To activate logging of disk space that is freed by defragmentation” in [Storage Limits](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Storage/ Database performance|<ul><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer"</li><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Reads/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Writes/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec"</li></ul>|<ul><li>Storage has two concerns to address<ul><li>Space available, which with the size of today’s spindle based and SSD based storage is irrelevant for most AD environments.</li> <li>Input/Output (IO) Operations available – In many environments, this is often overlooked. But it is important to evaluate only environments where there is not enough RAM to load the entire NTDS Database into memory.</li></ul><li>Storage can be a complex topic and should involve hardware vendor expertise for proper sizing. Particularly with more complex scenarios such as SAN, NAS, and iSCSI scenarios. However, in general, cost per Gigabyte of storage is often in direct opposition to cost per IO:<ul><li>RAID 5 has lower cost per Gigabyte than Raid 1, but Raid 1 has lower cost per IO</li><li>Spindle-based hard drives have lower cost per Gigabyte, but SSDs have a lower cost per IO</li></ul><li>After a restart of the computer or the Active Directory Domain Services service, the Extensible Storage Engine (ESE) cache is empty and performance will be disk bound while the cache warms.</li><li>In most environments AD is read intensive I/O in a random pattern to disks, negating much of the benefit of caching and read optimization strategies.  Plus, AD has a way larger cache in memory than most storage system caches.</li></ul>
|RAM|<ul><li>Database size</li><li>Base operating system recommendations</li><li>Third-party applications</li></ul>|<ul><li>Storage is the slowest component in a computer. The more that can be resident in RAM, the less it is necessary to go to disk.</li><li>Ensure enough RAM is allocated to store the operating system, Agents (antivirus, backup, monitoring), NTDS Database and growth over time.</li><li>For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.</li></ul>|
|Network|<ul><li>“Network Interface(\*)\Bytes Received/sec”</li><li>“Network Interface(\*)\Bytes Sent/sec”|<ul><li>In general, traffic sent from a DC far exceeds traffic sent to a DC.</li><li>As a switched Ethernet connection is full-duplex, inbound and outbound network traffic need to be sized independently.</li><li>Consolidating the number of DCs will increase the amount of bandwidth used to send responses back to client requests for each DC, but will be close enough to linear for the site as a whole.</li><li>If removing satellite location DCs, don’t forget to add the bandwidth for the satellite DC into the hub DCs as well as use that to evaluate how much WAN traffic there will be.</li></ul>|
|CPU|<ul><li>“Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read”</li><li>“Process(lsass)\\% Processor Time”</li></ul>|<ul><li>After eliminating storage as a bottleneck, address the amount of compute power needed.</li><li>While not perfectly linear, the number of processor cores consumed across all servers within a specific scope (such as a site) can be used to gauge how many processors are necessary to support the total client load. Add the minimum necessary to maintain the current level of service across all the systems within the scope.</li><li>Changes in processor speed, including power management related changes, impact numbers derived from the current environment. Generally, it is impossible to precisely evaluate how going from a 2.5 GHz processor to a 3 GHz processor will reduce the number of CPUs needed.</li></ul>|
|NetLogon|<ul><li>“Netlogon(\*)\Semaphore Acquires”</li><li>“Netlogon(\*)\Semaphore Timeouts”</li><li>“Netlogon(\*)\Average Semaphore Hold Time”</li></ul>|<ul><li>Net Logon Secure Channel/MaxConcurrentAPI only affects environments with NTLM authentications and/or PAC Validation. PAC Validation is on by default in operating system versions before Windows Server 2008. This is a client setting, so the DCs will be impacted until this is turned off on all client systems.</li><li>Environments with significant cross trust authentication, which includes intra-forest trusts, have greater risk if not sized properly.</li><li>Server consolidations will increase concurrency of cross-trust authentication.</li><li>Surges need to be accommodated, such as cluster fail-overs, as users re-authenticate en masse to the new cluster node.</li><li>Individual client systems (such as a cluster) might need tuning too.</li></ul>|

## Planning

For a long time, the community’s recommendation for sizing AD DS has been to “put in as much RAM as the database size.” For the most part, that recommendation is all that most environments needed to be concerned about. But the ecosystem consuming AD DS has gotten much bigger, as have the AD DS environments themselves, since its introduction in 1999. Although the increase in compute power and the switch from x86 architectures to x64 architectures has made the subtler aspects of sizing for performance irrelevant to a larger set of customers running AD DS on physical hardware, the growth of virtualization has reintroduced the tuning concerns to a larger audience than before.

The following guidance is thus about how to determine and plan for the demands of Active Directory as a service regardless of whether it is deployed in a physical, a virtual/physical mix, or a purely virtualized scenario. As such, we will break down the evaluation to each of the four main components: storage, memory, network, and processor. In short, in order to maximize performance on AD DS, the goal is to get as close to processor bound as possible.

## RAM

Simply, the more that can be cached in RAM, the less it is necessary to go to disk. To maximize the scalability of the server the minimum amount of RAM should be the sum of the current database size, the total SYSVOL size, the operating system recommended amount, and the vendor recommendations for the agents (antivirus, monitoring, backup, and so on). An additional amount should be added to accommodate growth over the lifetime of the server. This will be environmentally subjective based on estimates of database growth based on environmental changes.

For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage  section to ensure that storage is properly designed.

A corollary that comes up in the general context in sizing memory is sizing of the page file. In the same context as everything else memory related, the goal is to minimize going to the much slower disk. Thus the question should go from, “how should the page file be sized?” to “how much RAM is needed to minimize paging?” The answer to the latter question is outlined in the rest of this section. This leaves most of the discussion for sizing the page file to the realm of general operating system recommendations and the need to configure the system for memory dumps, which are unrelated to AD DS performance.

### Evaluating

The amount of RAM that a domain controller (DC) needs is actually a complex exercise for these reasons:

- High potential for error when trying to use an existing system to gauge how much RAM is needed as LSASS will trim under memory pressure conditions, artificially deflating the need.
- The subjective fact that an individual DC only needs to cache what is “interesting” to its clients. This means that the data that needs to be cached on a DC in a site with only an Exchange server will be very different than the data that needs to be cached on a DC that only authenticates users.
- The labor to evaluate RAM for each DC on a case-by-case basis is prohibitive and changes as the environment changes.
- The criteria behind the recommendation will help to make informed decisions: 
- The more that can be cached in RAM, the less it is necessary to go to disk. 
- Storage is by far the slowest component of a computer. Access to data on spindle-based and SSD storage media is on the order of 1,000,000x slower than access to data in RAM.

Thus, in order to maximize the scalability of the server, the minimum amount of RAM is the sum of the current database size, the total SYSVOL size, the operating system recommended amount, and the vendor recommendations for the agents (antivirus, monitoring, backup, and so on). Add additional amounts to accommodate growth over the lifetime of the server. This will be environmentally subjective based on estimates of database growth. However, for satellite locations with a small set of end users, these requirements can be relaxed as these sites will not need to cache as much to service most of the requests.

For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.

> [!NOTE]
> A corollary while sizing memory is sizing of the page file. Because the goal is to minimize going to the much slower disk, the question goes from “how should the page file be sized?” to “how much RAM is needed to minimize paging?” The answer to the latter question is outlined in the rest of this section. This leaves most of the discussion for sizing the page file to the realm of general operating system recommendations and the need to configure the system for memory dumps, which are unrelated to AD DS performance.

### Virtualization considerations for RAM

Avoid memory over-commit at the host. The fundamental goal behind optimizing the amount of RAM is to minimize the amount of time spent going to disk. In virtualization scenarios, the concept of memory over-commit exists where more RAM is allocated to the guests then exists on the physical machine. This in and of itself is not a problem. It becomes a problem when the total memory actively used by all the guests exceeds the amount of RAM on the host and the underlying host starts paging. Performance becomes disk-bound in cases where the domain controller is going to the NTDS.dit to get data, or the domain controller is going to the page file to get data, or the host is going to disk to get data that the guest thinks is in RAM.

### Calculation summary example

|Component|Estimated memory (example)|
|-|-|
|Base operating system recommended RAM (Windows Server 2008)|2 GB|
|LSASS internal tasks|200 MB|
|Monitoring agent|100 MB|
|Antivirus|100 MB|
|Database (Global Catalog)|8.5 GB are you sure ???|
|Cushion for backup to run, administrators to log on without impact|1 GB|
|Total|12 GB|

**Recommended: 16 GB**

Over time, the assumption can be made that more data will be added to the database and the server will probably be in production for 3 to 5 years. Based on an estimate of growth of 33%, 16 GB would be a reasonable amount of RAM to put in a physical server. In a virtual machine, given the ease with which settings can be modified and RAM can be added to the VM, starting at 12 GB with the plan to monitor and upgrade in the future is reasonable.

## Network

### Evaluating
This section is less about evaluating the demands regarding replication traffic, which is focused on traffic traversing the WAN and is thoroughly covered in [Active Directory Replication Traffic](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)), than it is about evaluating total bandwidth and network capacity needed, inclusive of client queries, Group Policy applications, and so on. For existing environments, this can be collected by using performance counters “Network Interface(\*)\Bytes Received/sec,” and “Network Interface(\*)\Bytes Sent/sec.” Sample intervals for Network Interface counters in either 15, 30, or 60 分钟utes. Anything less will generally be too volatile for good measurements; anything greater will smooth out daily peeks excessively.

> [!NOTE]
> Generally, the majority of network traffic on a DC is outbound as the DC responds to client queries. This is the reason for the focus on outbound traffic, though it is recommended to evaluate each environment for inbound traffic also. The same approaches can be used to address and review inbound network traffic requirements. For more information, see Knowledge Base article [929851: The default dynamic port range for TCP/IP has changed in Windows Vista and in Windows Server 2008](http://support.microsoft.com/kb/929851).

### Bandwidth needs

Planning for network scalability covers two distinct categories: the amount of traffic and the CPU load from the network traffic. Each of these scenarios is straight-forward compared to some of the other topics in this article.

In evaluating how much traffic must be supported, there are two unique categories of capacity planning for AD DS in terms of network traffic. The first is replication traffic that traverses between domain controllers and is covered thoroughly in the reference [Active Directory Replication Traffic](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)) and is still relevant to current versions of AD DS. The second is the intrasite client-to-server traffic. One of the simpler scenarios to plan for, intrasite traffic predominantly receives small requests from clients relative to the large amounts of data sent back to the clients. 100 MB will generally be adequate in environments up to 5,000 users per server, in a site. Using a 1 GB network adapter and Receive Side Scaling (RSS) support is recommended for anything above 5,000 users. To validate this scenario, particularly in the case of server consolidation scenarios, look at Network Interface(\*)\Bytes/sec across all the DCs in a site, add them together, and divide by the target number of domain controllers to ensure that there is adequate capacity. The easiest way to do this is to use the “Stacked Area” view in Windows Reliability and Performance Monitor (formerly known as Perfmon), making sure all of the counters are scaled the same.

Consider the following example (also known as, a really, really complex way to validate that the general rule is applicable to a specific environment). The following assumptions are made:

- The goal is to reduce the footprint to as few servers as possible. Ideally, one server will carry the load and an additional server is deployed for redundancy (*N* + 1 scenario). 
- In this scenario, the current network adapter supports only 100 MB and is in a switched environment.  
  The maximum target network bandwidth utilization is 60% in an N scenario (loss of a DC).
- Each server has about 10,000 clients connected to it.

Knowledge gained from the data in the chart (Network Interface(\*)\Bytes Sent/sec):

1. The business day starts ramping up around 5:30 and winds down at 7:00 PM.
1. The peak busiest period is from 8:00 AM to 8:15 AM, with greater than 25 Bytes sent/sec on the busiest DC.  
   > [!NOTE]
   > All performance data is historical. So the peak data point at 8:15 indicates the load from 8:00 to 8:15.
1. There are spikes before 4:00 AM, with more than 20 Bytes sent/sec on the busiest DC, which could indicate either load from different time zones or background infrastructure activity, such as backups. Since the peak at 8:00 AM exceeds this activity, it is not relevant.
1. There are five Domain Controllers in the site.
1. The max load is about 5.5 MB/s per DC, which represents 44% of the 100 MB connection. Using this data, it can be estimated that the total bandwidth needed between 8:00 AM and 8:15 AM is 28 MB/s.
   >[!NOTE]
   >Be careful with the fact that Network Interface sent/receive counters are in bytes and network bandwidth is measured in bits. 100 MB &divide; 8 = 12.5 MB, 1 GB &divide; 8 = 128 MB.
  
Conclusions:

1. This current environment does meet the N+1 level of fault tolerance at 60% target utilization. Taking one system offline will shift the bandwidth per server from about 5.5 MB/s (44%) to about 7 MB/s (56%).
1. Based on the previously stated goal of consolidating to one server, this both exceeds the maximum target utilization and theoretically the possible utilization of a 100 MB connection.
1. With a 1 GB connection this will represent 22% of the total capacity.
1. Under normal operating conditions in the *N* + 1 scenario, client load will be relatively evenly distributed at about 14 MB/s per server or 11% of total capacity.
1. To ensure that capacity is adequate during unavailability of a DC, the normal operating targets per server would be about 30% network utilization or 38 MB/s per server. Failover targets would be 60% network utilization or 72 MB/s per server.  
  
In short, the final deployment of systems must have a 1 GB network adapter and be connected to a network infrastructure that will support said load. A further note is that given the amount of network traffic generated, the CPU load from network communications can have a significant impact and limit the maximum scalability of AD DS. This same process can be used to estimate the amount of inbound communication to the DC. But given the predominance of outbound traffic relative to inbound traffic, it is an academic exercise for most environments. Ensuring hardware support for RSS is important in environments with greater than 5,000 users per server. For scenarios with high network traffic, balancing of interrupt load can be a bottleneck. This can be detected by Processor(\*)\% Interrupt Time being unevenly distributed across CPUs. RSS enabled NICs can mitigate this limitation and increase scalability.

> [!NOTE]
> A similar approach can be used to estimate the additional capacity necessary when consolidating data centers, or retiring a domain controller in a satellite location. Simply collect the outbound and inbound traffic to clients and that will be the amount of traffic that will now be present on the WAN links.  
>  
> In some cases, you might experience more traffic than expected because traffic is slower, such as when certificate checking fails to meet aggressive time-outs on the WAN. For this reason, WAN sizing and utilization should be an iterative, ongoing process.

### Virtualization considerations for network bandwidth

It is easy to make recommendations for a physical server: 1 GB for servers supporting greater than 5000 users. Once multiple guests start sharing an underlying virtual switch infrastructure, additional attention is necessary to ensure that the host has adequate network bandwidth to support all the guests on the system, and thus requires the additional rigor. This is nothing more than an extension of ensuring the network infrastructure into the host machine. This is regardless whether the network is inclusive of the domain controller running as a virtual machine guest on a host with the network traffic going over a virtual switch, or whether connected directly to a physical switch. The virtual switch is just one more component where the uplink needs to support the amount of data being transmitted. Thus the physical host physical network adapter linked to the switch should be able to support the DC load plus all other guests sharing the virtual switch connected to the physical network adapter.

### Calculation summary example

|System|Peak bandwidth|
|-|-|
DC 1|6.5 MB/s|
DC 2|6.25 MB/s|
|DC 3|6.25 MB/s|
|DC 4|5.75 MB/s|
|DC 5|4.75 MB/s|
|Total|28.5 MB/s|

**Recommended: 72 MB/s** (28.5 MB/s divided by 40%)

|Target system(s) count|Total bandwidth (from above)|
|-|-|
|2|28.5 MB/s|
|Resulting normal behavior|28.5 &divide; 2 = 14.25 MB/s|

As always, over time the assumption can be made that client load will increase and this growth should be planned for as best as possible. The recommended amount to plan for would allow for an estimated growth in network traffic of 50%.

## Storage

Planning storage constitutes two components:

- Capacity, or storage size
- Performance

A great amount of time and documentation is spent on planning capacity, leaving performance often completely overlooked. With current hardware costs, most environments are not large enough that either of these is actually a concern, and the recommendation to “put in as much RAM as the database size” usually covers the rest, though it may be overkill for satellite locations in larger environments.

### Sizing

#### Evaluating for storage

Compared to 13 years ago when Active Directory was introduced, a time when 4 GB and 9 GB drives were the most common drive sizes, sizing for Active Directory is not even a consideration for all but the largest environments. With the smallest available hard drive sizes in the 180 GB range, the entire operating system, SYSVOL, and NTDS.dit can easily fit on one drive. As such, it is recommended to deprecate heavy investment in this area.

The only recommendation for consideration is to ensure that 110% of the NTDS.dit size is available in order to enable defrag. Additionally, accommodations for growth over the life of the hardware should be made.

The first and most important consideration is evaluating how large the NTDS.dit and SYSVOL will be. These measurements will lead into sizing both fixed disk and RAM allocation. Due to the (relatively) low cost of these components, the math does not need to be rigorous and precise. Content about how to evaluate this for both existing and new environments can be found in the [Data Storage](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10)) series of articles. Specifically, refer to the following articles:

- **For existing environments &ndash;** The section titled “To activate logging of disk space that is freed by defragmentation” in the article [Storage Limits](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10)).
- **For new environments &ndash;** The article titled [Growth Estimates for Active Directory Users and Organizational Units](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10)).

  > [!NOTE]
  > The articles are based on data size estimates made at the time of the release of Active Directory in Windows 2000. Use object sizes that reflect the actual size of objects in your environment.

When reviewing existing environments with multiple domains, there may be variations in database sizes. Where this is true, use the smallest global catalog (GC) and non-GC sizes.  

The database size can vary between operating system versions. DCs that run earlier operating systems such as Windows Server 2003 has a smaller database size than a DC that runs a later operating system such as Windows Server 2008 R2, especially when features such Active Directory Recycle Bin or Credential Roaming are enabled.

> [!NOTE]  
  >
>- For new environments, notice that the estimates in Growth Estimates for Active Directory Users and Organizational Units indicate that 100,000 users (in the same domain) consume about 450 MB of space. Please note that the attributes populated can have a huge impact on the total amount. Attributes will be populated on many objects by both third-party and Microsoft products, including Microsoft Exchange Server and Lync. An evaluation based on the portfolio of the products in the environment is preferred, but the exercise of detailing out the math and testing for precise estimates for all but the largest environments may not actually be worth significant time and effort.
>- Ensure that 110% of the NTDS.dit size is available as free space in order to enable offline defrag, and plan for growth over a three to five year hardware lifespan. Given how cheap storage is, estimating storage at 300% the size of the DIT as storage allocation is safe to accommodate growth and the potential need for offline defrag.

#### Virtualization considerations for storage

In a scenario where multiple Virtual Hard Disk (VHD) files are being allocated on a single volume use a fixed sized disk of at least 210% (100% of the DIT + 110% free space) the size of the DIT to ensure that there is adequate space reserved.  

#### Calculation summary example

|Data collected from evaluation phase| |
|-|-|
|NTDS.dit size|35 GB|
|Modifier to allow for offline defrag|2.1|
|Total storage needed|73.5 GB|

> [!NOTE]
> This storage needed is in addition to the storage needed for SYSVOL, operating system, page file, temporary files, local cached data (such as installer files), and applications.

### Storage performance

#### Evaluating performance of storage

As the slowest component within any computer, storage can have the biggest adverse impact on client experience. For those environments large enough for which the RAM sizing recommendations are not feasible, the consequences of overlooking planning storage for performance can be devastating.  Also, the complexities and varieties of storage technology further increase the risk of failure as the relevance of long standing best practices of “put operating system, logs, and database” on separate physical disks is limited in it’s useful scenarios.  This is because the long standing best practice is based on the assumption that is that a “disk” is a dedicated spindle and this allowed I/O to be isolated.  This assumptions that make this true are no longer relevant with the introduction of:

- New storage types and virtualized and shared storage scenarios
- Shared spindles on a Storage Area Network (SAN)
- VHD file on a SAN or network-attached storage
- Solid State Drives
- Tiered storage architectures (i.e. SSD storage tier caching larger spindle based storage)

In short, the end goal of all storage performance efforts, regardless of underlying storage architecture and design, is to ensure the needed amount of Input/Output Operations per Second (IOPS) are available and that those IOPS happen within an acceptable time frame. This section covers how to evaluate what AD DS demands of the underlying storage in order to ensure storage solutions are properly designed.  Given the variability of today’s storage technologies, it is best to work with the storage vendors to ensure adequate IOPS.  For those scenarios with locally attached storage, reference the Appendix C for the basics in how to design traditional local storage scenarios.  This principals are generally applicable to more complex storage tiers and will also help in dialog with the vendors supporting backend storage solutions.

- Given the wide breadth of storage options available, it is recommended to engage the expertise of hardware support teams or vendors to ensure that the specific solution meets the needs of AD DS. The following numbers are the information that would be provided to the storage specialists.

For environments where the database is too large to be held in RAM, use the performance counters to determine how much I/O needs to be supported:

- LogicalDisk(\*)\Avg Disk sec/Read (for example, if NTDS.dit is stored on the D:/ drive, the full path would be LogicalDisk(D:)\Avg Disk sec/Read)
- LogicalDisk(\*)\Avg Disk sec/Write
- LogicalDisk(\*)\Avg Disk sec/Transfer
- LogicalDisk(\*)\Reads/sec
- LogicalDisk(\*)\Writes/sec
- LogicalDisk(\*)\Transfers/sec

These should be sampled in 15/30/60 minute intervals to benchmark the demands of the current environment.

#### Evaluating the results

> [!NOTE]
> The focus is on reads from the database as this is usually the most demanding component, the same logic can be applied to writes to the log file by substituting LogicalDisk(*\<NTDS Log\>*)\Avg Disk sec/Write and LogicalDisk(*\<NTDS Log\>*)\Writes/sec):
>  
> - LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read indicates whether or not the current storage is adequately sized.  If the results are roughly equal to the Disk Access Time for the disk type, LogicalDisk(*\<NTDS\>*)\Reads/sec is a valid measure.  Check the manufacturer specifications for the storage on the back end, but good ranges for LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read would roughly be:
>   - 7200 – 9 to 12.5 milliseconds (ms)
>   - 10,000 – 6 to 10 ms
>   - 15,000 – 4 to 6 ms  
>   - SSD – 1 to 3 ms  
>   - >[!NOTE]
>     >Recommendations exist stating that storage performance is degraded at 15ms to 20ms (depending on source).  The difference between the above values and the other guidance is that the above values are the normal operating range.  The other recommendations are troubleshooting guidance to identify when client experience significantly degrades and becomes noticeable.  Reference Appendix C for a deeper explanation.
> - LogicalDisk(*\<NTDS\>*)\Reads/sec is the amount of I/O that is being performed.
>   - If LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read is within the optimal range for the backend storage, LogicalDisk(*\<NTDS\>*)\Reads/sec can be used directly to size the storage.
>   - If LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read is not within the optimal range for the backend storage, additional I/O is needed according to the following formula:
>     > (LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read) &divide; (Physical Media Disk Access Time) &times; (LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read)

Considerations:

- Note that if the server is configured with a sub-optimal amount of RAM, these values will be inaccurate for planning purposes.  They will be erroneously on the high side and can still be used as a worst case scenario.
- Adding/Optimizing RAM specifically will drive a decrease in the amount of read I/O (LogicalDisk(*\<NTDS\>*)\Reads/Sec.  This means the storage solution may not have to be as robust as initially calculated.  Unfortunately, anything more specific than this general statement is environmentally dependent on client load and general guidance cannot be provided.  The best option is to adjust storage sizing after optimizing RAM.

#### Virtualization considerations for performance

Similar to all of the preceding virtualization discussions, the key here is to ensure that the underlying shared infrastructure can support the DC load plus the other resources using the underlying shared media and all pathways to it. This is true whether a physical domain controller is sharing the same underlying media on a SAN, NAS, or iSCSI infrastructure as other servers or applications, whether it is a guest using pass through access to a SAN, NAS, or iSCSI infrastructure that shares the underlying media, or if the guest is using a VHD file that resides on shared media locally or a SAN, NAS, or iSCSI infrastructure. The planning exercise is all about making sure that the underlying media can support the total load of all consumers.

Also, from a guest perspective, as there are additional code paths that must be traversed, there is a performance impact to having to go through a host to access any storage. Not surprisingly, storage performance testing indicates that the virtualizing has an impact on throughput that is subjective to the processor utilization of the host system (see Appendix A: CPU Sizing Criteria), which is obviously influenced by the resources of the host demanded by the guest. This contributes to the virtualization considerations regarding processing needs in a virtualized scenario (see [Virtualization considerations for processing](#virtualization-considerations-for-processing)).

Making this more complex is that there are a variety of different storage options that are available that all have different performance impacts. As a safe estimate when migrating from physical to virtual, use a multiplier of 1.10 to adjust for different storage options for virtualized guests on Hyper-V, such as pass-through storage, SCSI Adapter, or IDE. The adjustments that need to be made when transferring between the different storage scenarios are irrelevant as to whether the storage is local, SAN, NAS, or iSCSI.

#### Calculation summary example

Determining the amount of I/O needed for a healthy system under normal operating conditions:

- LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec during the peak period 15 minute period 
- To determine the amount of I/O needed for storage where the capacity of the underlying storage is exceeded:
  >*Needed IOPS* = (LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read &divide; *\<Target Avg Disk sec/Read\>*) &times; LogicalDisk(*\<NTDS Database Drive\>*)\Read/sec

|Counter|Value|
|-|-|
|Actual LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer|.02 seconds (20 milliseconds)|
|Target LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer|.01 seconds|
|Multiplier for change in available IO|0.02 &divide; 0.01 = 2|  
  
|Value name|Value|
|-|-|
|LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec|400|
|Multiplier for change in available IO|2|
|Total IOPS needed during peak period|800|

To determine the rate at which the cache is desired to be warmed:

- Determine the maximum acceptable time to warm the cache. It is either the amount of time that it should take to load the entire database from disk, or for scenarios where the entire database cannot be loaded in RAM, this would be the maximum time to fill RAM.
- Determine the size of the database, excluding white space.  For more information, see [Evaluating for storage](#evaluating-for-storage).  
- Divide the database size by 8 KB; that will be the total IOs necessary to load the database.
- Divide the total IOs by the number of seconds in the defined time frame.

Note that the rate calculated, while accurate, will not be exact because previously loaded pages are evicted if ESE is not configured to have a fixed cache size, and AD DS by default uses variable cache size.

|Data points to collect|Values
|-|-|
|Maximum acceptable time to warm|10 minutes (600 seconds)
|Database size|2 GB|  
  
|Calculation step|Formula|Result|
|-|-|-|
|Calculate size of database in pages|(2 GB &times; 1024 &times; 1024) = *Size of database in KB*|2,097,152 KB|
|Calculate number of pages in database|2,097,152 KB &divide; 8 KB = *Number of pages*|262,144 pages|
|Calculate IOPS necessary to fully warm the cache|262,144 pages &divide; 600 seconds = *IOPS needed*|437 IOPS|

## Processing

### Evaluating Active Directory processor usage

For most environments, after storage, RAM, and networking are properly tuned as described in the Planning section, managing the amount of processing capacity will be the component that deserves the most attention. There are two challenges in evaluating CPU capacity needed:

- Whether or not the applications in the environment are being well-behaved in a shared services infrastructure, and is discussed in the section titled “Tracking Expensive and Inefficient Searches” in the article Creating More Efficient Microsoft Active Directory-Enabled Applications or migrating away from down-level SAM calls to LDAP calls.  
  
  In larger environments, the reason this is important is that poorly coded applications can drive volatility in CPU load, “steal” an inordinate amount of CPU time from other applications, artificially drive up capacity needs, and unevenly distribute load against the DCs.  
- As AD DS is a distributed environment with a large variety of potential clients, estimating the expense of a “single client” is environmentally subjective due to usage patterns and the type or quantity of applications leveraging AD DS. In short, much like the networking section, for broad applicability, this is better approached from the perspective of evaluating the total capacity needed in the environment.

For existing environments, as storage sizing was discussed previously, the assumption is made that storage is now properly sized and thus the data regarding processor load is valid. To reiterate, it is critical to ensure that the bottleneck in the system is not the performance of the storage. When a bottleneck exists and the processor is waiting, there are idle states that will go away once the bottleneck is removed.  As processor wait states are removed, by definition, CPU utilization increases as it no longer has to wait on the data. Thus, collect performance counters “Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read” and “Process(lsass)\\% Processor Time”. The data in “Process(lsass)\\% Processor Time” will be artificially low if “Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read” exceeds 10 to 15 ms, which is a general threshold that Microsoft support uses for troubleshooting storage-related performance issues. As before, it is recommended that sample intervals be either 15, 30, or 60 minutes. Anything less will generally be too volatile for good measurements; anything greater will smooth out daily peeks excessively.

### Introduction

In order to plan capacity planning for domain controllers, processing power requires the most attention and understanding. When sizing systems to ensure maximum performance, there is always a component that is the bottleneck and in a properly sized Domain Controller this will be the processor.

Similar to the networking section where the demand of the environment is reviewed on a site-by-site basis, the same must be done for the compute capacity demanded. Unlike the networking section, where the available networking technologies far exceed the normal demand, pay more attention to sizing CPU capacity.  As any environment of even moderate size; anything over a few thousand concurrent users can put significant load on the CPU.

Unfortunately, due to the huge variability of client applications that leverage AD, a general estimate of users per CPU is woefully inapplicable to all environments. Specifically, the compute demands are subject to user behavior and application profile. Therefore, each environment needs to be individually sized.

#### Target site behavior profile

As mentioned previously, when planning capacity for an entire site, the goal is to target a design with an *N* + 1 capacity design, such that failure of one system during the peak period will allow for continuation of service at a reasonable level of quality. That means that in an “*N*” scenario, load across all the boxes should be less than 100% (better yet, less than 80%) during the peak periods.

Additionally, if the applications and clients in the site are using best practices for locating domain controllers (that is, using the [DsGetDcName function](http://msdn.microsoft.com/en-us/library/windows/desktop/ms675983(v=vs.85).aspx)), the clients should be relatively evenly distributed with minor transient spikes due to any number of factors.

In the next example, the following assumptions are made:

- Each of the five DCs in the site has four of CPUs.
- Total target CPU usage during business hours is 40% under normal operating conditions (“*N* + 1”) and 60% otherwise (“*N*”). During non-business hours, the target CPU usage is 80% because backup software and other maintenance are expected to consume all available resources.

![CPU usage chart](media/capacity-planning-considerations-cpu-chart.png)

Analyzing the data in the chart (Processor Information(_Total)\% Processor Utility) for each of the DCs:

- For the most part, the load is relatively evenly distributed which is what would be expected when clients use DC locator and have well written searches. 
- There are a number of five-minute spikes of 10%, with some as large as 20%. Generally, unless they cause the capacity plan target to be exceeded, investigating these is not worthwhile.  
- The peak period for all systems is between about 8:00 AM and 9:15 AM. With the smooth transition from about 5:00 AM through about 5:00 PM, this is generally indicative of the business cycle. The more randomized spikes of CPU usage on a box-by-box scenario between 5:00 PM and 4:00 AM would be outside of the capacity planning concerns.

  >[!NOTE]
  >On a well-managed system, said spikes are might be backup software running, full system antivirus scans, hardware or software inventory, software or patch deployment, and so on. Because they fall outside the peak user business cycle, the targets are not exceeded.

- As each system is about 40% and all systems have the same numbers of CPUs, should one fail or be taken offline, the remaining systems would run at an estimated 53% (System D's 40% load is evenly split and added to System A's and System C’s existing 40% load). For a number of reasons, this linear assumption is NOT perfectly accurate, but provides enough accuracy to gauge.  

  **Alternate scenario –** Two domain controllers running at 40%: One domain controller fails, estimated CPU on the remaining one would be an estimated 80%. This far exceeds the thresholds outlined above for capacity plan and also starts to severely limit the amount of head room for the 10% to 20% seen in the load profile above, which means that the spikes would drive the DC to 90% to 100% during the “*N*” scenario and definitely degrade responsiveness.

### Calculating CPU demands

The “Process\\% Processor Time” performance object counter sums the total amount of time that all of the threads of an application spend on the CPU and divides by the total amount of system time that has passed. The effect of this is that a multi-threaded application on a multi-CPU system can exceed 100% CPU time, and would be interpreted VERY differently than “Processor Information\\% Processor Utility”. In practice the “Process(lsass)\\% Processor Time” can be viewed as the count of CPUs running at 100% that are necessary to support the process’s demands. A value of 200% means that 2 CPUs, each at 100%, are needed to support the full AD DS load. Although a CPU running at 100% capacity is the most cost efficient from the perspective of money spent on CPUs and power and energy consumption, for a number of reasons detailed in Appendix A, better responsiveness on a multi-threaded system occurs when the system is not running at 100%.

To accommodate transient spikes in client load, it is recommended to target a peak period CPU of between 40% and 60% of system capacity. Working with the example above, that would mean that between 3.33 (60% target) and 5 (40% target) CPUs would be needed for the AD DS (lsass process) load. Additional capacity should be added in according to the demands of the base operating system and other agents required (such as antivirus, backup, monitoring, and so on). Although the impact of agents needs to be evaluated on a per environment basis, an estimate of between 5% and 10% of a single CPU can be made. In the current example, this would suggest that between 3.43 (60% target) and 5.1 (40% target) CPUs are necessary during peak periods.

The easiest way to do this is to use the “Stacked Area” view in Windows Reliability and Performance Monitor (perfmon), making sure all of the counters are scaled the same.

Assumptions:

- Goal is to reduce footprint to as few servers as possible. Ideally, one server would carry the load and an additional server added for redundancy (*N* + 1 scenario).

![Processor time chart for lsass process (over all processors)](media/capacity-planning-considerations-proc-time-chart.png)

Knowledge gained from the data in the chart (Process(lsass)\\% Processor Time):

- The business day starts ramping up around 7:00 and decreases at 5:00 PM.
- The peak busiest period is from 9:30 AM to 11:00 AM. 
  > [!NOTE]
  > All performance data is historical. The peak data point at 9:15 indicates the load from 9:00 to 9:15.
- There are spikes before 7:00 AM which could indicate either load from different time zones or background infrastructure activity, such as backups. Because the peak at 9:30 AM exceeds this activity, it is not relevant.
- There are three domain controllers in the site.

At maximum load, lsass consumes about 485% of one CPU, or 4.85 CPUs running at 100%. As per the math earlier, this means the site needs about 12.25 CPUs for AD DS. Add in the above suggestions of 5% to 10% for background processes and that means replacing the server today would need approximately 12.30 to 12.35 CPUs to support the same load. An environmental estimate for growth now needs to be factored in.

### When to tune LDAP weights

There are several scenarios where tuning [LdapSrvWeight](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10)) should be considered. Within the context of capacity planning, this would be done when the application or user loads are not evenly balanced, or the underlying systems are not evenly balanced in terms of capability. Reasons to do so beyond capacity planning are outside of the scope of this article.

There are two common reasons to tune LDAP Weights:

- The PDC emulator is an example that affects every environment for which user or application load behavior is not evenly distributed. As certain tools and actions target the PDC emulator, such as the Group Policy management tools, second attempts in the case of authentication failures, trust establishment, and so on, CPU resources on the PDC emulator may be more heavily demanded than elsewhere in the site.
  - It is only useful to tune this if there is a noticeable difference in CPU utilization in order  to reduce the load on the PDC emulator and increase the load on other domain controllers will allow a more even distribution of load.
  - In this case, set LDAPSrvWeight between 50 and 75 for the PDC emulator.
- Servers with differing counts of CPUs (and speeds) in a site.  For example, say there are two eight-core servers and one four-core server.  The last server has half the processors of the other two servers.  This means that a well distributed client load will increase the average CPU load on the four-core box to roughly twice that of the eight-core boxes.
  - For example, the two eight-core boxes would be running at 40% and the four-core box would be running at 80%.
  - Also, consider the impact of loss of one eight-core box in this scenario, specifically the fact that the four-core box would now be overloaded.

#### Example 1 - PDC

| |Utilization with defaults|New LdapSrvWeight|Estimated new utilization|
|-|-|-|-|
|DC 1 (PDC emulator)|53%|57|40%|
|DC 2|33%|100|40%|
|DC 3|33%|100|40%|

The catch here is that if the PDC emulator role is transferred or seized, particularly to another domain controller in the site, there will be a dramatic increase on the new PDC emulator.

Using the example from the section [Target site behavior profile](#target-site-behavior-profile), an assumption was made that all three domain controllers in the site had four CPUs. What should happen, under normal conditions, if one of the domain controllers had eight CPUs? There would be two domain controllers at 40% utilization and one at 20% utilization. While this is not bad, there is an opportunity to balance the load a little bit better. Leverage LDAP weights to accomplish this.  An example scenario would be:

#### Example 2 - Differing CPU counts

| |Processor Information\\ %&nbsp;Processor Utility(_Total)<br />Utilization with defaults|New LdapSrvWeight|Estimated new utilization|
|-|-|-|-|
|4-CPU DC 1|40|100|30%|
|4-CPU DC 2|40|100|30%|
|8-CPU DC 3|20|200|30%|

Be very careful with these scenarios though. As can be seen above, the math looks really nice and pretty on paper. But throughout this article, planning for an “*N* + 1” scenario is of paramount importance. The impact of one DC going offline must be calculated for every scenario. In the immediately preceding scenario where the load distribution is even, in order to ensure a 60% load during an “*N*” scenario, with the load balanced evenly across all servers, the distribution will be fine as the ratios stay consistent. Looking at the PDC emulator tuning scenario, and in general any scenario where user or application load is unbalanced, the effect is very different:

| |Tuned Utilization|New LdapSrvWeight|Estimated New Utilization|
|-|-|-|-|
|DC 1 (PDC emulator)|40%|85|47%|
|DC 2|40%|100|53%|
|DC 3|40%|100|53%|

### Virtualization considerations for processing

There are two layers of capacity planning that need to be done in a virtualized environment. At the host level, similar to the identification of the business cycle outlined for the domain controller processing previously, thresholds during the peak period need to be identified. Because the underlying principals are the same for a host machine scheduling guest threads on the CPU as for getting AD DS threads on the CPU on a physical machine, the same goal of 40% to 60% on the underlying host are recommended. At the next layer, the guest layer, since the principals of thread scheduling have not changed, the goal within the guest remains in the 40% to 60% range.

In a direct mapped scenario, one guest per host, all the capacity planning done to this point needs to be added to the requirements (RAM, disk, network) of the underlying host operating system. In a shared host scenario, testing indicates that there is 10% impact on the efficiency of the underlying processors. That means if a site needs 10 CPUs at a target of 40%, the recommended amount of virtual CPUs to allocate across all the “*N*” guests would be 11. In a site with a mixed distribution of physical servers and virtual servers, the modifier only applies to the VMs. For example, if a site has an “*N* + 1” scenario, one physical or direct-mapped server with 10 CPUs would be about equivalent to one guest with 11 CPUs on a host, with 11 CPUs reserved for the domain controller.

Throughout the analysis and calculation of the CPU quantities necessary to support AD DS load, the numbers of CPUs that map to what can be purchased in terms physical hardware do not necessarily map cleanly. Virtualization eliminates the need to round up. Virtualization decreases the effort necessary to add compute capacity to a site, given the ease with which a CPU can be added to a VM. It does not eliminate the need to accurately evaluate the compute power needed so that the underlying hardware is available when additional CPUs need to be added to the guests.  As always, remember to plan and monitor for growth in demand.

### Calculation summary example

|System|Peak CPU|
|-|-|-|
|DC 1|120%|
|DC 2|147%|
|Dc 3|218%|
|Total CPU being used|485%|  
  
|Target system(s) count|Total bandwidth (from above)|
|-|-|
|CPUs needed at 40% target|4.85 &divide; .4 = 12.25|

Repeating due to the importance of this point, *remember to plan for growth*. Assuming 50% growth over the next three years, this environment will need 18.375 CPUs (12.25 &times; 1.5) at the three-year mark. An alternate plan would be to review after the first year and add in additional capacity as needed.

### Cross-trust client authentication load for NTLM

#### Evaluating cross-trust client authentication load

Many environments may have one or more domains connected by a trust. An authentication request for an identity in another domain that does not use Kerberos authentication needs to traverse a trust using the domain controller’s secure channel to another domain controller either in the destination domain or the next domain in the path to the destination domain. The number of concurrent calls using the secure channel that a domain controller can make to a domain controller in a trusted domain is controlled by a setting known as **MaxConcurrentAPI**. For domain controllers, ensuring that the secure channel can handle the amount of load is accomplished by one of two approaches: tuning **MaxConcurrentAPI** or, within a forest, creating shortcut trusts. To gauge the volume of traffic across an individual trust, refer to [How to do performance tuning for NTLM authentication by using the MaxConcurrentApi setting](https://support.microsoft.com/kb/2688798).

During data collection, this, as with all the other scenarios, must be collected during the peak busy periods of the day for the data to be useful.

> [!NOTE]
> Intraforest and interforest scenarios may cause the authentication to traverse multiple trusts and each stage would need to be tuned.

#### Planning

There are a number of applications that use NTLM authentication by default, or use it in a certain configuration scenario. Application servers grow in capacity and service an increasing number of active clients. There is also a trend that clients keep sessions open for a limited time and rather reconnect on a regular basis (such as email pull sync). Another common example for high NTLM load is web proxy servers that require authentication for Internet access.

These applications can cause a significant load for NTLM authentication, which can put significant stress on the DCs, especially when users and resources are in different domains.

There are multiple approaches to managing cross-trust load, which in practice are used in conjunction rather than in an exclusive either/or scenario. The possible options are:

- Reduce cross-trust client authentication by locating the services that a user consumes in the same domain that the user is resident in.
- Increase the number of secure-channels available. This is relevant to intraforest and cross-forest traffic and are known as shortcut trusts.
- Tune the default settings for **MaxConcurrentAPI**.

For tuning **MaxConcurrentAPI** on an existing server, the equation is:

> *New_MaxConcurrentApi_setting* &ge; (*semaphore_acquires* + *semaphore_time-outs*) &times; *average_semaphore_hold_time* &divide; *time_collection_length*

For more information, see [KB article 2688798: How to do performance tuning for NTLM authentication by using the MaxConcurrentApi setting](http://support.microsoft.com/kb/2688798).

## Virtualization considerations

None, this is an operating system tuning setting.

### Calculation summary example

|Data type|Value|
|-|-|
|Semaphore Acquires (Minimum)|6,161|
|Semaphore Acquires (Maximum)|6,762|
|Semaphore Timeouts|0|
|Average Semaphore Hold Time|0.012|
|Collection Duration (seconds)|1:11 minutes (71 seconds)|
|Formula (from KB 2688798)|((6762 &ndash; 6161) + 0) &times; 0.012 /|
|Minimum value for **MaxConcurrentAPI**|((6762 &ndash; 6161) + 0) &times; 0.012 &divide; 71 = .101|

For this system for this time period, the default values are acceptable.

## Monitoring for compliance with capacity planning goals

Throughout this article, it has been discussed that planning and scaling go towards utilization targets. Here is a summary chart of the recommended thresholds that must be monitored to ensure the systems are operating within adequate capacity thresholds. Keep in mind that these are not performance thresholds, but capacity planning thresholds. A server operating in excess of these thresholds will work, but is time to start validating that all the applications are well behaved. If said applications are well behaved, it is time to start evaluating hardware upgrades or other configuration changes.

|Category|Performance counter|Interval/Sampling|Target|Warning|
|-|-|-|-|-|
|Processor|Processor Information(_Total)\\% Processor Utility|60 min|40%|60%|
|RAM (Windows Server 2008 R2 或更早版本)|Memory\Available MB|< 100 MB|不可用|< 100 MB|
|RAM (Windows Server 2012)|Memory\Long 术语的平均待机缓存 Lifetime(s)|30 分钟|必须进行测试|必须进行测试|
|网络|Network Interface(\*)\Bytes Sent/sec<br /><br />Network Interface(\*)\Bytes Received/sec|30 分钟|40%|60%|
|存储|LogicalDisk( *\<NTDS Database Drive\>* )\Avg Disk sec/Read<br /><br />LogicalDisk( *\<NTDS Database Drive\>* )\Avg Disk sec/Write|60 分钟|10 毫秒|15 毫秒|
|AD 服务|Netlogon(\*)\Average Semaphore Hold Time|60 分钟|0|1 秒|

## <a name="appendix-a-cpu-sizing-criteria"></a>附录 A：CPU 大小调整条件

### <a name="definitions"></a>定义

**处理器 （微处理器） –** 读取并执行程序指令的组件  

**CPU-** 中央处理单元  

**多核处理器 –** 相同集成线路上的多个 Cpu  

**多 CPU –** 不在同一集成线路上的多个 Cpu  

**逻辑处理器 –** 从操作系统的角度来看一种逻辑计算引擎  

这包括超线程在多核处理器或单核处理器上的一个核心。  

当今的服务器系统有多个处理器、 多个多核处理器和超线程，因为已通用化此信息才能涵盖这两种方案。 在这种情况下，因为它表示的操作系统和应用程序的可用计算引擎的角度来看，将使用术语逻辑处理器。

### <a name="thread-level-parallelism"></a>线程级并行性

每个线程都是独立的任务，因为每个线程都有其自己的堆栈和说明。 因为 AD DS 是多线程，并且可以通过使用优化的可用线程数[如何查看和设置 LDAP 策略在 Active Directory 中通过使用 Ntdsutil.exe](http://support.microsoft.com/kb/315071)，它还跨多个逻辑处理器扩展。

### <a name="data-level-parallelism"></a>数据级别的并行度

这涉及到共享数据跨多个线程 （对于单独的 AD DS 过程） 的一个进程内和跨多个进程中的多个线程 （一般情况下）。 使用过度简化这种情况的问题，这意味着对数据的任何更改都会反映到所有正在运行的线程在所有各种级别的所有内核缓存 (L1，L2、 L3) 正在运行的所述的线程，以及更新共享的内存。 性能会降低在写入操作期间，而所有不同的内存位置指令处理仍可以继续之前会保持一致。

### <a name="cpu-speed-vs-multiple-core-considerations"></a>CPU 速度和多核注意事项

一般的经验法则是速度更快的逻辑处理器减少处理一系列的说明，可以在同一时间运行更多的任务的多个逻辑处理器方法时所需的持续时间。 这些规则的经验法则细分为随着本质上是从共享内存，等待数据级并行性和管理多个线程的开销提取数据的需要注意的事项更复杂方案。 这也是为什么在多核系统中的可伸缩性不是线性的。

请考虑以下类比中需要考虑这些注意事项： 的高速公路，与每个线程正在单独的汽车，每个内核，且速度限制正在的时钟速度的球道思考。

1. 如果高速公路上只有一辆汽车，无论是否有两个通道或 12 通道。 该汽车只会尽可能快地速度限制将允许转。
1. 假定线程需要的数据不立即可用。 比喻这种情况是道路的段已关闭。 如果高速公路上只有一辆汽车，它并不重要的速度限制之前重新打开球道 （从内存提取的数据）。
1. 汽车数增加时，会增加的开销的汽车数。 比较的驾驶体验，并关注必要的量时之路几乎是空 （例如为深夜） 与当通信流量较大 （例如中间下午，但不是刺激小时）。 此外，注意量必要时考虑推动两道高速公路上的其中只有一个其他球道担心驱动程序正在执行哪些操作，用户都不必担心什么很多其他驱动程序的六个球道高速公路与正在执行。
   > [!NOTE]
   > 有关出行方案的相似之处被扩展下一节中所示：响应时间 / 系统的日常工作如何影响性能。

因此，有关更多或更快的处理器的具体信息会变得高度主观到应用程序行为，这就 AD DS 而言是非常环保特定，甚至在环境中不同服务器到服务器。 这是为什么前面文章中的引用不方面投入了大量中是过于简洁，而且计算中包括安全的边距。 预算驱动购买决定时，建议，优化使用情况的 40%（或环境所需的数量） 处理器之前考虑购买更快的处理器中第一次，发生。 跨多个处理器的增加的同步减少了从线性进度的多个处理器的真正优势 (2&times;处理器数提供了小于 2&times;提供额外的计算能力)。

> [!NOTE]
> Amdahl 定律和 Gustafson 的法律的相关的概念。

### <a name="response-timehow-the-system-busyness-impacts-performance"></a>响应时间 / 系统的日常工作如何影响性能

队列从理论上讲是数学等待行 （队列） 的研究。 在队列从理论上讲，利用率法律公式表示：

*U* k = *B* &divide; *T*

其中*U* k 是利用率百分比*B*处于繁忙状态，时间量并*T*是观察到系统的总时间。 转换到 Windows 的上下文，也就是说，100 毫微秒 (ns) 数除以多少 100 ns 间隔了中给定的时间间隔到运行状态的间隔线程。 这是完全的公式用于计算 %处理器实用程序 (引用[处理器对象](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10))并[PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10)))。

队列从理论上讲还提供了该公式：*N* = *U* k &divide; (1 &ndash; *U* k) 来估计利用的基础等待项的数目 ( *N*是队列长度）。 利用率的所有时间间隔将此图表提供了以下估计从中获取处理器上的队列位于任何给定的 CPU 负载的多长时间。

![队列长度](media/capacity-planning-considerations-queue-length.png)

它观察到，50%的 CPU 负载，平均有后始终的另一个要在队列中等待，明显快速增大后大约 70%的 CPU 使用率。

返回到本部分中前面使用的驾驶相似之处：

- "下午"的空闲的时间，例如，在某处 40%到 70%范围内。 没有足够多的流量这样的功能选取任何球道不是 majorly 受到限制，并且所用的方式，高，同时另一个驱动程序的可能性不需要付出的努力"查找"公路上其他汽车较安全的差异的级别。
- 一个将注意到，随着流量的临近高峰时段，道路系统接近 100%容量。 更改通道可能会极具挑战性，因为汽车太近一起增加的时必须谨慎以执行此操作。

这就是从长远来看求平均值保守估计为 40%的容量允许的净空间异常峰值负载，无论说峰值暂时性 （如运行几分钟的效果不佳编码查询） 或异常喷发一般情况下负载 （第二天早上的原因第一个之后的日期长周末）。

上述语句将成为相同利用率法律就会执行的常规的读取器以便简化 %处理器时间百分比计算。 有关这些数学上更严格的：  
- 转换[PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = 100 ns 逻辑处理器的"空闲"线程所花费的时间间隔数。 中的更改"*X*"中用户定义变量[PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))计算
  - *T* = 100 纳秒间隔在给定的时间范围内的总数。 中的更改"*Y*"中用户定义变量[PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))计算。
  - *U* k ="空闲线程"或 %通过的逻辑处理器的利用率百分比 Idle Time。  
- 数学计算出的工作：
  - *U* k = 1-%处理器时间百分比
  - 处理器时间百分比 = 1 – *U* k
  - 处理器时间百分比 = 1 – *B* / *T*
  - 处理器时间百分比 = 1 – *X1* – *X0* / *Y1* – *Y0*

### <a name="applying-the-concepts-to-capacity-planning"></a>将概念应用于容量规划

前面的数学计算可能会使系统中所需的逻辑处理器数决定看起来极其复杂。 这就是原因大小调整系统的方法侧重于确定基于当前的最大目标利用率加载和计算来实现此目的所需的逻辑处理器数。 此外，逻辑处理器的速度将产生重大影响性能，缓存效率、 内存一致性要求、 线程计划和同步，而不能准确平衡客户端加载将所有有重大影响基于服务器的服务器而异的性能。 使用相对比较便宜的计算能力的成本，尝试进行分析并确定所需的 Cpu 的完美数目变得更多的学术练习比它提供业务价值。

40%不是硬性要求，它是一个合理的起点。 Active Directory 的各种使用者需要不同级别的响应能力。 可能会的情况下，环境可以运行在作为持续的平均值，80%或 90%的利用率，因为访问处理器的增加的等待时间不会显著影响客户端性能。 务必重新循环访问在系统中，包括对 RAM 的访问速度远低于逻辑处理器的系统中有多个区域，请访问磁盘，并通过网络传输响应。 所有这些项需要优化一起使用。 示例：

- 将多个处理器添加到运行受磁盘限制的 90%的系统可能不打算显著提高性能。 更深入的分析系统可能将标识，存在大量未甚至获得处理器因为它们正在等待 I/O 完成线程。
- 解决磁盘绑定问题意味着以前耗费了大量时间处于等待状态的线程将不再可能处于等待状态的 I/O 并将有更多的竞争 CPU 时间，也就是说，在之前的 90%利用率（因为它可以会更高版本），例如将转到 100%。 这两个组件需要优化一起使用。
  > [!NOTE]
  > 处理器 Information(*)\\%处理器实用程序可能会超过 100%，与具有"Turbo"模式的系统。  这是在 CPU 超过了已评分的处理器速度短时间内。  参考 CPU 制造商文档和更好地了解有关计数器的说明。  

讨论整个系统利用率注意事项还将带到会话域控制器作为虚拟化来宾。 [响应时间 / 系统的日常工作如何影响性能](#response-timehow-the-system-busyness-impacts-performance)适用于主机和来宾虚拟化方案中的。 这就是为什么只有一个来宾与主机中的，域控制器 （和通常任何系统） 具有相同的性能与其在物理硬件附近。 将其他来宾添加到主机提高了基础的主机，从而提高了等待时间以获取对处理器的访问权限，如上文所述的利用率。 简单地说，需要在这两个主机和来宾级别进行管理的逻辑处理器利用率。

扩展前面的类比，保留高速公路的物理硬件，在来宾 VM 将 analogized 总线 （希望是直接目标 rider express 总线） 中。 假设以下四种方案：

- 它是空闲时间、 rider 是几乎是空的总线上获取和总线获取也几乎是空的道路上。 因为没有要应对没有流量，rider 具有很好轻松持续一段时间，而像 rider 必须改为驱动，获取存在只是速度一样快。 Rider 的出差次数仍受速度限制。
- 它是空闲时间因此总线是几乎为空，但大部分旅途通道都将关闭，因此仍拥塞高速公路。 Rider 是堵塞公路上几乎空总线上。 虽然 rider 坐的位置在总线中没有大量的竞赛，仍由外部流量的其余部分指定的总行程时间。
- 它是高峰时段，因此拥塞高速公路和总线。 需要在行程时间较长，不仅获取打开和关闭总线是一场，因为人是即时权限提升到即时权限提升，高速公路不是好得多。 添加更多总线 （到来宾的逻辑处理器） 并不意味着它们还可以放在路上任何更轻松地或行程将被截短。
- 最后一种方案，尽管它可能会被拉伸比喻这种情况有点，其中总线已满，而不是在路途拥塞。 而 rider 仍有问题获取打开和关闭总线，总线是在路上后将高效行程。 这是在其中添加更多总线 （到来宾的逻辑处理器） 将提高来宾性能的唯一情形。

从此处是影响的相对较容易进行推断有许多方案之间 0%利用率和道路的 100%利用率状态和有不同程度的总线 0%和 100%利用率状态。

应用上面的主体的 40%，CPU 为合理的目标为主机和来宾是上面的队列作为同理合理开始。

## <a name="appendix-b-considerations-regarding-different-processor-speeds-and-the-effect-of-processor-power-management-on-processor-speeds"></a>附录 B：有关不同的处理器速度和处理器速度上的处理器电源管理的效果的注意事项

所有处理器所选内容上章节： 处理器正在运行的时钟速度的 100%处收集数据的整个时间且替换系统将具有相同速度的处理器进行假设。 尽管在实践中正在其中默认电源计划 false，尤其是 Windows Server 2008 R2 和更高版本，这两个假设**平衡**，因为它是保守的做法，仍然保持完整方法。 潜在的错误率可能会增加，而它只会增加安全处理器速度提高的边距。

- 例如，在 11.25 Cpu 其中要求的如果处理器运行速度一半时收集数据的情况下，更准确的估算值可能是 5.125 &divide; 2。
- 它不可能保证，加倍时钟速度将双精度的给定的时间段内发生的处理量。 这是因为处理器所用的 RAM 上等待的时间量或其他系统组件可能保持不变。 净效果是更快的处理器可能会花费更高的等待要提取的数据时的空闲时间百分比。 同样，建议继续使用最低通用标准，被保守，并避免尝试计算可能 false 程度的准确性，通过假定处理器速度的线性比较。

或者，如果低于当前硬件处理器速度，在更换硬件，它可以放心地增加处理器所需的少量成比例的估计值。 例如，计算，需要 10 个处理器来维持在站点中，负载和当前处理器运行 3.3 ghz，并替换处理器将运行 2.6 ghz，21%降低速度。 在这种情况下，12 处理器会推荐的数量。

话虽如此，但这种变化可能不会更改的容量管理处理器利用率目标。 将根据要求，运行更高的负载下，系统负载动态调整处理器的时钟速度将生成何处 CPU 消耗更多的时间在更高版本的时钟速度状态，使最终的目标是以 100%中的 40%利用率的方案在高峰期的时钟速度状态。 早于，将生成为 CPU 速度的能源节约的任何内容，将限制返回期间关闭峰值方案。

> [!NOTE]
> 一个选项是关闭的处理器上的电源管理 (将电源计划设置为**高性能**) 收集数据的过程。 在目标服务器上将会得到的 CPU 占用率的更准确地表示形式。

若要调整不同处理器的估计值，它用于为安全起见，不包括其他系统的瓶颈，上述体系结构来假设，加倍处理器速度增加一倍的可执行的处理量。  目前，处理器的内部体系结构是不同的仪表不是从提取数据时使用不同的处理器的效果的更安全方法是利用 SPECint_rate2006 基准检验标准性能评估从处理器之间公司。

1. 找到的处理器，正在使用中，要使用的计划 SPECint_rate2006 分数。
    1. 在标准性能评估机构的网站，选择**结果**，突出显示**CPU2006**，然后选择**搜索所有 SPECint_rate2006 结果**。
    1. 下**简单请求**，输入搜索条件的目标处理器，例如**处理器匹配 E5-2630 (baselinetarget)** 和**处理器匹配 E5-2650 （基线）** .
    1. 查找要使用的服务器和处理器配置 （或类似的关闭，未提供完全匹配时） 并记下值中的**结果**并 **# 内核**列。
1. 若要确定修饰符，请使用以下公式：
   >((*目标平台基于核心分数值*) &times; (*MHz 按核心的基线平台*)) &divide; ((*基线按核心分数值*) &times;(*MHz 按核心目标平台的*))  

    使用上面的示例：
   >(35.83 &times; 2000) &divide; (33.75 &times; 2300) = 0.92
1. 估计的处理器数乘以修饰符。  在上面的示例从 E5 2650 转到 E5 2630 处理器的处理器相乘计算 11.25 Cpu &times; 0.92 = 10.35 所需的处理器。

## <a name="appendix-c-fundamentals-regarding-the-operating-system-interacting-with-storage"></a>附录 C：有关与存储交互的操作系统的基础知识

队列的理论概念中所述[响应时间 / 系统的日常工作如何影响性能](#response-timehow-the-system-busyness-impacts-performance)还有适用于存储。 具有您所熟悉的操作系统句柄 I/O 的必要应用这些概念的方式。 在 Microsoft Windows 操作系统，为每个物理磁盘创建一个队列来保存输入/输出请求。 但是，需要进行明确说明物理磁盘上。 数组控制器和 San 提供给系统的心轴的聚合为单个物理磁盘。 此外，数组控制器和 San 可以聚合到一个数组集的多个磁盘，然后拆分此数组设置为多个"分区"，进而提供给系统作为多个物理磁盘 （请图）。

![块心轴](media/capacity-planning-considerations-block-spindles.png)  

在此图中两个心轴是镜像卷和拆分为用于数据存储 （Data 1 和 2） 的逻辑区域。 这些逻辑区域被视为由操作系统单独的物理磁盘。

虽然这可以是高度令人困惑，但下列术语全篇中用于本附录标识不同的实体：

- **轴-** 以物理方式安装在服务器中的设备。
- **数组-** 控制器进行聚合的心轴集合。
- **数组分区 –** 聚合数组分区
- **LUN –** 数组、 引用时使用 San
- **磁盘 –** 操作系统观察是单个物理磁盘。
- **分区-** 逻辑分区的操作系统可以体验为物理磁盘。

### <a name="operating-system-architecture-considerations"></a>操作系统体系结构注意事项

操作系统会创建未观察到; 每个磁盘的第一个 In/First 出 (FIFO) I/O 队列一个轴、 数组或数组分区，则可能表示此磁盘。 从操作系统的角度看，对于处理 I/O，多个活动进行排队，性能越好。 为先进先出队列已序列化，这意味着，必须按顺序处理所有颁发给的存储子系统的 I/o 请求到达。 通过将由操作系统使用的主轴/数组观察到每个磁盘相关联，操作系统现在维护每个唯一组的磁盘，从而在磁盘上消除稀缺 I/O 资源争用和隔离到单个的 I/O 需求的 I/O 队列磁盘。 为异常，Windows Server 2008 引入了的 I/O 优先级排列的概念和应用程序设计为使用"低"优先级脱离此正常顺序和需要后席位。 应用程序不是专门编码以利用"低"优先级默认为"Normal"。

### <a name="introducing-simple-storage-subsystems"></a>引入了简单的存储子系统

启动一个简单的示例 （单个硬盘驱动器计算机内部的） 将提供的组件由组件分析。 打破这为主要的存储子系统组件，包括系统：

- **1 –** 10,000 RPM 超高快速 SCSI HD （超高的快速 SCSI 具有的 20 MB/秒传输速率）
- **1 –** SCSI 总线 （电缆）
- **1 –** 超高的快速 SCSI 适配器
- **1 –** 32 位 33 MHz PCI 总线

一旦识别出组件，可以计算的数据量可以传输系统，或者可以处理多少 I/O，了解。 请注意，I/O 量和可以传输系统的数据量相关，但并不相同。 此相关依赖于磁盘 I/O 是随机或顺序和块大小。 （所有数据都写入到磁盘作为一个块，但使用不同的不同应用程序块大小。）基于组件的组件：

- **硬盘 –** 平均 10,000 RPM 硬盘驱动器有 7 毫秒 (ms) 寻道时间和 3 毫秒访问时间。 查找时间是读/写磁头能够在盘片上移动到的位置所需的时间的平均量。 访问时间是时间的平均读取或正确的位置标头后将数据写入到磁盘，花费量。 因此，用于读取 10,000 RPM HD 中的数据的唯一块的平均时间构成搜寻和总共大约 10 毫秒 （或.010 秒为单位） 的访问，每个数据块。

  当每个磁盘访问需要时移动的开头到磁盘上的新位置时，读/写行为被称为"随机"。 因此，当所有 I/O 都是随机的 10,000 RPM HD 可以处理大约 100 每秒 I/O (IOPS) (该公式都是每秒 10 毫秒，每个 I/O 或 1000年/10 除以 1000 毫秒 = 100 IOPS)。

  或者，从 HD 上相邻扇区的所有 i/o 操作时，这称为顺序 I/O。 因为第一次 I/O 完成后，在开始下一个数据块 HD 上的存储位置是读/写磁头寻道时间顺序 I/O 没有。 因此 10,000 RPM HD 很强的处理大约 333 I/O 每秒 （每秒除以 3 毫秒，每个 I/O 的 1000 毫秒）。

  >[!NOTE]
  >此示例不会反映磁盘缓存，其中的一个柱面数据通常保存。 在这种情况下，第一次 I/O 上所需 10 毫秒和磁盘读取整个柱状体。 从缓存满足所有其他顺序 I/O。 因此，在磁盘缓存可以提高顺序 I/O 性能。
  
  到目前为止，已不相关的硬盘驱动器的传输速率。 硬盘空间是否为 20 MB/秒超高宽或 Ultra3 160 MB/秒，实际的 IOPS 可量由 10,000 RPM HD 为 ~ 100 随机或 ~ 300 顺序 I/O。 应用程序写入驱动器上基于的块的大小改变，是不同的每个 I/O 请求的数据量。 例如，如果块大小为 8 KB，100 个 I/O 操作将读取或写入到硬盘总共 800 KB。 但是，如果块大小为 32 KB，100 I/O 将读/写 3,200 KB (3.2 MB) 到硬盘驱动器。 只要 SCSI 传输速率已超过总数据传输量，则获取"快"的传输速率驱动器将获得执行任何操作。 请参阅下面的表以比较。

  | |7200 RPM 9ms 查找，4ms 访问|10,000 RPM 7 毫秒查找，3 毫秒访问|15,000 RPM 4ms 查找，2ms 访问
  |-|-|-|-|
  |随机 I/O|80|100|150|
  |顺序 I/O|250|300|500|  
  
  |10,000 RPM 驱动器|8 KB 块大小 (Active Directory Jet)|
  |-|-|
  |随机 I/O|800 KB/秒|
  |顺序 I/O|2400 KB/秒|

- **SCSI 底板 （总线） –** 了解如何的"SCSI 底板 （总线）"，或在此方案的功能区电缆，影响的存储子系统的吞吐量取决于知识的块大小。 是实质上是问题，如果 I/O 是以 8 KB 的块 I/O 可以总线句柄的多少？ 在此方案中，在 SCSI 总线是 20 MB/秒或 20480 KB/秒。 20480 KB/s 除以 8 KB 的块会产生最多支持的 SCSI 总线大约 2500 IOPS。

  >[!NOTE]
  >下表中的数字表示一个示例。 最附加的存储设备当前使用 PCI Express，它提供更高的吞吐量。  
  
  |支持的 SCSI 总线，每个块大小的 I/O|2 KB 块大小|8 KB 块大小 (AD Jet) (SQL Server 7.0/SQL Server 2000)
  |-|-|-|
  |20 MB/秒|10,000|2,500|
  |40 MB/秒|20,000|5,000|
  |128 MB/秒|65,536|16,384|
  |320 MB/秒|160,000|40,000|

  因为可以确定在此图表中，在此方案，无论何种使用，总线将永远不会成为瓶颈，作为轴最大值为 100 I/O 远低于任一上述的阈值。

  >[!NOTE]
  >此操作假定 SCSI 总线高效的 100%。
  
- **SCSI 适配器 –** 用于确定这可以处理的 I/O 量，制造商的规范需要检查。 I/O 将请求定向到适当的设备需要某种类型的处理，因此可以处理的 I/O 量取决于 SCSI 适配器 （或数组控制器） 处理器。

  在此示例中，假设 1,000 I/O 可处理将进行。

- **PCI 总线 –** 这是一个经常被忽视的组件。 在此示例中，这将不是瓶颈;但是系统纵向扩展，它可能成为瓶颈。 有关参考，操作为 33 Mhz 32 位 PCI 总线可以在理论上传输 133 MB/秒的数据。 下面是公式：  
  > 32 位&divide;每个字节的 8 位&times;33 MHz = 133 MB/s。  

  请注意，理论限制;在现实中仅约 50%的最大实际达到，尽管在某些突发情况下，效率 75%可获取短时间内。

  66 Mhz 64-bit PCI 总线可以支持的理论最大值 (64 位&divide;每个字节的 8 位&times;66 Mhz) = 528 MB/秒。 此外，任何其他设备 （如网络适配器，第二个 SCSI 控制器和等等） 会减少可用带宽共享带宽以及设备将争用有限资源。

在分析后的存储子系统的组件，心轴是量可以请求的 I/O，因此可以传输系统的数据量的限制因素。 具体而言，在 AD DS 方案中，这是 100 随机 I/O 每秒 8 KB 的增量，总共 800 KB 每第二个时访问 Jet 数据库。 或者，一个以独占方式分配日志文件的心轴的最大吞吐量将会遇到以下限制：每秒 8 KB 的增量，每秒 2400 KB (2.4 MB) 的总计 300 顺序 I/O。

现在，让分析的简单配置下, 表演示其中瓶颈会进行更改或添加的存储子系统中的组件。

|说明|瓶颈分析|Disk|Bus|适配器|PCI 总线|
|-|-|-|-|-|-|
|这是添加第二个磁盘后的域控制器配置。 合盒砞 ﹚ 表示 800 KB/秒的瓶颈。|添加 1 个磁盘 (总 = 2)<br /><br />是随机的 I/O<br /><br />4 KB 块大小<br /><br />10,000 RPM HD|200 I/o 总数<br />800 KB/s 总数。| | | |
|添加后 7 个磁盘，磁盘配置仍然表示 3200 KB/秒的瓶颈。|**添加 7 个磁盘 (总 = 8)**  <br /><br />是随机的 I/O<br /><br />4 KB 块大小<br /><br />10,000 RPM HD|800 I/o 总数。<br />3200 KB/s 总计| | | |
|更改为顺序 I/O 后, 的网络适配器成为瓶颈，因为它被限制为 1000 IOPS。|添加 7 个磁盘 (总 = 8)<br /><br />**I/O 是按顺序**<br /><br />4 KB 块大小<br /><br />10,000 RPM HD| | |2400 I/O 秒可以读/写到磁盘中，控制器限制为 1000 IOPS| |
|在替换支持 10,000 IOPS 的 SCSI 适配器的网络适配器后, 瓶颈返回磁盘配置。|添加 7 个磁盘 (总 = 8)<br /><br />是随机的 I/O<br /><br />4 KB 块大小<br /><br />10,000 RPM HD<br /><br />**SCSI 适配器 (现在支持 10,000 I/O) 升级**|800 I/o 总数。<br />3,200 KB/s 总计| | | |
|增加到 32 KB 的块大小之后, 在总线成为瓶颈，因为它仅支持 20 MB/秒。|添加 7 个磁盘 (总 = 8)<br /><br />是随机的 I/O<br /><br />**32 KB 块大小**<br /><br />10,000 RPM HD| |800 I/o 总数。 25,600 KB/秒 （25 MB/秒） 可以是读/写到磁盘。<br /><br />总线仅支持 20 MB/秒| | |
|后升级到总线，并且添加更多磁盘，则磁盘将仍然瓶颈。|**添加 13 个磁盘 (总 = 14)**<br /><br />添加包含 14 个磁盘的第二个 SCSI 适配器<br /><br />是随机的 I/O<br /><br />4 KB 块大小<br /><br />10,000 RPM HD<br /><br />**升级到 320 MB/秒 SCSI 总线**|2800 I/Os<br /><br />11,200 KB/秒 （10.9 MB/秒）| | | |
|后更改为顺序 I/O，则磁盘将仍然瓶颈。|添加 13 个磁盘 (总 = 14)<br /><br />添加包含 14 个磁盘的第二个 SCSI 适配器<br /><br />**I/O 是按顺序**<br /><br />4 KB 块大小<br /><br />10,000 RPM HD<br /><br />升级到 320 MB/秒 SCSI 总线|8,400 I/Os<br /><br />33,600 KB\s<br /><br />(32.8 MB\s)| | | |
|添加后更快的硬盘驱动器，则磁盘将仍然瓶颈。|添加 13 个磁盘 (总 = 14)<br /><br />添加包含 14 个磁盘的第二个 SCSI 适配器<br /><br />I/O 是按顺序<br /><br />4 KB 块大小<br /><br />**15,000 RPM HD**<br /><br />升级到 320 MB/秒 SCSI 总线|14,000 I/o<br /><br />56000 KB/秒<br /><br />（54.7 MB/秒）| | | |
|增加到 32 KB 的块大小后, PCI 总线成为瓶颈。|添加 13 个磁盘 (总 = 14)<br /><br />添加包含 14 个磁盘的第二个 SCSI 适配器<br /><br />I/O 是按顺序<br /><br />**32 KB 块大小**<br /><br />15,000 RPM HD<br /><br />升级到 320 MB/秒 SCSI 总线| | | |14,000 I/o<br /><br />448,000 KB/秒<br /><br />（437 MB/秒） 是到主轴的读/写限制。<br /><br />PCI 总线支持 133 MB/秒 （最高效的 75%) 的理论最大值。|

### <a name="introducing-raid"></a>引入了 RAID

引入数组控制器; 时，存储子系统的性质不会显著更改它只需将替换在计算中的 SCSI 适配器。 什么会更改为读取和使用 （如 RAID 0，RAID 1 或 RAID 5) 的各种数组级别时向磁盘写入数据的成本。

在 RAID 0，数据的条带化所有 RAID 中的磁盘设置中。 这意味着在读取或写入操作，过程一部分数据是从请求或推送到每个磁盘，增加可在同一时间段内传输系统的数据量。 因此，在一秒中 （同样假设 10,000 RPM 的驱动器），每个心轴上 100 次 I/O 操作可以执行。 可以支持的 I/O 的总金额是次数为 100 的 N 主轴上每秒每个轴 (每秒产生 100 * N I/O) 的 I/O。

![逻辑 d： 驱动器](media/capacity-planning-considerations-logical-d-drive.png)

RAID 1 的数据 （复制） 在一对冗余的心轴之间镜像。 因此，当执行读取的 I/O 操作时，可以从这两个集中心轴读取数据。 这实际上会从这两个磁盘 I/O 容量可用在读取操作过程。 需要注意的是 RAID 1 中写入操作提升任何性能优势。 这是因为相同的数据需要写入到为了冗余这两个驱动器。 尽管它不会超过此时间的数据写入发生时同时在这两个心轴，因为这两个轴都被占用，会出现数据重复，写入 I/O 操作本质上可防止两个读取的操作的发生。 因此，每个写入 I/O 开销两个读取 I/O。 从该信息来确定发生的 I/O 操作的总数，可以创建一个公式：  

> *读取 I/O* + 2 &times; *写入 I/O* = *总可用的磁盘 I/O 使用*  

在比率为读取比写入并心轴数已知的可以从上面的公式来标识数组可以支持的最大 I/O 派生以下公式：  

> *每个轴的最大 IOPS* &times; 2 个心轴&times;[( *%读取* +  *%写入*) &divide; ( *%读取*+ 2 &times; *%写入*)] =*的总 IOPS*

RAID 1 + 0，有关读取和写入的费用的行为与 RAID 1 完全相同。 但是，I/O 是现在个条带化每个镜像集。 如果  

> *每个轴的最大 IOPS* &times; 2 个心轴&times;[( *%读取* +  *%写入*) &divide; ( *%读取*+ 2 &times; *%写入*)] =*的总 I/O*  

在 RAID 1 集中，当重数 (*N*) 的 RAID 1 条带化集，可以处理的 I/O 总数将成为 N &times; I/O 的每个 RAID 1 集：  

> *N* &times; {*每个轴的最大 IOPS* &times; 2 个心轴&times;[( *%读取* +  *%写入*) &divide;( *%读取*+ 2 &times; *%写入*)]} =*的总 IOPS*

在 RAID 5 中，有时称为*N* + 1 RAID，数据个条带化*N*心轴数和奇偶校验信息写入到"+ 1"主轴。 但是，RAID 5 是昂贵得多，执行比 RAID 1 或 1 + 0 是写入 I/O 时。 每次写入 I/O 提交到的数组，RAID 5 将执行以下过程：

1. 读取旧数据
1. 读取旧的奇偶校验
1. 写入新数据
1. 编写新的奇偶校验

因为每个写入 I/O 请求都由操作系统提交到数组控制器需要四个 I/O 操作完成，写入请求提交需要四次这么长时间完成，因为单个读取 I/O。 若要派生一个公式，将从操作系统的角度来看的心轴所经历的 I/O 请求转换：  

> *读取 I/O* + 4 &times; *写入 I/O* = *的总 I/O*  

同样集中 RAID 1 时已知的读取写入和心轴数的比率，以下公式可从上面的公式来标识数组 （请注意，心轴的总数不包括可支持的最大 I/Oe"drive"丢失到奇偶校验）：  

> *每个轴的 IOPS* &times; (*心轴*– 1) &times; [( *%读取* +  *%写入*) &divide; ( *%读取*+ 4 &times; *%写入*)] =*的总 IOPS*

### <a name="introducing-sans"></a>引入了 San

扩展的存储子系统，复杂性 SAN 引入到环境时，所述的基本原则不会更改，但是连接到 SAN 的所有系统的 I/O 行为需要考虑在内。 由于使用 SAN 中的主要优势之一是其他数量的冗余内置或外置方式附加的存储方式相比，容量规划现在需要考虑到帐户的容错需求。 此外，引入更多的组件的需要进行评估。 划分 SAN 组件的各部分：

- SCSI 或光纤通道的硬盘空间
- 存储单元通道底板
- 存储单元
- 存储控制器模块
- SAN 开关
- HBA(s)
- PCI 总线

设计以实现冗余任何系统时，其他组件是包括在内，以适应故障的可能性。 它是非常重要的是，当容量规划，以从可用资源中排除的冗余组件。 例如，如果 SAN 具有两个控制器模块，一个控制器模块的 I/O 容量是所有应用于可用于系统的总 I/O 吞吐量。 这是由于一个控制器发生故障，如果所要求的所有连接的系统的整个 I/O 负载需要处理的另一个控制器。 为高峰使用周期完成所有容量规划，如冗余组件不应考虑到可用资源和计划的高峰使用率 （以适应突发情况或异常系统不应超过 80%的系统的饱和度行为）。 同样，冗余的 SAN 交换机、 存储单元和心轴都应不考虑到 I/O 计算。

当对硬盘 SCSI 或光纤通道的行为进行分析，分析行为，如所述的方法之前不会更改。 虽然有特定的优点和缺点到每个协议，但每个磁盘的限制因素是机械硬盘空间的限制。

分析存储单元上的通道是完全相同计算可用带宽 （如 20 MB/s) 除以块大小 （例如 8 KB) 的 SCSI 总线上的资源。 这背离简单的上一个示例是在多个通道的聚合。 例如，如果有 6 个声道，每个都支持 20 MB/秒的最大传输速率的总可用的 I/O 和数据传输量是 100 MB/秒 （这是正确的它是不 120 MB/秒）。 同样，容错是此计算，整个通道丢失的主要播放器，系统仅左侧具有 5 个正常运行的通道。 因此，若要确保继续满足性能预期发生故障时，所有存储通道的总吞吐量不应超过 100 MB/秒 （假定负载和容错能力均匀地分布在所有通道）。 启用该选项到 I/O 配置文件是依赖于应用程序的行为。 在 Active Directory Jet I/O 的情况下这会关联到大约 12500 每秒 I/O 数 (100 MB/秒&divide;每个 I/O 的 8 KB)。

接下来，获取制造商的规范的控制器模块则必须以深入了解有关每个模块可以支持的吞吐量。 在此示例中，SAN 该支持 7,500 I/O 每个具有两个控制器模块。 如果不需要冗余，系统的总吞吐量可能会 15,000 IOPS。 在计算发生故障的最大吞吐量，限制是一个控制器或 7500 IOPS 的吞吐量。 此阈值远低于中的所有存储通道可以支持的 12,500 （假设 4 KB 块大小） 的 IOPS 最大，因此，目前在分析中的瓶颈。 所需的最大 I/O 计划执行仍为进行规划，是 10400 I/O。

当数据退出控制器模块时，传输速率： 1 GB/秒 （或每秒 1 千兆位） 的光纤通道连接。 若要将此与其他度量值相关联，1 GB/s 将转变为 128 MB/秒 (1 GB/秒&divide;8 位/字节)。 由于这是在所有渠道中的存储单位 （100 MB/秒） 超出总带宽，这不会妨碍系统。 此外，因为这是仅有一个两个通道 （额外 1 GB/s 光纤通道连接用于冗余），如果一个连接失败，其余连接仍有足够的容量来处理所需的所有数据传输。

消息传递到服务器，数据将很可能传输 SAN 交换机。 SAN 交换机必须处理传入的 I/O 请求，并转发其相应的端口，此开关将拥有可以处理的 I/O 量限制，但是，制造商规范将需要确定该限制是。 例如，如果有两个开关，每个开关可以处理 10,000 IOPS，总吞吐量为 20,000 IOPS。 同样，容错问题，如果一个交换机出现故障，系统的总吞吐量将为 10,000 IOPS。 它将根据需要不能超过 80%利用率在正常操作中，使用不超过 8000 I/O 应为目标。

最后，服务器中安装 HBA 还会限制它可以处理的 I/O 量。 通常情况下，第二个 HBA 安装以实现冗余，但计算可以处理的总吞吐量的最大 I/O 时，就像使用 SAN 开关*N* &ndash; 1 Hba 是什么是系统的最大可伸缩性。

### <a name="caching-considerations"></a>缓存的注意事项

缓存是一种可能会显著影响存储系统中任何位置的整体性能的组件。 有关缓存的算法的详细的分析不在范围内的这篇文章;但是，有关缓存磁盘子系统上的一些基本语句是值得说明：

- 缓存执行改进了持续的顺序写入 I/O，因为它可以缓冲到更大的 I/O 块中许多较小的写入操作和数据转储到更少，但更大块大小中的存储。 这将减少随机 I/O 总数和总顺序 I/O，从而为其他 I/O 提供更高的资源可用性。
- 缓存不会不会提高持续的写入的存储子系统的 I/O 吞吐量。 它只允许对写入会缓冲，直到心轴都可用于将数据提交。 时可用的存储子系统中轴的所有 i/o 操作处于满负荷状态很长一段，则缓存将最终填满。 若要清空缓存，足够喷发或额外的心轴之间的时间，需要分配以提供足够 I/O，从而使缓存刷新。

  更多的数据会缓冲，仅允许更大的缓存。 这意味着可以容纳的饱和度值更长时间。

  通常情况下运行的存储子系统，随着数据只需将写入缓存，操作系统将经历提高的写入性能。 一旦基础媒体处于满负荷状态与 I/O，将填充缓存并写入性能将返回到磁盘速度。

- 缓存读取 I/O，缓存是最有利的情况时，数据按顺序在磁盘上存储和缓存可以预读 （它生成的下一步的扇区包含接下来将请求的数据的假设） 时。
- 当读取 I/O 是随机的在驱动器控制器缓存不太可能提供任何可以从磁盘读取的数据量的增强功能。 任何增强功能是不存在的操作系统或应用程序基于缓存大小是否大于基于硬件的缓存大小。

  对于 Active Directory 中，缓存仅限于按 RAM 量。

### <a name="ssd-considerations"></a>SSD 的注意事项

SSDs 是比基于您的硬盘完全另一回事。 但仍能两个关键条件："多少 IOPS 能处理？" 和"什么是这些 IOPS 的延迟？" 与基于您的硬盘，相比 Ssd 可以处理更高的 I/O 卷，并且可以具有更低的延迟。 一般情况下，在撰写本文时，仍然很高的成本 /gb 比较成本 Ssd 时，它们是成本每个的 I/o 方面开销很低，值得在存储性能方面的重要考虑因素。

注意事项：

- IOPS 和延迟是非常主观向制造商设计，并在某些情况下观察到为性能不是主轴基于的技术。 简单地说，是查看和验证制造商规格驱动器的驱动器，并假定任何 generalities 更为重要。
- IOPS 类型可以具有非常不同的数字，具体取决于是否被读取或写入。 AD DS 服务，一般情况下，主要是为了读取为基础，将一些其他应用程序方案相比不太受影响。
- "写入耐用性"– 这是 SSD 单元格最终磨损的概念。各个制造商将不同方式处理这一挑战。 至少为数据库驱动器中，主要是为了读取的 I/O 配置文件允许轻描淡写这一问题的重要性，因为数据不是非常不稳定。

### <a name="summary"></a>总结

考虑存储的一种方法想像家庭基本功能。 假设数据存储在媒体的 IOPS 是家庭主使用量。 这堵塞 （例如在管道中的根） 或有限 （它是折叠或太小），重新启动时进行过多水家庭中的所有接收器使用 （过多来宾） 时。 这是非常类似于其中一个或多个系统利用 SAN/NAS/iSCSI 使用相同的基础媒体上的共享的存储的共享环境。 可以采用不同的方法，以解决不同的方案：

- 折叠或较小漏需要完整的规模替换和修补程序。 这将是类似于在新硬件中添加或重新发布使用共享的存储整个基础结构的系统。
- "被堵塞"管道通常意味着有问题的一个或多个问题的标识和删除这些问题。 在存储方案中，这可能是存储或系统级别的备份，跨所有服务器，并在高峰期间同步碎片整理软件运行同步防病毒扫描。

在任何探测设计中，多个清空后馈送到主使用量。 如果任何内容停止其中一个这些清空后或一个交接点，只后面这个交接点关注点备份。 在存储方案中，这可能是重载的交换机 （SAN/NAS/iSCSI 方案）、 驱动程序兼容性问题 (错误驱动程序/HBA Firmware/storport.sys 组合) 或备份/防病毒/碎片整理。 若要确定存储"管道"是否足够大，需要测量 IOPS 和 I/O 大小。 在每个联合将其添加在一起，确保足够"管道直径。"

## <a name="appendix-d---discussion-on-storage-troubleshooting---environments-where-providing-at-least-as-much-ram-as-the-database-size-is-not-a-viable-option"></a>附录 D-讨论存储故障排除的环境，因为数据库大小不可行的选项提供至少像会 RAM

它是有助于您了解这些建议为什么存在，以便可以容纳存储技术中的更改。 这些建议存在以下两个原因。 第一个是隔离的 IO，以便在操作系统主轴上 （分页） 的性能问题不会影响性能的数据库和 I/O 配置文件。 第二个是用于 AD DS （与大多数数据库） 的日志文件实际上是按顺序，并基于您的硬盘驱动器和缓存具有顺序 I/O 相比更随机 I/O 模式的操作系统和几乎与使用一项非常大的性能优势纯随机 I/O 模式的 AD DS 数据库驱动器。 通过隔离到单独的物理驱动器的顺序 I/O，可以增加吞吐量。 主讲人今天的存储选项的难题是这些建议背后的基本假设不再成立。 在许多虚拟化存储方案，如 iSCSI SAN、 NAS 和虚拟磁盘图像文件、 基础的存储媒体共享跨多个主机，因此完全抵消 IO"隔离"和"顺序 I/O 优化"方面。 事实上这些方案将添加一层额外的复杂性，因为访问共享的媒体的其他主机可能会降低对域控制器的响应能力。

在规划存储性能，有三个类别，需要考虑： 冷缓存状态、 上做好准备缓存状态和备份/还原。 在方案，例如当最初重新启动域控制器，出现冷缓存状态或重新启动 Active Directory 服务和在 RAM 中没有 Active Directory 数据。 预热缓存状态是域控制器将处于稳定状态和缓存数据库的位置。 这些注意事项很重要，需要注意，即它们将驱动器非常不同的性能配置文件，并具有足够的 RAM 来缓存整个数据库没有任何帮助性能时缓存处于低温状态。 一个可以考虑使用下面的假设，预热冷缓存这两种方案的性能设计是"冲刺 （sprint）"，运行具有热缓存服务器是"marathon。"

对于冷缓存和热缓存方案，这个问题变得速度存储可以将数据从磁盘到内存中。 预热缓存是一种方案，随着时间推移，性能可提高，因为更多的查询重用数据、 缓存命中速率增加和减少无需转到磁盘的频率。 因此会降低转到磁盘的不利性能影响。 等待要暖和增长到最大值、 依赖于系统的允许大小的缓存时才暂时性性能降级。 会话可以简化为速度数据可以获得从磁盘，并为简单度量值的可用的 Active Directory 中，IOPS 即主观到可用 IOPS 从基础存储。 从规划角度来看，因为预热缓存和备份/还原方案会发生异常按正常情况下发生空闲时间，而是主观的 DC，加载的一般建议不存在除外，这些活动进行计划对于非高峰时间。

AD DS 中的，在大多数情况下，主要是读取 IO，通常的 90 %read/10%写比率。 读取 I/O 通常往往是用户体验的瓶颈和与写入 IO 原因写入性能下降。 NTDS.dit 的 I/O 是主要是随机的因为缓存趋向于提供优势极小，读取 IO，使其更为重要，若要配置的存储 I/O 配置文件正确读取。

正常操作条件的存储规划目标是最大程度减少 AD DS 为从磁盘中返回的请求的等待时间。 这实质上意味着的未完成和挂起的 I/O 数小于或到磁盘的路径的数字等效的。 有多种方式来测量此。 中的性能监视方案中，常规建议是该逻辑磁盘 ( *\<NTDS 数据库驱动器\>* ) \Avg Disk sec/Read 是小于 20 毫秒。 可能的具体取决于存储的类型 2 到 6 毫秒 （.002 到.006 秒） 范围内的所需的操作阈值必须要低得多，最好是为接近的存储区的速度。

例如：

![存储延迟图表](media/capacity-planning-considerations-storage-latency.png)

分析图表：

- **在左侧 – 绿色椭圆**延迟保持一致在 10 毫秒。 在负载增加时从 800 IOPS 为 2400 IOPS。 这是到基础存储可以多快的速度处理 I/O 请求的绝对下限。 这是根据存储解决方案的具体信息。
- **在右侧 – 暗 oval**时延迟不断增加，吞吐量保持不变从退出的绿色圆圈通过对数据集合的末尾。 这演示了当请求量超出基础存储的物理限制，较长的请求将花费坐在队列中等待发送到的存储子系统。

应用此知识：

- **对一大组 – 用户查询成员身份的影响**假定这需要从磁盘的 I/O 活动量读取 1 MB 的数据，因此需要多长时间可以进行评估，如下所示：
  - Active Directory 数据库页的大小是 8 KB。 
  - 需要从磁盘中读取至少 128 页。 
  - 缓存假定执行任何操作，在这将需要最少 1.28 秒从磁盘中加载数据之后才能将其返回到客户端的下限 （10 毫秒）。 在 20 毫秒，其中存储上的吞吐量早已极限，也是建议的最大值，它将需要 2.5 秒从磁盘中获取数据，以便将其返回给最终用户。  
- **以何速率将缓存上做好准备 –** 做出的假设是客户端负载要在此示例中存储吞吐量最大化，缓存将暖的 2400 IOPS 速率&times;每个 IO 的 8 KB。 或者，每个第二个，加载大约 1 GB 的数据库到 RAM 每隔 53 秒的大约 20 MB/秒。

> [!NOTE]
> 是正常的短时间内观察延迟攀升组件主动的方式读取或写入到磁盘，例如当备份系统正在或 AD DS 时运行垃圾回收时。 应提供基于计算的额外头空间以容纳这些定期事件。 目标是能提供足够的吞吐量来应对这些情况，而不会影响正常功能。

可以看出，没有物理限制基于存储设计到可能预热缓存可以多快的速度。 什么将预热高速缓存是传入客户端请求到基础存储可以提供的速率。 运行脚本"预先预热"高速缓存在高峰时间将提供竞赛加载取决于实际的客户端请求。 可以产生负面影响交付数据的客户端需要第一次，因为按照设计，它将生成稀缺磁盘资源争用情况，如人工尝试来预热缓存将加载与联系域控制器的客户端并不相关的数据。
