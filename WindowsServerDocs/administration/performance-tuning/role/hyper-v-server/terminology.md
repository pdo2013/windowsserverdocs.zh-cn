---
title: Hyper-v 术语
description: Hyper-v 术语在 Hyper-v 性能优化中很有用
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: d18557a205f8366631becb65b7460c07757db3d5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866525"
---
# <a name="hyper-v-terminology"></a>Hyper-v 术语
本部分概述了本性能优化主题中使用的虚拟机技术特定的关键术语：

| 术语        | 定义           |
| ------------- |:------------|
|*子分区* | 根分区创建的任何虚拟机。|
|*设备虚拟化* | 一种允许在多个使用者之间抽象并共享硬件资源的机制。|
|*模拟设备*|模拟实际物理硬件设备的虚拟化设备，以便来宾可以使用该硬件设备的典型驱动程序。|
|*悟道*|对来宾操作系统的优化，以使其能够识别虚拟机环境并为虚拟机优化其行为。|
|*来宾*|在分区中运行的软件。 它可以是功能完备的操作系统或小型的专用内核。 虚拟机监控程序是不可知的。|
|*hypervisor*|位于硬件和一个或多个操作系统下的软件层。 它的主要工作是提供称为分区的隔离执行环境。 每个分区都有其自己的虚拟化硬件资源集（中心处理单元或 CPU、内存和设备）。 虚拟机监控程序控制并裁定对基础硬件的访问。|
|*逻辑处理器*| 处理一个执行线程的处理单元（说明流）。 每个处理器核心可以有一个或多个逻辑处理器，每个处理器套接字可有一个或多个内核。|
| *传递磁盘访问*|作为来宾内的虚拟磁盘的整个物理磁盘的表示形式。 数据和命令会传递到物理磁盘（通过根分区的本机存储堆栈），而不会对虚拟堆栈进行任何干预。|
|*根分区*|首先创建并拥有虚拟机监控程序不包含的所有资源（包括大多数设备和系统内存）的根分区。 根分区承载虚拟化堆栈，并创建并管理子分区。|
|*Hyper-v 专用设备*|不带物理硬件模拟的虚拟化设备，因此来宾可能需要驱动程序（虚拟化服务客户端）到该特定于 Hyper-v 的设备。 驱动程序可以使用虚拟机总线（VMBus）与根分区中的虚拟化设备软件进行通信。|
|*虚拟机*|由软件仿真创建的虚拟计算机，与真实计算机具有相同的特征。|
| *虚拟网络交换机*|（也称为虚拟交换机）物理网络交换机的虚拟版本。 可将虚拟网络配置为向一台或多台虚拟机提供对本地或外部网络资源的访问。|
|*虚拟处理器*|计划在逻辑处理器上运行的处理器的虚拟抽象。 虚拟机可以有一个或多个虚拟处理器。|
|*虚拟化服务客户端（VSC）*|来宾加载以使用资源或服务的软件模块。 对于 i/o 设备，虚拟化服务客户端可以是操作系统内核加载的设备驱动程序。|
| *虚拟化服务提供程序（VSP）*|  根分区中的虚拟化堆栈公开的提供程序，可向子分区提供 i/o 等资源或服务。|
| *虚拟化堆栈*|根分区中的软件组件的集合，它们协同工作以支持虚拟机。 虚拟化堆栈适用于并且位于虚拟机监控程序之上。 它还提供管理功能。|
|*VMBus*|基于通道的通信机制，用于在具有多个活动虚拟化分区的系统上进行分区间通信和设备枚举。 VMBus 随 Hyper-V 集成服务一起安装。|

## <a name="see-also"></a>请参阅

-   [Hyper-V 体系结构](architecture.md)

-   [Hyper-V 服务器配置](configuration.md)

-   [Hyper-V 处理器性能](processor-performance.md)

-   [Hyper-V 内存性能](memory-performance.md)

-   [Hyper-V 存储 I/O 性能](storage-io-performance.md)

-   [Hyper-V 网络 I/O 性能](network-io-performance.md)

-   [检测虚拟化环境中的瓶颈](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虚拟机](linux-virtual-machine-considerations.md)
