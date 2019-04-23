---
title: 在 AD 性能优化的硬件注意事项
description: 在 AD 性能优化的硬件注意事项
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 0f1aa1e3c07c5cb9238a332156abfec248e74176
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866088"
---
# <a name="hardware-considerations-in-adds-performance-tuning"></a>中 ADDS 性能优化的硬件注意事项 

>[!Important]
> 以下是需要优化服务器硬件的详细介绍的更高版本中的 Active Directory 工作负荷的关键的建议和注意事项的摘要[Active Directory 域服务的容量规划](https://go.microsoft.com/fwlink/?LinkId=324566)一文。 读者都高度建议，若要查看[Active Directory 域服务的容量规划](https://go.microsoft.com/fwlink/?LinkId=324566)的更高版本的技术知识和这些建议的影响。

## <a name="avoid-going-to-disk"></a>避免从磁盘

因为内存允许，active Directory 将尽可能多的数据库的缓存。 从内存提取页面数量级比要快转到物理介质的介质主轴还是基于 SSD。 添加更多内存来最大程度减少磁盘 I/O。

-   Active Directory 最佳做法建议将足够的 RAM 来加载到内存中，整个 DIT 加上容纳的操作系统和其他已安装的应用程序，如防病毒、 备份软件进行监视，等等。

    -   有关旧的平台限制，请参阅[Lsass.exe 进程的运行 Windows Server 2003 或 Windows 2000 Server 域控制器上的内存使用率](https://support.microsoft.com/kb/308356)。

    -   使用内存\\长期平均待机缓存生存期 (s) &gt; 30 分钟的性能计数器。

-   将操作系统、 日志和数据库放在单独的卷。 如果所有或大多数 DIT 可以缓存，缓存上做好准备，并在稳定状态下，这才会变为不太相关，并提供了一些更灵活地存储数据布局后。 在不能在其中缓存整个 DIT 的情况下，拆分操作系统、 日志和单独的卷上的数据库的重要性变得更重要。

-   通常情况下，到 DIT I/O 比为大约 90%读取和写入 10%。 写入的 I/O 卷会大大超过 10%-20%的情况下被视为具有大量写入操作。 写入密集型方案不会从 Active Directory 缓存大大获益。 若要保证写入目录数据的事务的持续性，Active Directory 不执行磁盘写入缓存。 相反，它提交到磁盘的所有写入操作之前，将返回成功完成状态的操作，除非显式请求无法执行此操作。 因此，较快的磁盘 I/O 对向 Active Directory 写入操作的性能至关重要。 下面是可能会提高性能，对于这些方案的硬件建议：

    -   硬件 RAID 控制器

    -   增加低延迟高 RPM 磁盘托管 DIT 和日志文件的数

    -   写入在控制器上缓存

-   查看单独为每个卷的磁盘子系统性能。 大多数 Active Directory 方案都是主要基于读取的因此托管 DIT 的卷上的统计信息是最重要检查。 但是，不要忽视监视的驱动器，包括操作系统，其余部分和日志文件驱动器。 若要确定域控制器是否正确配置以避免存储区的性能瓶颈，引用在存储子系统上的标准存储建议的部分。 在许多环境中，基本原理是确保有足够的净空间以容纳激增或负载高峰。 这些阈值警告的阈值其中头足够的空间来容纳激增或负载高峰变得约束和客户端响应能力会降低。 简单地说，超过这些阈值不是错误在短期内 （5 到 15 分钟一天几次），但是运行持续使用这些种类的统计信息的系统完全没有缓存数据库，并且可能取决于需要交税，应进行调查。

    -   Database ==&gt; Instances(lsass/NTDSA)\\I/O Database Reads Averaged Latency &lt; 15ms

    -   Database ==&gt; Instances(lsass/NTDSA)\\I/O Database Reads/sec &lt; 10

    -   Database ==&gt; Instances(lsass/NTDSA)\\I/O Log Writes Averaged Latency &lt; 10ms

    -   Database ==&gt; Instances(lsass/NTDSA)\\I/O Log Writes/sec – informational only.

        若要维护数据的一致性，所有更改必须都写入日志。 不好或错误编号，它是仅支持多少存储的度量值。

-   计划非 core 磁盘 I/O 负载，例如备份和防病毒扫描、 非峰值负载时间。 此外，使用备份和防病毒解决方案，支持在 Windows Server 2008，以减少 Active Directory 的 I/O 需求，竞赛中引入的低优先级 I/O 功能。

## <a name="dont-over-tax-the-processors"></a>不超过税务处理器

没有足够的可用周期的处理器可能会导致到处理器的线程获得执行长时间的等待。 在许多环境中，基本原理是确保有足够的净空间以容纳激增或峰值负载，在这些情况下的客户端响应式设置的影响降至最低。 简单地说，超出低于阈值不是错误在短期内 （5 到 15 分钟一天几次），但是运行持续使用这些种类的统计信息的系统不提供任何头足够的空间来容纳异常装载和可以轻松地放入一个负载过重的 s方案。 系统支出持续的期阈值应调查以及减少处理器负载的方式。

-   有关如何选择一个处理器的详细信息，请参阅[性能优化服务器硬件](../../hardware/index.md)。

-   硬件、 优化的负载，将在其他位置，客户端定向中添加或删除负载环境以降低 CPU 负载。

-   使用的处理器信息 (\_总)\\%的处理器利用率&lt;60%性能计数器。

## <a name="avoid-overloading-the-network-adapter"></a>避免重载的网络适配器

就像个处理器，过多的网络适配器使用率都将导致长时间的等待获取到网络上的出站流量。 Active Directory 往往具有较小的入站的请求和相对到明显更大量的数据返回到客户端系统。 发送的数据远远超过了接收到的数据。 在许多环境中，基本原理是确保有足够的净空间以容纳激增或负载高峰。 此阈值是约束将成为其中头足够的空间来容纳激增或负载高峰的警告阈值和客户端响应能力会降低。 简单地说，超过这些阈值不是错误在短期内 （5 到 15 分钟一天几次），但是运行持续使用这些种类的统计信息的系统是通过需要交税，应进行调查。

-   有关如何优化网络子系统的详细信息，请参阅[网络子系统性能调整](../../../../networking/technologies/network-subsystem/net-sub-performance-top.md)。

-   使用比较网络接口 (\*)\\与网络接口的 Bytes Sent/Sec (\*)\\当前带宽性能计数器。 比率应利用率小于 60%。

## <a name="see-also"></a>请参阅
- [性能优化 Active Directory 服务器](index.md)
- [LDAP 注意事项](ldap-considerations.md)
- [正确放置域控制器和站点的注意事项](site-definition-considerations.md)
- [ADDS 性能故障排除](troubleshoot.md) 
- [Active Directory 域服务的容量规划](https://go.microsoft.com/fwlink/?LinkId=324566)