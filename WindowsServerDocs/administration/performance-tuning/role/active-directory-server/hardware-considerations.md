---
title: AD 性能优化中的硬件注意事项
description: AD 性能优化中的硬件注意事项
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 4d1e6c2744cfe0d16b034e6511144bef92a46b2e
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866663"
---
# <a name="hardware-considerations-in-adds-performance-tuning"></a>中的硬件注意事项添加性能优化 

>[!Important]
> 下面概述了优化 Active Directory 工作负荷的服务器硬件的关键建议和注意事项，详细介绍了 Active Directory 域服务文章的[容量规划](https://go.microsoft.com/fwlink/?LinkId=324566)。 为了更好地理解和影响这些建议，强烈建议读者查看[Active Directory 域服务的容量规划](https://go.microsoft.com/fwlink/?LinkId=324566)。

## <a name="avoid-going-to-disk"></a>避免转到磁盘

Active Directory 将数据库缓存为内存允许的数量。 从内存中获取页面比进入物理介质更快，无论介质是主轴还是 SSD。 添加更多内存以最大程度减少磁盘 i/o。

-   Active Directory 最佳实践，建议将足够的 RAM 加载到内存中，并容纳操作系统和其他已安装的应用程序，如防病毒、备份软件、监视等。

    -   有关旧平台的限制，请参阅[运行 Windows Server 2003 或 windows 2000 服务器的域控制器上的 lsass.exe 进程的内存使用情况](https://support.microsoft.com/kb/308356)。

    -   使用 "内存\\长期平均备用缓存生存期（秒） &gt; " 性能计数器。

-   将操作系统、日志和数据库放在不同的卷上。 如果缓存的全部或大部分都可以缓存，一旦缓存准备好并处于稳定状态，这就会变得不太相关，并为存储布局提供更大的灵活性。 在无法缓存整个 DIT 的情况下，将操作系统、日志和数据库拆分为不同卷的重要性变得更加重要。

-   通常，对 DIT 的 i/o 比率大约为 90%，写入 10%。 写入 i/o 卷明显超过 10%-20% 的情况被视为写高。 编写繁重的方案并不大大地受益于 Active Directory 缓存。 为了保证写入目录的数据的事务性持久性，Active Directory 不会执行磁盘写入缓存。 相反，它会在返回操作的成功完成状态之前将所有写入操作提交到磁盘，除非显式请求不执行此操作。 因此，快速磁盘 i/o 对于写入操作的性能 Active Directory 很重要。 下面是可能会提高这些方案性能的硬件建议：

    -   硬件 RAID 控制器

    -   增加托管 DIT 和日志文件的低延迟/高 RPM 磁盘数

    -   在控制器上写入缓存

-   分别检查每个卷的磁盘子系统性能。 大多数 Active Directory 方案主要是基于读取的，因此，承载 DIT 的卷上的统计信息最重要。 但是，不要忽略监视驱动器的其余部分，包括操作系统和日志文件驱动器。 若要确定域控制器是否正确配置为避免存储成为性能瓶颈，请参考存储子系统上有关标准存储建议的部分。 在许多环境中，这一理念旨在确保有足够的空间来适应电涌或负载高峰。 这些阈值为警告阈值，其中，用于适应负载突然或峰值的房间会受到限制，客户端响应会降低。 简而言之，超过这些阈值在短期内（5到15分钟，一天的时间）不正确，但使用这些统计信息运行持续时间的系统不会完全缓存数据库，因此可能会因负载过多而被调查。

    -   Database = =&gt;实例（lsass/NTDSA）\\i/o 数据库读取平均延迟&lt; 15ms

    -   Database = =&gt;实例（lsass/NTDSA）\\i/o 数据库读取次数/秒&lt; 10

    -   Database = =&gt;实例（lsass/NTDSA）\\i/o 日志写入平均延迟&lt; 10ms

    -   Database = =&gt;实例（lsass/NTDSA）\\i/o 日志写入/秒–仅提供信息。

        为了保持数据的一致性，必须将所有更改写入日志。 这里没有正确或不正确的数字，只是度量存储支持的量。

-   在非高峰负载期间规划非核心磁盘 i/o 负载，如备份和防病毒扫描。 此外，请使用支持 Windows Server 2008 中引入的低优先级 i/o 功能的备份和防病毒解决方案，以减少对 Active Directory 的 i/o 需求的竞争。

## <a name="dont-over-tax-the-processors"></a>不要对处理器进行税费

如果处理器没有足够的可用循环，则可能会导致长时间等待，使线程进入处理器执行。 在许多环境中，这一理念旨在确保有足够的空间可适应电涌或负载峰值，从而最大程度地减少对客户端响应的影响。 简而言之，超过以下阈值在短期内（5到15分钟，一天的时间）不正确，但使用这些统计信息运行持续的系统不会提供任何头空间来容纳异常负载，并可以轻松地将其放入cenario. 应调查比阈值更高的时间段的系统开销，以减少处理器负载。

-   有关如何选择处理器的详细信息，请参阅[服务器硬件性能优化](../../hardware/index.md)。

-   添加硬件、优化负载、将客户端定向到其他位置，或从环境中删除负载以减少 CPU 负载。

-   使用 "处理器信息（\_总计）\\% processor 使用率&lt; 60%" 性能计数器。

## <a name="avoid-overloading-the-network-adapter"></a>避免将网络适配器重载

与处理处理器一样，过多的网络适配器使用率会导致出站流量进入网络的长时间等待。 Active Directory 通常具有较小的入站请求和返回到客户端系统的数据量。 发送的数据远远超过了收到的数据。 在许多环境中，这一理念旨在确保有足够的空间来适应电涌或负载高峰。 此阈值为警告阈值，其中，用于适应负载突然或峰值的房间会受到限制，客户端响应会降低。 简而言之，超过这些阈值在短期内（5到15分钟，一天的时间）不正确，但使用这些统计信息运行持续时间的系统就会超出高峰期，应进行调查。

-   有关如何优化网络子系统的详细信息，请参阅对[网络子系统的性能优化](../../../../networking/technologies/network-subsystem/net-sub-performance-top.md)。

-   使用 "比较 NetworkInterface （\*）\\Bytes Sent/Sec with NetworkInterface （\*）\\Current 带宽" 性能计数器。 该比率应小于使用的 60%。

## <a name="see-also"></a>请参阅
- [性能优化 Active Directory 服务器](index.md)
- [LDAP 注意事项](ldap-considerations.md)
- [域控制器的正确放置和站点注意事项](site-definition-considerations.md)
- [ADDS 性能疑难解答](troubleshoot.md) 
- [Active Directory 域服务的容量计划](https://go.microsoft.com/fwlink/?LinkId=324566)