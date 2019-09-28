---
title: Hyper-v 网络 i/o 性能
description: Hyper-v 性能优化中的网络 i/o 性能注意事项
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: e8f4261c11a63786c2d170105fb0fa65dc6966a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385121"
---
# <a name="hyper-v-network-io-performance"></a>Hyper-v 网络 i/o 性能

服务器2016包含多项改进和新功能，可在 Hyper-v 下优化网络性能。  本文的后续版本中将包含有关如何优化网络性能的文档。

## <a name="live-migration"></a>实时迁移

实时迁移使你能够以透明方式将运行中的虚拟机从故障转移群集的一个节点移动到同一群集中的另一个节点，而无需断开网络连接或感觉到停机。

> [!NOTE]
> 故障转移群集需要群集节点的共享存储。

移动正在运行的虚拟机的过程可分为两个主要阶段。 第一个阶段是将虚拟机的内存从当前主机复制到新主机。 第二个阶段将虚拟机状态从当前主机传输到新主机。 这两个阶段的持续时间很大程度上取决于将数据从当前主机传输到新主机的速度。

为实时迁移流量提供专用网络有助于最大程度地减少完成实时迁移所需的时间，并确保迁移时间一致。

![hyper-v 实时迁移配置示例](../../media/perftune-guide-live-migration.png)

此外，在迁移所涉及的每个网络适配器上增加发送和接收缓冲区的数量可以提高迁移性能。

Windows Server 2012 R2 引入了一个选项，可通过在网络上传输之前压缩内存来加速实时迁移，或使用远程直接内存访问（RDMA）（如果硬件支持）。

## <a name="see-also"></a>请参阅

-   [Hyper-V 术语](terminology.md)

-   [Hyper-V 体系结构](architecture.md)

-   [Hyper-V 服务器配置](configuration.md)

-   [Hyper-V 处理器性能](processor-performance.md)

-   [Hyper-V 内存性能](memory-performance.md)

-   [Hyper-V 存储 I/O 性能](storage-io-performance.md)

-   [检测虚拟化环境中的瓶颈](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虚拟机](linux-virtual-machine-considerations.md)
