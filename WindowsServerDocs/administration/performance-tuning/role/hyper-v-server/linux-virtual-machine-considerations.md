---
title: Linux 虚拟机注意事项
description: Linux 和 BSD 虚拟机
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5668629e7eded214525561d30fec496a4e91b8dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385067"
---
# <a name="linux-virtual-machine-considerations"></a>Linux 虚拟机注意事项

与 Hyper-v 中的 Windows 虚拟机相比，Linux 和 BSD 虚拟机有其他注意事项。

首先要考虑 Integration Services 是存在还是仅在不悟道的模拟硬件上运行 VM。 [Windows 上的 Hyper-v 支持的 linux 和 FreeBSD 虚拟机](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows)中提供了包含内置或可下载 Integration Services 的 LINUX 和 BSD 版本表。 这些页面提供适用于 Linux 分发版的可用 Hyper-v 功能网格，并在适用的情况下提供有关这些功能的说明。

即使来宾 Integration Services 运行，它也可以配置旧版硬件，而不会表现出最佳性能。 例如，为来宾配置和使用虚拟以太网适配器，而不是使用旧版网络适配器。 对于 Windows Server 2016，还提供了高级网络，如 SR-IOV。

## <a name="linux-network-performance"></a>Linux 网络性能

默认情况下，Linux 允许硬件加速和卸载。 如果在主机上的 NIC 的属性中启用了 vRSS 并且 Linux 来宾可以使用 vRSS，则将启用该功能。 在 Powershell 中，可以通过 `EnableNetAdapterRSS` 命令更改同一个参数。

同样，可以在来宾**属性**使用的物理 NIC 上启用 VMMQ （虚拟交换机 RSS）功能， > **配置 ...** @no__t 3 "**高级**" 选项卡 > 将**虚拟交换机 RSS**设置为 "**启用**"，或使用以下命令在 Powershell 中启用 VMMQ：

```PowerShell
 Set-VMNetworkAdapter -VMName **$VMName** -VmmqEnabled $True
 ```

在来宾中，可以通过增加限制来执行其他 TCP 优化。 为了获得跨多个 Cpu 的最佳性能分配工作负荷，并且具有深度工作负载，可以获得最佳吞吐量，因为虚拟化工作负荷的延迟高于 "裸机"。

一些在网络基准中有用的优化参数示例包括：

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

用于网络 microbenchmarks 的有用工具是 ntttcp，可在 Linux 和 Windows 上使用。 Linux 版本是开放源代码，可[在 github.com 上的 ntttcp](https://github.com/Microsoft/ntttcp-for-linux)中使用。 可以在[下载中心](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769)找到 Windows 版本。 优化工作负荷时，最好使用尽可能多的流来获得最佳吞吐量。 使用 ntttcp 来为流量建模，`-P` 参数设置使用的并行连接数。

## <a name="linux-storage-performance"></a>Linux 存储性能

以下是一些最佳实践，如在[hyper-v 上运行 Linux 的最佳实践](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/best-practices-for-running-linux-on-hyper-v)。 Linux 内核具有不同的 i/o 计划程序，用不同的算法对请求重新排序。 NOOP 是一个先进先出队列，该队列通过虚拟机监控程序做出的计划决策。 在 Hyper-v 上运行 Linux 虚拟机时，建议使用 NOOP 作为计划程序。 若要更改特定设备的计划程序，请在启动加载程序的配置（例如/etc/grub.conf）中，将 `elevator=noop` 添加到内核参数，然后重新启动。

与网络类似，Linux 来宾的存储性能从具有足够深度的多个队列中获益最大，使主机保持繁忙状态。 Microbenchmarking 存储性能可能适用于 fio 基准工具与 libaio 引擎。

## <a name="see-also"></a>请参阅

-   [Hyper-V 术语](terminology.md)

-   [Hyper-V 体系结构](architecture.md)

-   [Hyper-V 服务器配置](configuration.md)

-   [Hyper-V 处理器性能](processor-performance.md)

-   [Hyper-V 内存性能](memory-performance.md)

-   [Hyper-V 存储 I/O 性能](storage-io-performance.md)

-   [Hyper-V 网络 I/O 性能](network-io-performance.md)

-   [检测虚拟化环境中的瓶颈](detecting-virtualized-environment-bottlenecks.md)
