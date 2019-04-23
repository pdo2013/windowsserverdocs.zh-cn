---
title: Hyper V 处理器性能
description: 中的 HYPER-V 性能优化的处理器性能注意事项
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 2a49fdaba89a01c8daf6483f72dbc88daa91452b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843238"
---
# <a name="hyper-v-processor-performance"></a>Hyper V 处理器性能


## <a name="virtual-machine-integration-services"></a>虚拟机集成服务

虚拟机集成服务包括适用于 Hyper V 特定 I/O 设备，这可大大减少 CPU 开销的比较对模拟设备的 I/O 已启用驱动程序。 您应在每个受支持的虚拟机中安装虚拟机集成服务的最新版本。 来宾从空闲状态的 CPU 使用率来宾到很大程度的服务减少使用来宾，并提高 I/O 吞吐量。 这是在运行 HYPER-V 的服务器的优化性能的第一步。 有关受支持的来宾操作系统的列表，请参阅[HYPER-V 概述](https://technet.microsoft.com/library/hh831531.aspx)。

## <a name="virtual-processors"></a>虚拟处理器

Windows Server 2016 中的 HYPER-V 支持的每个虚拟机 240 虚拟处理器的最大值。 不是 CPU 密集型的负载也有不同的虚拟机应配置为使用一个虚拟处理器。 这是因为多个虚拟处理器，如来宾操作系统中的其他同步成本与相关联的额外开销。

如果虚拟机需要多个 CPU 处理峰值负载下，请增加虚拟处理器的数量。

## <a name="background-activity"></a>后台活动

最大程度减少空闲虚拟机中的后台活动释放其他虚拟机可以在其他地方使用的 CPU 周期。 Windows 来宾处于空闲状态时，通常使用小于 1%的一个 CPU。 以下是最小化的后台 CPU 使用率的虚拟机的多个最佳做法：

-   安装最新版本的虚拟机集成服务。

-   删除通过虚拟机设置对话框 （使用 Microsoft Hyper V 特定适配器） 仿真的网络适配器。

-   删除未使用的设备，如 CD-ROM 和 COM 端口，或其介质断开连接。

-   在登录屏幕上保留 Windows 来宾操作系统未使用时，禁用屏幕保护程序。

-   查看计划的任务，并且默认情况下启用的服务。

-   查看 ETW 跟踪提供程序是在默认情况下，通过运行**logman.exe 查询-ets**

-   提高服务器应用程序，以减少周期性活动 （例如计时器）。

-   在主机和来宾操作系统上关闭服务器管理器。

-   不要将 Hyper-v 管理器运行，因为它会不断地刷新虚拟机的缩略图。

以下是配置的其他最佳做法*客户端版本*的 Windows 虚拟机中，以减少总体 CPU 使用率：

-   禁用后台服务，如 SuperFetch 和 Windows Search。

-   禁用计划的任务，例如计划碎片整理。

## <a name="virtual-numa"></a>虚拟 NUMA

若要启用虚拟化大型规模工作负载，Windows Server 2016 中的 HYPER-V，请扩展虚拟机扩展限制。 可以分配单个虚拟机，最多到 240 个虚拟处理器和 12 TB 的内存。 创建此类大型的虚拟机时，可能会使用从主机系统上的多个 NUMA 节点的内存。 在此类虚拟机配置中，如果虚拟处理器和内存从同一个 NUMA 节点中，不分配工作负荷可能具有错误的性能，因为无法充分利用 NUMA 优化。

在 Windows Server 2016 中，HYPER-V 提供到虚拟机的虚拟 NUMA 拓扑。 在默认情况下，将优化此虚拟 NUMA 拓扑以匹配基础主计算机的 NUMA 拓扑。 通过在虚拟机中公开虚拟 NUMA 拓扑，可允许来宾操作系统以及在其中运行的任何 NUMA 感知应用程序利用 NUMA 性能优化，就像在物理计算机上运行时的行为一样。

从工作负载的角度来看，虚拟和物理 NUMA 之间没有区别。 在虚拟机中，当工作负载为数据分配本地内存并在同一个 NUMA 节点中访问该数据时，将在基础物理系统上快速访问本地内存结果。 可成功避免由于远程内存访问而引起的性能损失。 只有 NUMA 感知应用程序可以受益的 vNUMA。

Microsoft SQL Server 是 NUMA 感知应用程序的示例。 有关详细信息，请参阅[了解非一致性内存访问](https://technet.microsoft.com/library/ms178144.aspx)。

无法同时使用虚拟 NUMA 和动态内存功能。 有效启用了动态内存的虚拟机只有一个虚拟 NUMA 节点，并且不会向虚拟机提供任何 NUMA 拓扑，无论虚拟 NUMA 设置如何。

虚拟 NUMA 的详细信息，请参阅[HYPER-V 虚拟 NUMA 概述](https://technet.microsoft.com/library/dn282282.aspx)。

##<a name="see-also"></a>请参阅

-   [HYPER-V 术语](terminology.md)

-   [HYPER-V 体系结构](architecture.md)

-   [HYPER-V 服务器配置](configuration.md)

-   [HYPER-V 内存性能](memory-performance.md)

-   [HYPER-V 存储 I/O 性能](storage-io-performance.md)

-   [HYPER-V 网络 I/O 性能](network-io-performance.md)

-   [虚拟化环境中检测瓶颈](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虚拟机](linux-virtual-machine-considerations.md)
