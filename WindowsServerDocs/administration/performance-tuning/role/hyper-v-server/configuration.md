---
title: Hyper-V 配置
description: 用于性能优化的 hyper-v 配置注意事项
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 0c608d3762c45a0b1478bcb3303159feef963291
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866622"
---
# <a name="hyper-v-configuration"></a>Hyper-V 配置

## <a name="hardware-selection"></a>硬件选择

运行 hyper-v 的服务器的硬件注意事项通常类似于非虚拟化服务器，但运行 Hyper-v 的服务器可能会提高 CPU 使用率，消耗更多内存，并且由于服务器合并，需要更大的 i/o 带宽。

-   **款**

    Windows Server 2016 中的 hyper-v 将逻辑处理器显示为每个活动虚拟机的一个或多个虚拟处理器。 Hyper-v 现在需要支持二级地址转换（SLAT）技术（如扩展页表（EPT））或嵌套页表（NPT）的处理器。

-   **Cache**

    Hyper-v 可以受益于更大的处理器缓存，尤其是对于在内存中具有较大工作集的负载以及虚拟机与逻辑处理器的比率较高的虚拟机配置。

-   **内存**

    物理服务器需要足够的内存用于根分区和子分区。 根分区需要内存来代表虚拟机和操作（例如虚拟机快照）来有效地执行 i/o。 Hyper-v 确保有足够的内存可用于根分区，并允许将剩余的内存分配给子分区。 子分区应根据每个虚拟机的预期负载需求进行调整。

-   **存储**

    存储硬件应具有足够的 i/o 带宽和容量，以满足物理服务器托管的虚拟机当前和未来的需要。 选择存储控制器和磁盘并选择 RAID 配置时，请考虑以下要求。 在不同的物理磁盘上放置具有大量磁盘密集型工作负荷的虚拟机可能会提高整体性能。 例如，如果有四个虚拟机共享单个磁盘并主动使用，则每个虚拟机只能产生该磁盘的 25% 的带宽。

## <a name="power-plan-considerations"></a>电源计划注意事项

作为一种核心技术，虚拟化是一种功能强大的工具，可用于提高服务器工作负荷密度，减少数据中心所需的物理服务器的数量，提高运营效率并降低功耗成本。 电源管理对成本管理至关重要。 

在理想的数据中心环境中，通过将工作整合到计算机上，直到它们最忙碌，并关闭闲置计算机，从而管理功率消耗。 如果这种方法不可行，管理员可以利用物理主机上的电源计划，确保它们不会消耗更多的电量。 

服务器电源管理技术附带了一个成本，特别是租户工作负荷不受信任，不能规定有关宿主主机的物理基础结构的策略。 主机层软件将在最大限度地降低功率的同时，推断如何最大化吞吐量。 在大多数闲置的计算机中，这可能会导致物理基础结构得出中等功耗的结论，导致各个租户工作负荷的运行速度比其他情况下的运行速度更慢。

Windows Server 在各种情况下使用虚拟化。 从负载较轻的 IIS 服务器到适度繁忙的 SQL Server，到每台服务器上运行数百个虚拟机的 Hyper-v。 其中每个方案都有独特的硬件、软件和性能要求。 默认情况下，Windows Server 使用并推荐**平衡**电源计划，该计划基于 CPU 利用率扩展处理器性能来实现节能。

利用**平衡**电源计划，仅当物理主机相对繁忙时，才应用最高电源状态（以及租户工作负荷中的最低响应延迟）。 如果对所有租户工作负荷值具有确定性、低延迟的响应，则应考虑从默认的**均衡**电源计划切换到**高性能**电源计划。 **高性能**电源计划将始终运行处理器全速，并有效地禁用基于需求的切换以及其他电源管理技术，并通过节能优化性能。

对于满足以下条件的客户：减少了减少物理服务器的数量，并希望确保它们为虚拟化工作负荷实现最大性能，则应考虑使用**高性能**电源计划。

若要深入了解如何利用电源计划优化基础结构，请阅读[建议的均衡电源计划参数以获得快速响应时间](../../hardware/power/recommended-balanced-plan-parameters.md)



## <a name="server-core-installation-option"></a>服务器核心安装选项

Windows Server 2016 功能是服务器核心安装选项。 服务器核心提供了一个用于承载一组选择的服务器角色（包括 Hyper-v）的最小环境。 它为主机操作系统提供较小的磁盘空间，以及较小的攻击和维护图面。 因此，强烈建议 Hyper-v 虚拟化服务器使用服务器核心安装选项。

仅当用户登录时，服务器核心安装才会提供控制台窗口，但 Hyper-v 会公开包含[Windows Powershell](https://technet.microsoft.com/library/hh848559.aspx)的远程管理功能，以便管理员可以远程管理它。

## <a name="dedicated-server-role"></a>专用服务器角色

根分区应专用于 Hyper-v。 在运行 Hyper-v 的服务器上运行其他服务器角色可能会对虚拟化服务器的性能产生负面影响，特别是在消耗大量 CPU、内存或 i/o 带宽时。 最大程度地减少根分区中的服务器角色具有其他优点，例如降低攻击面。

系统管理员应仔细考虑根分区中安装的软件，因为某些软件可能会对运行 Hyper-v 的服务器的整体性能产生负面影响。

## <a name="guest-operating-systems"></a>来宾操作系统

Hyper-v 支持并已针对多个不同的来宾操作系统进行了优化。 每个来宾支持的虚拟处理器数量取决于来宾操作系统。 有关支持的来宾操作系统的列表，请参阅[Hyper-v 概述](https://technet.microsoft.com/library/hh831531.aspx)。

## <a name="cpu-statistics"></a>CPU 统计信息

Hyper-v 发布性能计数器以帮助描述虚拟化服务器的行为，并报告资源使用情况。 用于在 Windows 中查看性能计数器的一组标准工具包括性能监视器和 Logman，它可以显示和记录 Hyper-v 性能计数器。 相关计数器对象的名称以**hyper-v**为前缀。

应始终使用 Hyper-v 虚拟机监控程序逻辑处理器性能计数器来度量物理系统的 CPU 使用情况。 根和子分区中的任务管理器和性能监视器报表的 CPU 使用率计数器不反映实际的物理 CPU 使用情况。 使用下列性能计数器来监视性能：

- **Hyper-v 虚拟机监控程序逻辑处理器（\*）\\% 总运行时间总**的逻辑处理器非空闲时间

- **Hyper-v 虚拟机监控程序逻辑处理器（\*）\\% Guest 运行时间**在来宾内或主机内运行周期所用的时间

- **Hyper-v 虚拟机监控程序逻辑处理器（\*）\\% 监控程序运行时间**在虚拟机监控程序内运行的时间

- **Hyper-v 虚拟机监控程序根虚拟处理器（\*）\\\\** * 测量根分区的 CPU 使用率

- **Hyper-v 虚拟机监控程序虚拟处理器（\*）\\\\** * 测量来宾分区的 CPU 使用情况


## <a name="see-also"></a>请参阅

-   [Hyper-V 术语](terminology.md)

-   [Hyper-V 体系结构](architecture.md)

-   [Hyper-V 处理器性能](processor-performance.md)

-   [Hyper-V 内存性能](memory-performance.md)

-   [Hyper-V 存储 I/O 性能](storage-io-performance.md)

-   [Hyper-V 网络 I/O 性能](network-io-performance.md)

-   [检测虚拟化环境中的瓶颈](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虚拟机](linux-virtual-machine-considerations.md)
