---
title: Linux 虚拟机的注意事项
description: Linux 和 BSD 虚拟机
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cc6aab7825754579269eb05e591ca2a3cf5a561b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869678"
---
# <a name="linux-virtual-machine-considerations"></a>Linux 虚拟机的注意事项

Linux 和 BSD 虚拟机具有与 Windows 虚拟机中的 HYPER-V 相比的其他注意事项。

第一个注意事项是 Integration Services 是否存在，或如果 VM 正在运行仿真硬件上只是与任何一种启发式。 表具有内置或可下载 Integration Services 的 Linux 和 BSD 版本现已推出[Windows 上的 HYPER-V 的支持的 Linux 和 FreeBSD 虚拟机](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows)。 这些页面具有网格的可用的 HYPER-V 功能适用于 Linux 分发的版本中和有关这些功能的说明 （如果适用）。

即使并且来宾正在运行 Integration Services，它可以与旧硬件不展示获得最佳性能的配置。 例如，配置并为访客而不是使用旧版网络适配器使用虚拟以太网适配器。 使用 Windows Server 2016，高级网络如 SR-IOV 也同样可用。

## <a name="linux-network-performance"></a>Linux 网络性能

默认情况下 Linux 启用硬件加速，并默认情况下将卸载。 如果在主机上的 nic 属性中启用 vRSS 和 Linux 来宾能够使用 vRSS 将启用此功能。 可以在 Powershell 中更改此相同的参数与`EnableNetAdapterRSS`命令。

同样，可以使用来宾操作系统的物理 NIC 上启用 VMMQ (虚拟交换机 RSS) 功能**属性** > **配置...**  > **高级**选项卡 > 设置**虚拟交换机 RSS**到**已启用**或启用 VMMQ 中使用以下 Powershell:

```PowerShell
 Set-VMNetworkAdapter -VMName **$VMName** -VmmqEnabled $True
 ```

在来宾中其他 TCP 优化可以通过增加限制。 为了获得最佳性能将工作负荷分布到多个 Cpu 和具有深度的工作负荷将产生最佳吞吐量，因为虚拟化工作负荷必须比"裸机"更高的延迟的。

已在网络基准测试中非常有用的一些示例优化参数包括：

```PowerShell
net.core.netdev_max_backlog = 30000
net.core.rmem_max = 67108864
net.core.wmem_max = 67108864
net.ipv4.tcp_wmem = 4096 12582912 33554432
net.ipv4.tcp_rmem = 4096 12582912 33554432
net.ipv4.tcp_max_syn_backlog = 80960
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_tw_reuse = 1
net.ipv4.ip_local_port_range = 10240 65535
net.ipv4.tcp_abort_on_overflow = 1
```

用于网络 microbenchmarks 的有用工具是 ntttcp，可在 Linux 和 Windows 上。 Linux 版本是开放源代码并且可从[github.com 上为 linux ntttcp](https://github.com/Microsoft/ntttcp-for-linux)。 Windows 版本可在[下载中心](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769)。 优化工作负荷时，最好根据需要使用尽可能多的流以获得最佳吞吐量。 使用模型流量 ntttcp`-P`参数设置使用的并行连接数。

## <a name="linux-storage-performance"></a>Linux 存储性能

上列出了一些最佳做法，如图所示[HYPER-V 上运行 Linux 的最佳实践](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/best-practices-for-running-linux-on-hyper-v)。 Linux 内核具有不同的 I/O 计划程序进行重新排序具有不同的算法的请求。 NOOP 是传递由虚拟机监控程序的计划决定的先入先出队列。 建议 NOOP 计划程序运行时要使用 Linux 虚拟机上的 HYPER-V。 若要更改的特定设备时，在启动加载程序的配置中的计划程序 (/ etc/grub.conf，例如)，添加`elevator=noop`到内核参数以及然后重新启动。

类似于网络，使用存储的 Linux 来宾性能受益充分利用多个队列具有足够的深度，以使主机处于繁忙状态。 可能最适合于 fio 基准测试工具与 libaio 引擎 Microbenchmarking 存储性能。

## <a name="see-also"></a>请参阅

-   [HYPER-V 术语](terminology.md)

-   [HYPER-V 体系结构](architecture.md)

-   [HYPER-V 服务器配置](configuration.md)

-   [HYPER-V 处理器性能](processor-performance.md)

-   [HYPER-V 内存性能](memory-performance.md)

-   [HYPER-V 存储 I/O 性能](storage-io-performance.md)

-   [HYPER-V 网络 I/O 性能](network-io-performance.md)

-   [虚拟化环境中检测瓶颈](detecting-virtualized-environment-bottlenecks.md)
