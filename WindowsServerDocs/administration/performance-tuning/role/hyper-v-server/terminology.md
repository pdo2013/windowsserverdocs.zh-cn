---
title: HYPER-V 术语
description: Hyper-v 术语中的 HYPER-V 性能优化有用
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bc970633ff24827207eb3a27e282656f2486a6eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841138"
---
# <a name="hyper-v-terminology"></a>Hyper V 术语
本部分总结了特定于此性能优化主题通篇使用的虚拟机技术的关键术语：

| 术语        | 定义           |
| ------------- |:------------|
|*子分区* | 创建根分区的任何虚拟机。|
|*设备虚拟化* | 一种机制，可让硬件资源进行抽象和在多个使用者之间共享。|
|*模拟的设备*|一种虚拟化的设备，以模拟实际的物理硬件设备，以便来宾可以使用该硬件设备的典型驱动程序。|
|*enlightenment*|一种优化到来宾操作系统，使其虚拟机环境了解和调整其行为的虚拟机。|
|*guest*|在分区中运行的软件。 它可以是一个全功能的操作系统或小型的专用内核。 虚拟机监控程序与来宾无关。|
|*hypervisor*|一个位于在硬件上方和下方一个或多个操作系统的软件层。 它的主要工作是提供称为分区的隔离执行环境。 每个分区都有其自己的虚拟化的硬件资源 （中央处理单元或 CPU、 内存和设备） 集。 虚拟机监控程序控制并裁定对基础硬件的访问。|
|*逻辑处理器*| 处理一个线程执行 （指令流） 处理单元。 可以每个处理器核心的一个或多个逻辑处理器和每个处理器套接字的一个或多个核心数。|
| *传递磁盘访问*|表示形式的整个的物理磁盘在来宾中的虚拟磁盘。 数据和命令都被传递到处理任何干预的物理磁盘 （通过根分区的本机存储堆栈） 通过虚拟堆栈。|
|*根分区*|根分区的第一次创建并拥有虚拟机监控程序不，不包括大多数设备和系统内存的所有资源。 根分区承载虚拟化堆栈并创建和管理子分区。|
|*Hyper V 特定设备*|虚拟化的设备没有物理硬件支持模拟，因此，来宾可能需要该 Hyper V 特定设备驱动程序 （虚拟化服务客户端）。 该驱动程序可以使用虚拟机总线 (VMBus) 与在根分区中的虚拟化的设备软件进行通信。|
|*虚拟机*|已通过软件模拟并具有相同的特性的实际计算机的虚拟计算机。|
| *虚拟网络交换机*|（也称为虚拟交换机）物理网络交换机的虚拟版本。 可将虚拟网络配置为向一台或多台虚拟机提供对本地或外部网络资源的访问。|
|*虚拟处理器*|计划在逻辑处理器上运行的处理器的虚拟抽象。 虚拟机可以有一个或多个虚拟处理器。|
|*虚拟化服务客户端 (VSC)*|来宾加载要使用的资源或服务的软件模块。 对于 I/O 的设备，虚拟化服务客户端可以是操作系统内核加载的设备驱动程序。|
| *虚拟化服务提供商 (VSP)*|  提供资源或服务，例如 I/O 子分区到提供程序公开的根分区中的虚拟化堆栈。|
| *虚拟化堆栈*|根分区中的软件组件，它们协同工作以支持虚拟机的集合。 虚拟化堆栈处理，并且位于虚拟机监控程序的上方。 它还提供管理功能。|
|*VMBus*|内分区的通信和设备枚举，具有多个活动的虚拟化分区的系统上使用基于通道的通信机制。 VMBus 随 Hyper-V 集成服务一起安装。|

## <a name="see-also"></a>请参阅

-   [HYPER-V 体系结构](architecture.md)

-   [HYPER-V 服务器配置](configuration.md)

-   [HYPER-V 处理器性能](processor-performance.md)

-   [HYPER-V 内存性能](memory-performance.md)

-   [HYPER-V 存储 I/O 性能](storage-io-performance.md)

-   [HYPER-V 网络 I/O 性能](network-io-performance.md)

-   [虚拟化环境中检测瓶颈](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虚拟机](linux-virtual-machine-considerations.md)
