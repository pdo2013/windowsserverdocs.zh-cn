---
title: Hyper-V 配置
description: 有关性能调整的 HYPER-V 配置注意事项
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: e5e55ad492439fcb7150469d9a35b639f5ff9a2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830508"
---
# <a name="hyper-v-configuration"></a>Hyper-V 配置

## <a name="hardware-selection"></a>硬件选择

通常运行 HYPER-V 的服务器的硬件注意事项类似于那些非虚拟化服务器，但运行 HYPER-V 的服务器可以 cpu，占用较多内存，而且由于服务器合并需要更大的 I/O 带宽。

-   **处理器**

    Windows Server 2016 中的 HYPER-V 显示逻辑处理器，并且为每个活动的虚拟机的一个或多个虚拟处理器。 HYPER-V 现在需要支持二级地址转换 (SLAT) 技术，例如扩展页表 (EPT) 或嵌套页表 (NPT) 的处理器。

-   **Cache**

    HYPER-V 可以受益于更大的处理器缓存，尤其是对于大型工作集在内存中和在其中的虚拟处理器到逻辑处理器比率较高的虚拟机配置中的负载。

-   **内存**

    物理服务器的根和子分区需要足够的内存。 根分区需要的内存来有效地执行 I/o 代表的虚拟机和虚拟机快照等操作。 HYPER-V 可确保有足够的内存可供根分区，并且允许剩余的内存分配给子分区。 根据每个虚拟机的预期负载需求，应调整子分区。

-   **存储**

    存储硬件应该有足够的 I/O 带宽和容量，以满足当前和未来需求的虚拟机的物理服务器主机。 选择存储控制器和磁盘，并选择 RAID 配置时，请考虑这些要求。 将具有高磁盘密集型工作负荷的虚拟机放置在不同的物理磁盘上可能会提高整体性能。 例如，如果四个虚拟机共享单个磁盘，并积极地使用它，每个虚拟机可以生成仅该磁盘的带宽的 25%。

## <a name="power-plan-considerations"></a>电源计划注意事项

作为一项核心技术，虚拟化是一个强大的工具中增加服务器工作负荷密度、 减少数据中心内的所需的物理服务器的数量、 提高运营效率和降低功耗成本非常有用。 电源管理的成本管理至关重要。 

在理想的数据中心环境中，由合并到计算机上的工作，直到它们主要是正忙，然后关闭空闲状态机管理功率消耗。 如果此方法不可行，管理员可以利用的物理主机的电源计划，以确保它们不会占用不必要的更多能力。 

服务器电源管理技术需要一定的代价，尤其是当租户工作负荷不受信任，可以规定的策略来托管商的物理基础结构。 保留主机层软件来推断如何使吞吐量最大化时最大程度减少电源消耗。 在主要空闲状态机中，这会导致物理基础结构才能得出结论： 中等 power 绘图是适当情况下，从而导致速度低于否则它们可能运行的每个租户工作负荷。

Windows Server 使用各种方案中的虚拟化。 从 IIS 服务器太忙的 SQL Server 到轻，到具有 HYPER-V 的云主机运行数百个每个服务器的虚拟机。 每种方案可能具有唯一的硬件、 软件和性能要求。 默认情况下，Windows Server 使用和建议**平衡**电源计划可让你节省电源来缩放的处理器性能基于 CPU 使用率。

与**平衡**相对忙而在物理主机时应用电源计划、 最高的电源状态 （和租户工作负荷的最小响应延迟）。 如果为所有租户工作负荷值确定性的、 低延迟响应，则应考虑从默认的切换**平衡**到电源计划**高性能**电源计划。 **高性能**电源计划将处理器全速运行所有的时间，有效地禁用基于需求的切换以及其他电源管理技术，并通过节能优化性能。

对于客户感到满意从物理服务器的数量减少的成本节省，并想要确保它们实现其虚拟化工作负荷的最大性能，应考虑使用**高性能**电源计划。

有关其他建议和利用电源计划，以优化你的基础结构的深入，读取[建议平衡电源计划参数的快速响应时间](../../hardware/power/recommended-balanced-plan-parameters.md)



## <a name="server-core-installation-option"></a>服务器核心安装选项

Windows Server 2016 功能的服务器核心安装选项。 服务器核心提供了用于托管选定的一组包括 HYPER-V 的服务器角色的最小环境。 它提供了针对主机 OS，较小磁盘空间占用量和较小攻击和维护服务图面。 因此，我们强烈建议的 HYPER-V 虚拟化服务器使用服务器核心安装选项。

服务器核心安装提供了一个控制台窗口，仅当用户登录，但 HYPER-V 会公开远程管理功能，包括[Windows Powershell](https://technet.microsoft.com/library/hh848559.aspx)使管理员可以远程管理它。

## <a name="dedicated-server-role"></a>专用的服务器角色

根分区应将专用于 HYPER-V。 运行 HYPER-V 的服务器上运行其他服务器角色可以性能产生负面影响的虚拟化服务器，尤其是在它们需要占用大量 CPU、 内存或 I/O 带宽。 最大程度减少根分区中的服务器角色中还有其他好处，例如减小攻击面。

由于某些软件会产生负面影响运行 HYPER-V 的服务器的整体性能，根分区中安装的软件，系统管理员应仔细考虑。

## <a name="guest-operating-systems"></a>来宾操作系统

HYPER-V 支持并已被优化的几个不同的来宾操作系统。 每个来宾支持的虚拟处理器的数目取决于来宾操作系统。 有关受支持的来宾操作系统的列表，请参阅[HYPER-V 概述](https://technet.microsoft.com/library/hh831531.aspx)。

## <a name="cpu-statistics"></a>CPU 统计信息

HYPER-V 发布性能计数器来帮助描述虚拟化服务器的行为的特征和报告资源使用情况。 在 Windows 中查看性能计数器的工具的标准集包括性能监视器和 Logman.exe，它们可以显示和记录 HYPER-V 性能计数器。 相关计数器对象的名称将使用前缀**HYPER-V**。

始终应通过使用 HYPER-V 虚拟机监控程序逻辑处理器性能计数器来测量物理系统的 CPU 使用率。 CPU 利用率计数器根目录中的任务管理器和性能监视器报表和子分区不反映实际的物理 CPU 使用情况。 使用以下性能计数器来监视性能：

-   **HYPER-V 虚拟机监控程序逻辑处理器 (\*)\\总运行时间百分比**逻辑处理器的总非空闲时间

-   **HYPER-V 虚拟机监控程序逻辑处理器 (\*)\\来宾运行时间百分比**所花费的时间内主机或来宾内运行周期

-   **HYPER-V 虚拟机监控程序逻辑处理器 (\*)\\虚拟机监控程序运行时间百分比**内虚拟机监控程序运行花费的时间

-   **HYPER-V 虚拟机监控程序根虚拟处理器 (\*)\\ \*** 测量根分区的 CPU 使用率

-   **HYPER-V 虚拟机监控程序虚拟处理器 (\*)\\ \*** 测量来宾分区的 CPU 使用率


## <a name="see-also"></a>请参阅

-   [HYPER-V 术语](terminology.md)

-   [HYPER-V 体系结构](architecture.md)

-   [HYPER-V 处理器性能](processor-performance.md)

-   [HYPER-V 内存性能](memory-performance.md)

-   [HYPER-V 存储 I/O 性能](storage-io-performance.md)

-   [HYPER-V 网络 I/O 性能](network-io-performance.md)

-   [虚拟化环境中检测瓶颈](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虚拟机](linux-virtual-machine-considerations.md)
