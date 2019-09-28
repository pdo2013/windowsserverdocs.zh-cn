---
title: Hyper-V 体系结构
description: 用于性能优化的 hyper-v 体系结构 condsiderations
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 0feb2977791dd181907c381e4898924ff51c2bc5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383487"
---
# <a name="hyper-v-architecture"></a>Hyper-V 体系结构

Hyper-v 的功能是基于类型1的虚拟机监控程序体系结构。 虚拟机监控程序虚拟化处理器和内存，并为根分区中的虚拟化堆栈提供管理子分区（虚拟机）的机制，并向虚拟机公开 i/o 设备等服务。

根分区拥有并可以直接访问物理 i/o 设备。 根分区中的虚拟化堆栈为虚拟机、管理 Api 和虚拟化 i/o 设备提供了内存管理器。 它还实现了模拟设备（如集成设备电子设备（IDE）磁盘控制器和 PS/2 输入设备端口），并支持特定于 Hyper-v 的合成设备，以提高性能并降低开销。

![基于 hyper-v 虚拟机监控程序的体系结构](../../media/perftune-guide-hyperv-arch.png)

Hyper-v 特定 i/o 体系结构由子分区中根分区和虚拟化服务客户端（Vsc）中的虚拟化服务提供程序（Vsp）组成。 每个服务都是通过 VMBus 作为设备公开的，它充当 i/o 总线，并在使用诸如共享内存等机制的虚拟机之间实现高性能的通信。 来宾操作系统的即插即用管理器枚举这些设备，包括 VMBus，并加载适当的设备驱动程序（虚拟服务客户端）。 除 i/o 以外的服务还通过此体系结构公开。

从 Windows Server 2008 开始，操作系统功能自旋在虚拟机中运行时优化其行为。 优点包括降低内存虚拟化的成本、提高多核的可伸缩性并降低来宾操作系统的后台 CPU 使用率。

以下各部分提供了在运行 Hyper-v 角色的服务器上提高性能的最佳做法。

## <a name="see-also"></a>请参阅

-   [Hyper-V 术语](terminology.md)

-   [Hyper-V 服务器配置](configuration.md)

-   [Hyper-V 处理器性能](processor-performance.md)

-   [Hyper-V 内存性能](memory-performance.md)

-   [Hyper-V 存储 I/O 性能](storage-io-performance.md)

-   [Hyper-V 网络 I/O 性能](network-io-performance.md)

-   [检测虚拟化环境中的瓶颈](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虚拟机](linux-virtual-machine-considerations.md)
