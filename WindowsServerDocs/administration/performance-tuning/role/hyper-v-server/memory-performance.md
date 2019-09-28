---
title: Hyper-v 内存性能
description: 性能优化 Hyper-v 中的内存注意事项
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 88827176e8a425e9bf68fdc170a4a114346562f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385095"
---
# <a name="hyper-v-memory-performance"></a>Hyper-v 内存性能


虚拟机监控程序虚拟化了来宾物理内存，以将虚拟机彼此隔离，并为每个来宾操作系统提供一个连续的、从零开始的内存空间，就像在非虚拟化系统中一样。

## <a name="correct-memory-sizing-for-child-partitions"></a>为子分区更正内存大小

应按照通常为物理计算机上的服务器应用程序执行的操作来调整虚拟机内存的大小。 必须调整它的大小以合理地处理一般和高峰时间的预期负载，因为内存不足可能会显著增加响应时间和 CPU 或 i/o 使用率。

可以启用动态内存允许 Windows 动态调整虚拟机内存的大小。 在动态内存中，如果虚拟机中的应用程序遇到导致大量内存分配的问题，则可以增加虚拟机的页面文件大小，以确保在动态内存响应内存压力时暂时获得支持。

有关动态内存的详细信息，请参阅[hyper-v 动态内存概述]( https://go.microsoft.com/fwlink/?linkid=834434)和[Hyper-v 动态内存配置指南](https://go.microsoft.com/fwlink/?linkid=834435)。

在子分区中运行 Windows 时，可以使用子分区中的以下性能计数器来确定子分区是否遇到内存压力，并可能更好地提高虚拟机内存大小。

| 性能计数器                                                         | 建议的阈值                                                                                                                                                           |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 内存–备用缓存保留字节数                                        | 在具有 1 GB 的系统上，备用缓存保留字节和空闲和零页列表字节的总和应为 200 MB 或更大，300并且系统上有 2 GB 或更多的可视 RAM。 |
| 内存–可用 & 零页列表字节                                        | 在具有 1 GB 的系统上，备用缓存保留字节和空闲和零页列表字节的总和应为 200 MB 或更大，300并且系统上有 2 GB 或更多的可视 RAM。 |
| 内存–每秒输入的页数                                                    | 1小时内的平均时间小于10。                                                                                                                                       | 

## <a name="correct-memory-sizing-for-root-partition"></a>为根分区更正内存大小

根分区必须具有足够的内存来提供服务（例如 i/o 虚拟化、虚拟机快照）和管理以支持子分区。

Windows Server 2016 中的 hyper-v 监视根分区的管理操作系统的运行时运行状况以确定可以安全地分配给子分区的内存量，同时仍可确保根分区的高性能和可靠性。

## <a name="see-also"></a>请参阅

-   [Hyper-V 术语](terminology.md)

-   [Hyper-V 体系结构](architecture.md)

-   [Hyper-V 服务器配置](configuration.md)

-   [Hyper-V 处理器性能](processor-performance.md)

-   [Hyper-V 存储 I/O 性能](storage-io-performance.md)

-   [Hyper-V 网络 I/O 性能](network-io-performance.md)

-   [检测虚拟化环境中的瓶颈](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虚拟机](linux-virtual-machine-considerations.md)
