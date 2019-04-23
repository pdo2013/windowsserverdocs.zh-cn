---
title: Hyper-V 体系结构
description: Hyper-v 体系结构 condsiderations 进行性能优化
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fcc87b04698a44e115c8f49150fe33443f8e6a88
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890278"
---
# <a name="hyper-v-architecture"></a>Hyper-V 体系结构

HYPER-V 功能的类型 1 基于虚拟机监控程序的体系结构。 虚拟机监控程序虚拟处理器和内存，并提供了根分区管理子分区 （虚拟机） 和公开服务，例如 I/O 设备到虚拟机的虚拟化堆栈的机制。

根分区拥有并具有直接访问物理 I/O 设备。 根分区中的虚拟化堆栈的虚拟机、 管理 Api，以及虚拟化的 I/O 设备提供的内存管理器。 它还实现模拟的设备，如集成的设备电子学 (IDE) 磁盘控制器和 PS/2 输入的设备端口，以及它支持 Hyper V 特定合成设备以提高性能和减少的负载。

![hyper-v 基于虚拟机监控程序的体系结构](../../media/perftune-guide-hyperv-arch.png)

Hyper V 特定 I/O 体系结构包括虚拟化服务提供商 (Vsp) 中的根分区和虚拟化服务客户端 (Vsc) 子分区中。 通过 VMBus，可充当 I/O 总线并支持使用机制，例如共享内存的虚拟机之间的高性能通信情况下，每个服务公开为设备。 来宾操作系统的插管理器枚举包括 VMBus，这些设备，并加载相应的设备驱动程序 （虚拟服务客户端）。 通过此体系结构还公开 I/O 以外的服务。

从 Windows Server 2008，操作系统功能启发方法来优化其行为，在虚拟机中运行时开始。 优势包括减少内存虚拟化，改进多核可伸缩性，以及减少的后台 CPU 使用率的来宾操作系统的成本。

以下各节建议生成运行的 HYPER-V 角色的服务器上提高的性能的最佳做法。

## <a name="see-also"></a>请参阅

-   [HYPER-V 术语](terminology.md)

-   [HYPER-V 服务器配置](configuration.md)

-   [HYPER-V 处理器性能](processor-performance.md)

-   [HYPER-V 内存性能](memory-performance.md)

-   [HYPER-V 存储 I/O 性能](storage-io-performance.md)

-   [HYPER-V 网络 I/O 性能](network-io-performance.md)

-   [虚拟化环境中检测瓶颈](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虚拟机](linux-virtual-machine-considerations.md)
