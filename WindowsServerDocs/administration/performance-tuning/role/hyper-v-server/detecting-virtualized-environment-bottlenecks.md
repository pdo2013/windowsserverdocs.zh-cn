---
title: 虚拟化环境中检测瓶颈
description: 了解如何检测和解决潜在的 hyper-v 的性能瓶颈
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cdad5f0cc3b0e49ae46e975e3acc2c48a18e5f70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867528"
---
# <a name="detecting-bottlenecks-in-a-virtualized-environment"></a>虚拟化环境中检测瓶颈

本部分将帮助您要使用性能监视器监视的内容以及如何识别其中问题可能是因为您将希望在主机或虚拟机的一些未执行的一些提示。

## <a name="processor-bottlenecks"></a>处理器瓶颈

下面是一些可能会导致处理器瓶颈的常见方案：

-   加载一个或多个逻辑处理器

-   加载一个或多个虚拟处理器

可以使用从主机的以下性能计数器：

-   逻辑处理器使用率-\\逻辑处理器的 HYPER-V 虚拟机监控程序 (\*)\\%总运行时间

-   虚拟处理器使用率-\\的 HYPER-V 虚拟机监控程序虚拟处理器 (\*)\\%总运行时间

-   根虚拟处理器使用率-\\的 HYPER-V 虚拟机监控程序根虚拟处理器 (\*)\\%总运行时间

如果**HYPER-V 虚拟机监控程序逻辑处理器 (\_总)\\%总运行时**计数器超过 90%，重载主机。 应添加更多处理能力或将一些虚拟机移动到另一台主机。

如果**HYPER-V 虚拟机监控程序虚拟 Processor(VM Name:VP x)\\%总运行时**计数器超过 90%的所有虚拟处理器，则应执行以下操作：

-   验证主机未重载

-   查找工作负荷可以利用多个虚拟处理器

-   将多个虚拟处理器分配给虚拟机

如果**HYPER-V 虚拟机监控程序虚拟 Processor(VM Name:VP x)\\%总运行时**计数器超过 90%的一些，但不是全部的虚拟处理器，则应执行以下操作：

-   如果你的工作负荷是收到网络密集型，应考虑使用 vRSS。

-   如果虚拟机未运行 Windows Server 2012 R2，则应添加更多的网络适配器。

-   如果你的工作负荷是存储密集型，应启用虚拟 NUMA 并添加更多的虚拟磁盘。

如果**HYPER-V 虚拟机监控程序根虚拟处理器 (根副总裁 x)\\%总运行时**计数器已超过 90%的部分，而不是所有虚拟处理器和**处理器 (x)\\%Interrupt Time 和处理器 (x)\\%DPC Time**计数器大约相加的值**根虚拟 Processor(Root VP x)\\%总运行时**计数器，应确保上启用 VMQ网络适配器。

## <a name="memory-bottlenecks"></a>内存瓶颈

下面是一些可能导致内存瓶颈的常见方案：

-   主机没有响应。

-   无法启动虚拟机。

-   虚拟机运行内存不足。

可以使用从主机的以下性能计数器：

-   内存\\可用兆字节数

-   HYPER-V 动态内存的均衡器 (\*)\\可用内存

可以使用从虚拟机的以下性能计数器：

-   内存\\可用兆字节数

如果**内存\\Available Mbytes**并**HYPER-V Dynamic Memory Balancer (\*)\\可用内存**计数器较低的主机上，应停止非基本服务，并将一个或多个虚拟机迁移到另一台主机。

如果**内存\\Available Mbytes**计数器较低的虚拟机中，应将更多的内存分配给虚拟机。 如果使用动态内存，则应增加最大内存设置。

## <a name="network-bottlenecks"></a>网络瓶颈

下面是一些可能导致网络瓶颈的常见方案：

-   主机是网络绑定。

-   虚拟机是网络绑定。

可以使用从主机的以下性能计数器：

-   网络接口 (*网络适配器名称*)\\字节数/秒

可以使用从虚拟机的以下性能计数器：

-   HYPER-V 虚拟网络适配器 (*虚拟机名称名称&lt;GUID&gt;*)\\字节数/秒

如果**物理 NIC 字节数/秒**计数器是大于或等于容量的 90%时，应添加其他网络适配器、 虚拟机迁移到另一台主机和配置网络 QoS。

如果**HYPER-V 虚拟网络适配器字节数/秒**计数器是大于或等于 250 MBps，你应添加虚拟机中的其他成组的网络适配器启用 vRSS，并使用 SR-IOV。

如果您的工作负载不能满足其网络延迟，启用 SR-IOV 呈现到虚拟机的物理网络适配器资源。

## <a name="storage-bottlenecks"></a>存储瓶颈

下面是一些可能导致存储瓶颈的常见方案：

-   操作主机和虚拟机速度缓慢或超时。

-   虚拟机是缓慢。

可以使用从主机的以下性能计数器：

-   物理磁盘 (*磁盘号*)\\avg.disk sec/Read

-   物理磁盘 (*磁盘号*)\\avg.disk sec/Write

-   物理磁盘 (*磁盘号*)\\平均磁盘读取队列长度

-   物理磁盘 (*磁盘号*)\\平均磁盘写入队列长度

如果始终大于 50 毫秒的延迟，应执行以下操作：

-   虚拟机分散到其他存储

-   请考虑购买更快的存储

-   请考虑分层存储空间，Windows Server 2012 R2 中引入

-   请考虑使用存储 QoS，在 Windows Server 2012 R2 中引入

-   使用 VHDX

## <a name="see-also"></a>请参阅

-   [HYPER-V 术语](terminology.md)

-   [HYPER-V 体系结构](architecture.md)

-   [HYPER-V 服务器配置](configuration.md)

-   [HYPER-V 处理器性能](processor-performance.md)

-   [HYPER-V 内存性能](memory-performance.md)

-   [HYPER-V 存储 I/O 性能](storage-io-performance.md)

-   [HYPER-V 网络 I/O 性能](network-io-performance.md)

-   [Linux 虚拟机](linux-virtual-machine-considerations.md)
