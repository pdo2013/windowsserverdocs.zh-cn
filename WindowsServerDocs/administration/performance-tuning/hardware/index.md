---
title: 服务器硬件性能注意事项
description: Windows Server 2016 的服务器硬件性能注意事项
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: phstee
author: phstee
ms.date: 01/08/2018
ms.openlocfilehash: f9c7f0e589980f7d985f165e318667ebe2e5d5c5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866745"
---
# <a name="server-hardware-performance-considerations"></a>服务器硬件性能注意事项

以下部分列出了在选择服务器硬件时应考虑的重要事项。 遵循这些指南可以帮助消除可能会影响服务器性能的性能瓶颈。

## <a name="processor-recommendations"></a>处理器建议

为服务器选择 64 位处理器。 64 位处理器拥有更多的地址空间，是 Windows Server 2016 所需的。 虽然将不提供 32 位版本的操作系统，但 32 位应用程序将在 64 位 Windows Server 2016 操作系统上运行。

若要增加服务器中的计算资源，可以使用带有高频核心的处理器，或者可以增加处理器核心数。 如果 CPU 是系统中的限制资源，那么与两个 1x 频率的核心相比，一个 2x 频率的核心通常能够提供更大的性能改进。

多个核心无法提供完美的线性缩放，如果启用超线程，缩放因子甚至还可能更少，因为超线程依赖于共享同一物理核心的资源。


>[!Important]
> 将内存和 I/O 子系统与 CPU 性能进行匹配并缩放，反之亦然。

请勿比较制造商之间和几代处理器之间的 CPU 频率，因为此比较结果可能是具有误导性的速度指标。

对于 Hyper-V 而言，请确保处理器支持 SLAT（二级地址转换）。 它由 Intel 实现为扩展页表 (EPT)，由 AMD 实现为嵌套页表 (NPT)。 你可以通过在服务器上使用 SystemInfo.exe 来验证此功能是否存在。

## <a name="cache-recommendations"></a>缓存建议

选择大型 L2 或 L3 处理器缓存。 较新的体系结构（例如 Haswell 或 Skylake）上有统一的最后一级缓存 (LLC) 或 L4。 更大的缓存通常能够提供更好的性能，并且通常能够发挥比原始 CPU 频率更大的作用。

## <a name="memory-ram-and-paging-storage-recommendations"></a>内存 (RAM) 和分页存储建议

>[!Note] 
> 相比 Windows Server 2012 R2，运行新安装 Windows Server 2016 时某些系统可能会出现存储性能降低的情况。 在 Windows Server 2016 的开发期间进行了大量更改，从而改进此平台的安全性和可靠性。 其中某些更改（例如默认启用 Windows Defender）会导致 I/O 路径拉长，进而导致在特定工作负荷和模式中 I/O 性能降低。 Microsoft 不建议禁用 Windows Defender，因为它是系统的重要保护层。 

增加 RAM 以满足内存需求。
当计算机内存不足并且需要立即补充更多内存时，Windows 将使用硬盘空间通过一个名为分页的过程来补充系统 RAM。 分页过多会降低整体系统性能。
你可以使用以下针对页面文件放置的指南来优化分页：
- 将页面文件隔离在此文件所在的存储设备上，或至少确保它不与其他经常访问的文件共享相同的存储设备。 例如，将页面文件和操作系统文件放置在单独的物理磁盘驱动器上。

- 将页面文件放置在不具备容错能力的驱动器上。 如果磁盘发生故障，则可能会发生系统崩溃。 如果将页面文件放置在具备容错能力的驱动器上，请记住，容错系统写入数据的速度通常较慢，因为它们会将数据写入到多个位置。

- 如果需要更多的磁盘带宽用于进行分页，请使用多个磁盘或磁盘阵列。 请勿将多个页面文件放置在同一物理磁盘驱动器的不同分区上。

## <a name="peripheral-bus-recommendations"></a>外设总线建议
在 Windows Server 2016 中，主要存储和网络接口应为 PCI 快速模块 (PCIe)，因此建议使用带有 PCIe 总线的服务器。 为避免总线速度受限，请使用适用于 10+ GB 以太网适配器的 PCIe x8 以及更高版本的槽。

## <a name="disk-recommendations"></a>磁盘建议
选择旋转速度较高的磁盘，从而减少随机请求服务时间（如果拿 7,200 RPM 和 15,000 RPM 驱动器相比，可以平均减少大约 2 毫秒）并增加顺序请求带宽。 但是，如果使用旋转速度较高的磁盘，还应考虑与之相关的成本、功能和其他注意事项。

与等效的 3.5 英寸驱动器相比，2.5 英寸的企业级磁盘每秒可以处理明显更多的随机请求数。

将经常访问的数据（尤其是按顺序访问的数据）存储在磁盘的开始部分，因为这大致对应于最外侧的（最快的）轨道。

将小型驱动器合并为较少的高容量驱动器会降低整体存储性能。 较少的轴意味着请求服务并发减少；因此，可能会降低吞吐量并延长响应时间（具体取决于工作负荷强度）。

对于高 I/O 率或延迟敏感型 I/O 的主读磁盘，使用 SSD 和高速闪存磁盘将非常有用。 使用启动磁盘可以很好地代替 SSD 或高速闪存磁盘，因为它们可以显著提高启动时间。

NVMe SSD 可以通过更好的命令队列深度、更高效的中断处理以及针对 4KB 命令更高的效率提供优异的性能。 这尤其适用于需要大量同步 I/O 的情况。


## <a name="network-and-storage-adapter-recommendations"></a>网络和存储器适配器建议

以下部分列出了适用于高性能服务器的网络和存储器适配器的建议特性。 这些设置可以帮助防止网络或存储硬件在负载过大时遭遇瓶颈。

### <a name="certified-adapter-usage"></a>使用认证的适配器
使用已通过 Windows 硬件认证测试套件的适配器。

### <a name="64-bit-capability"></a>64 位功能
支持 64 位的适配器可以对高物理内存位置（大于 4 GB）以及从中执行直接内存存取 (DMA) 操作。 如果驱动器不支持大于 4 GB 的 DMA，系统则会将 I/O 双缓冲到小于 4 GB 的物理地址空间。

### <a name="copper-and-fiber-adapters"></a>铜适配器和光纤适配器
铜适配器通常与光纤适配器具有相同的性能，并且在某些光纤通道适配器上均会使用铜和光纤。 某些环境更适合使用铜适配器，而其他环境则更适合使用光纤适配器。

### <a name="dual--or-quad-port-adapters"></a>双端口或四端口适配器
多端口适配器适用于具有有限数量的 PCI 槽的服务器。

为应对可连接到 SCSI 总线的磁盘数的 SCSI 限制，某些适配器在单个适配卡上提供了两条或四条 SCSI 总线。 光纤通道适配器通常对连接到适配器的磁盘数没有限制，除非它们隐藏在 SCSI 接口后面。

由于协议的串行特性，串行附加 SCSI (SAS) 和串行 ATA (SATA) 适配器也有连接数限制，但你可以使用开关来附加更多的磁盘。

网络适配器的此功能适用于负载均衡或故障转移的情况。 对于相同的工作负荷而言，使用两个单端口网络适配器通常会比使用一个双端口网络适配器获得更好的性能。

PCI 总线限制可能是限制多端口适配器性能的主要因素。 因此，请务必考虑将它们放置在能提供足够带宽的高性能 PCIe 槽中。

### <a name="interrupt-moderation"></a>中断调解
某些适配器可以裁决它们中断主机处理器的频率以指示任务处于活动状态或已完成。 裁决中断经常会导致主机上的 CPU 负载减少，除非中断裁决是以智能方式执行的；节省 CPU 可能会增加延迟。

### <a name="receive-side-scaling-rss-support"></a>接收方缩放 (RSS) 支持
RSS 使数据包接收处理能够根据可用计算机处理器的数量进行缩放。 这对于 10 GB 以太网以及更快的网络来说尤为重要。

### <a name="offload-capability-and-other-advanced-features-such-as-message-signaled-interrupt-msi-x"></a>卸载功能以及其他高级功能，例如消息信号中断 (MSI)-X
支持卸载的适配器可以提供能改善性能的 CPU 节省功能。

### <a name="dynamic-interrupt-and-deferred-procedure-call-dpc-redirection"></a>动态中断和延迟过程调用 (DPC) 重定向
在 Windows Server 2016 中，Numa I/O 启用 PCIe 存储器适配器来动态重定向中断和 DPC，并且可以通过改进工作负荷分区、提高缓存命中率，以及改进 I/O 密集型工作负荷的载入硬件互连使用情况为任何多处理器系统提供帮助。

## <a name="see-also"></a>另请参阅
- [服务器硬件电源注意事项](power.md)
- [电源和性能优化](power/power-performance-tuning.md)
- [处理器电源管理优化](power/processor-power-management-tuning.md)
- [建议的平衡计划参数](power/recommended-balanced-plan-parameters.md)
