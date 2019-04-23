---
title: 缓存和内存管理器性能问题进行故障排除
description: 缓存和内存管理器在 Windows Server 16 上的性能问题进行故障排除
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 66c7e2a6b264a837c65df927b271fadd2672fa24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835788"
---
# <a name="troubleshoot-cache-and-memory-manager-performance-issues"></a>缓存和内存管理器性能问题进行故障排除

在 Windows Server 2012 之前两个主要的潜在问题引起的系统文件缓存增长，直到某些工作负荷下几乎耗尽可用内存的了。 当此种情况下的会导致系统缓慢，可以确定是否在服务器遇到以下问题之一。


## <a name="counters-to-monitor"></a>要监视的计数器

-   内存\\长期平均待机缓存生存期 (s) &lt; 1800 秒

-   内存\\可用兆字节数不足

-   内存\\系统缓存驻留字节数

如果内存\\Available Mbytes 是低和在同一时间内存\\System Cache Resident Bytes 使用的物理内存的重要部分，则可以使用[RAMMAP](https://technet.microsoft.com/sysinternals/ff700229.aspx)找出正在使用哪些缓存有关。

## <a name="system-file-cache-contains-ntfs-metafile-data-structures"></a>系统文件缓存包含 NTFS 图元文件的数据结构


下图中所示，将由极大量活动图元文件中的页数 RAMMAP 输出，指示此问题。 此问题可能会观察到繁忙的服务器上具有数百万个文件访问，从而导致缓存未从缓存释放 NTFS 图元文件数据。

![rammap 视图](../../media/perftune-guide-rammap.png)

问题用于缓解*DynCache*工具。 在 Windows Server 2012 及更高，体系结构进行了重新设计并应不再存在此问题。

## <a name="system-file-cache-contains-memory-mapped-files"></a>系统文件缓存包含内存映射文件


极大量的 RAMMAP 输出中的活动映射文件页的指示了此问题。 这通常表示服务器上的某些应用程序正在打开大量使用的大型文件[CreateFile](https://msdn.microsoft.com/library/windows/desktop/aa363858.aspx)使用文件 API\_标志\_随机\_访问标志设置。

在知识库文章中详细介绍了此问题[2549369](https://support.microsoft.com/default.aspx?scid=kb;en-US;2549369)。 文件\_标志\_随机\_访问标志是一个提示用于缓存管理器以保留文件的映射的视图在内存中尽可能长时间，直到内存管理器不会指示内存不足的情况）。 同时，此标志指示缓存管理器禁用预提取的文件数据。

这种情况下已减轻了某种程度上工作集修整改进在 Windows Server 2012 及更高，但该问题本身需要主要由不使用文件的应用程序供应商\_标志\_随机\_访问权限。 应用程序供应商的替代解决方案可能是访问文件时使用较低内存优先级。 这可以使用[SetThreadInformation](https://msdn.microsoft.com/library/windows/desktop/hh448390.aspx) API。 优先级低的内存访问的页面将从工作集中更主动的方式删除。

缓存管理器中，启动 Windows Server 2016 中进一步减轻这通过忽略 FILE_FLAG_RANDOM_ACCESS 决定修整，因此没有 （缓存管理器仍遵循这 FILE_FLAG_RANDOM_ACCESS 标志的情况下打开的任何其他文件一样处理时标记为禁用预提取的文件数据）。 如果有大量的文件打开使用此标志和真正随机的方式访问，仍然可能导致系统缓存膨胀。 强烈建议不由应用程序使用 FILE_FLAG_RANDOM_ACCESS。
