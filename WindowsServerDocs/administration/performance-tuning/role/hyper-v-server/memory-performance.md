---
title: HYPER-V 内存性能
description: 中的性能优化 HYPER-V 内存注意事项
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 63a1b654b8ac52725cc5dd87c8b245f9dfaf40f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848068"
---
# <a name="hyper-v-memory-performance"></a>HYPER-V 内存性能


虚拟机监控程序虚拟化来宾物理内存，以将虚拟机相互隔离并为每个来宾操作系统，提供从零开始的连续内存空间，只是因为非虚拟化系统上。

## <a name="correct-memory-sizing-for-child-partitions"></a>正确的内存大小调整为子分区的

像通常对物理计算机上的服务器应用程序，应该调整虚拟机内存的大小。 必须调整其合理地处理在普通的预期的负载的大小和峰值时间，因为没有足够的内存可显著提高响应时间和 CPU 或 I/O 使用率。

您可以启用动态内存，以允许 Windows 动态调整虚拟机内存的大小。 使用动态内存，如果虚拟机中的应用程序时遇到问题，从而大型突然内存分配，可以增加虚拟机，以确保临时备份，而动态内存的内存压力响应的页面文件大小。

有关动态内存的详细信息，请参阅[HYPER-V 动态内存概述]( https://go.microsoft.com/fwlink/?linkid=834434)并[HYPER-V 动态内存配置指南](https://go.microsoft.com/fwlink/?linkid=834435)。

在运行时 Windows 子分区，可以使用子分区中的以下性能计数器来确定是否子分区遇到内存不足的情况，并且可能具有更高版本的虚拟机内存大小更好地执行。

| 性能计数器                                                         | 建议的阈值                                                                                                                                                           |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 内存-备用缓存保留字节数                                        | 备用缓存保留字节数和免费版和零页列表字节数的总和应为 200 MB 或更多具有 1 GB，和 300 MB 或更多具有 2 GB 或更大的可见 RAM 的系统上的系统上。 |
| 内存-可用和零页列表字节                                        | 备用缓存保留字节数和免费版和零页列表字节数的总和应为 200 MB 或更多具有 1 GB，和 300 MB 或更多具有 2 GB 或更大的可见 RAM 的系统上的系统上。 |
| 内存-Pages Input/Sec                                                    | 1 小时内的平均值为小于 10。                                                                                                                                       | 

## <a name="correct-memory-sizing-for-root-partition"></a>正确的内存大小调整为根分区的

根分区必须具有足够的内存来提供服务，例如 I/O 虚拟化、 虚拟机快照发布和管理支持子分区。

Windows Server 2016 中的 HYPER-V 监视根分区的管理操作系统来确定内存量可以安全地分配给子分区，同时仍能确保根分区的高性能和可靠性的运行时运行状况。

## <a name="see-also"></a>请参阅

-   [HYPER-V 术语](terminology.md)

-   [HYPER-V 体系结构](architecture.md)

-   [HYPER-V 服务器配置](configuration.md)

-   [HYPER-V 处理器性能](processor-performance.md)

-   [HYPER-V 存储 I/O 性能](storage-io-performance.md)

-   [HYPER-V 网络 I/O 性能](network-io-performance.md)

-   [虚拟化环境中检测瓶颈](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虚拟机](linux-virtual-machine-considerations.md)
