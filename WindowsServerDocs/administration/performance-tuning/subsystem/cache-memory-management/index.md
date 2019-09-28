---
title: 针对缓存和内存管理器子系统的性能优化
description: 针对缓存和内存管理器子系统的性能优化
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: c914768378303b8a36cb2e3ec468e853296249a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370017"
---
# <a name="performance-tuning-cache-and-memory-manager"></a>性能优化缓存和内存管理器

默认情况下，Windows 缓存从磁盘读取的和写入到磁盘的文件数据。 这意味着读取操作从系统内存中的某个区域（称为系统文件缓存）读取文件数据，而不是从物理磁盘读取。 相应地，写入操作将文件数据写入系统文件缓存，而不是写入磁盘。这类缓存称为回写缓存。 缓存按文件对象进行管理。 缓存在缓存管理器的引导下进行。只要 Windows 运行，该管理器就会持续运行。

系统文件缓存中的文件数据按操作系统确定的时间间隔写入磁盘。 刷新的页面保存在系统缓存工作集中（此时 FILE\_FLAG\_RANDOM\_ACCESS 已设置，文件句柄未关闭），或者保存在备用列表中，成为可用内存的一部分。

延迟将数据写入文件，将其保存在缓存中，直至缓存刷新，这一策略称为惰性写入，由缓存管理器按确定的时间间隔触发。 刷新文件数据块的时间部分取决于该块已在缓存中存储了多长时间，以及自上次在读取操作中访问数据以来已过去多长时间。 这样可确保频繁读取的文件数据在最大时间范围内可以在系统文件缓存中供访问。

下图演示了该文件数据缓存过程：

![文件数据缓存](../../media/perftune-guide-file-data-caching.png)

如上图中的实线箭头所示，在文件读取操作过程中，当缓存管理器首次提出请求时，系统将一个 256 KB 的数据区域读取到系统地址空间一个 256 KB 的缓存槽中。 然后，一个用户模式进程将该槽中的数据复制到它自己的地址空间。 该进程在完成其数据访问操作后，会将更改的数据写回到系统缓存中的同一槽，如进程地址空间和系统缓存之间的虚线箭头所示。 缓存管理器在确定在特定的时间内不再需要该数据后，就会将更改的数据写回到磁盘上的文件中，如系统缓存和磁盘之间的虚线箭头所示。

**本部分内容：**

-   [缓存和内存管理器可能存在的性能问题](troubleshoot.md)

-   [Windows Server 2016 中的缓存和内存管理器改进](improvements-in-2016.md)
