---
title: 有关 HYPER-V 虚拟机监控程序计划程序类型选择
description: 提供有关 HYPER-V 的计划程序的使用模式的 HYPER-V 主机管理员的信息
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: c5360d8e2fdc23f8b05c6be0f665407eebedeba2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823878"
---
# <a name="about-hyper-v-hypervisor-scheduler-type-selection"></a>有关 HYPER-V 虚拟机监控程序计划程序类型选择

适用于：

* Windows Server 2016
* Windows Server 版本 1709
* Windows Server 版本 1803
* Windows Server 2019

本文档介绍了为 HYPER-V 的默认值的重要更改，并推荐使用的虚拟机监控程序的计划程序类型。 这些更改会影响这两个系统安全和虚拟化性能。 虚拟化主机管理员应查看并了解其更改和本文档中所述的影响，应认真评估影响，建议的部署指南和以更好地理解如何部署和管理所涉及的风险因素面对快速变化的安全环境的 HYPER-V 主机。

>[!IMPORTANT]
>当前已知的障碍在多个处理器体系结构中的漏洞可能被恶意来宾 VM 同时进行的主机上运行时的旧虚拟机监控程序经典的计划程序类型的计划行为通过利用端通道安全性多线程处理 (SMT) 启用。  如果成功利用此漏洞的恶意工作负荷可以观察到其分区边界之外的数据。 通过配置的 HYPER-V 虚拟机监控程序可以利用虚拟机监控程序 core 计划程序类型和重新配置来宾虚拟机，可以降低此类攻击。 利用内核计划程序，虚拟机监控程序将限制来宾虚拟机的 VPs 上相同的物理处理器核心，因此强隔离到 access 数据移到其运行所在的物理核心的边界的 VM 的功能运行。  这是很有效的缓解措施对这些旁路攻击，这样可防止 VM 观察其他分区，所有项目是否为根或另一个来宾分区。  因此，Microsoft 正在更改默认值，并建议虚拟化主机和来宾虚拟机的配置设置。

## <a name="background"></a>后台

从 Windows Server 2016 开始，HYPER-V 支持计划和管理虚拟处理器，称为虚拟机监控程序计划程序类型的多个的方法。  虚拟机监控程序计划程序的所有类型的详细的说明可在[理解和使用的 HYPER-V 虚拟机监控程序计划程序类型](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types)。

>[!NOTE]
>虚拟机监控程序计划程序的新型首次随 Windows Server 2016 中，并在早期版本中不可用。 所有版本的 Windows Server 2016 之前的 HYPER-V 都支持仅经典的计划程序。 最近才发布对核心计划程序的支持。

## <a name="about-hypervisor-scheduler-types"></a>有关虚拟机监控程序计划程序类型

本文将重点介绍特定于使用与旧的"经典"计划程序的新虚拟机监控程序 core 计划程序类型和这些计划程序类型如何与使用对称多线程处理或 SMT 相交。  请务必了解的核心和经典部署模型的计划程序和如何每个基础系统处理器上放置工作从来宾 Vm 之间的差异。

### <a name="the-classic-scheduler"></a>经典的计划程序

经典的计划程序是指跨系统-包括根 VPs VPs 属于来宾虚拟机调度虚拟处理器 (VPs) 上的工作的公平共享轮循机制方法。 经典的计划程序已在所有版本的 HYPER-V （直到 Windows Server 2019，此处所述) 所使用的默认计划程序类型。  经典的计划程序的性能特征很好地了解，经典的计划程序进行了演示 ably 支持过度订阅的工作负荷-即，过度订阅的主机的 VP:LP 比率由合理边距 (具体取决于类型的工作负荷被虚拟化、 整体资源利用率，等等。）。

当虚拟化主机上运行 SMT 启用时，经典的计划程序将计划从独立属于 core 每个 SMT 线程上的任何 VM 的来宾 VPs。 因此，不同的 Vm 可以在上运行的同一内核在同一时间 (一个 VM 核心的一个线程上运行，而在其他线程上运行的另一个 VM)。

### <a name="the-core-scheduler"></a>核心计划程序

核心计划程序利用 SMT 来提供隔离的来宾工作负载，这会影响安全和系统性能的属性。 核心计划程序可确保从 VM VPs 同级 SMT 线程上安排。 这是对称，以便如果 LPs 中组的两个，VPs 计划两个，一组和 Vm 之间永远不会共享的系统 CPU 核心。

通过计划对基础 SMT 对来宾 VPs，核心计划程序提供了强大的安全边界的工作负荷隔离，并还可用于减少对延迟敏感的工作负荷的性能变化。

请注意，不启用，SMT 的情况下为虚拟机计划副总裁时运行，和 core 的同级 SMT 线程保持空闲状态时，副总裁将占用的所有核心。  这是必须提供正确的工作负荷隔离，但影响整个系统的性能，尤其是随着系统 LPs 变得过度订阅-即，总 VP:LP 比率超过 1 对 1 时。 因此，正在运行的来宾 Vm 配置为不包含每个核心的多个线程是不够理想配置。

### <a name="benefits-of-the-using-the-core-scheduler"></a>使用核心计划程序的好处

核心计划程序提供以下优势：

* 若要在基础物理核心对，减少到窥探攻击旁, 道漏洞上运行大容量限制来宾工作负荷隔离-来宾 VPs 的强大的安全边界。

* 减少工作负荷可变性-来宾工作负荷吞吐量可变性大大减少了，提供更好的工作负荷一致性。

* 在来宾 Vm 的 OS 和来宾虚拟机中运行的应用程序中的 SMT 使用可以利用 SMT 行为和编程接口 (Api) 来控制和在 SMT 线程间分发工作，就像它们时将运行非虚拟化。

当前在 Azure 虚拟化主机上使用的核心计划程序专门用于充分利用强大的安全边界和较低的工作负荷 variabilty。 Microsoft 认为核心计划程序类型应为和仍为默认虚拟机监控程序计划大多数虚拟化方案的类型。  因此，若要确保我们的客户安全默认情况下，Microsoft 正在 Windows Server 2019 此更改现在。

### <a name="core-scheduler-performance-impacts-on-guest-workloads"></a>核心计划程序上来宾工作负载的性能影响

虽然需要有效地缓解特定类别的漏洞，但 core 计划程序可能也有可能会降低性能。 客户可能会看到其 Vm 和影响的性能特征与总体工作负荷容量及其虚拟化主机的差异。 在 core 计划程序必须运行非 SMT VP 的位置的情况下，只有一个基础的逻辑内核中的指令流执行，而其他必须保持空闲状态。 这将限制来宾工作负载的总主机容量。

可以按照本文档中的部署指南最小化这些性能影响。 主机管理员必须仔细考虑其特定的虚拟化部署方案和平衡其所允许的安全风险与要使用最大工作负荷密度、 过度合并虚拟化主机，等等。

## <a name="changes-to-the-default-and-recommended-configurations-for-windows-server-2016-and-windows-server-2019"></a>设为默认与建议的配置 Windows Server 2016 和 Windows Server 2019 更改

部署使用的最大的安全状况的 HYPER-V 主机需要使用的虚拟机监控程序 core 计划程序类型。 若要确保我们的客户安全默认情况下，Microsoft 更改了以下默认和推荐的设置。

>[!NOTE]
>虽然对计划程序类型的虚拟机监控程序的内部支持已包含在 Windows Server 2016、 Windows Server 1709 和 Windows Server 1803 的初始版本中，更新所需访问允许您选择的配置控件虚拟机监控程序计划程序类型。  请参阅[理解和使用的 HYPER-V 虚拟机监控程序计划程序类型](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types)有关这些更新的详细信息。

### <a name="virtualization-host-changes"></a>虚拟化主机更改

* 虚拟机监控程序将由默认 Windows Server 2019 开头使用核心计划程序。

* Microsoft Windows Server 2016 上配置核心计划程序 reccommends。 虚拟机监控程序 core 计划程序类型支持在 Windows Server 2016 中，但默认值是经典的计划程序。 核心计划程序是可选的必须显式启用的 HYPER-V 主机管理员。

### <a name="virtual-machine-configuration-changes"></a>更改虚拟机配置

* 在 Windows Server 2019 上使用默认 VM 9.0 版创建的新虚拟机将自动继承 SMT 属性 （启用或禁用） 的虚拟化主机。 也就是说，如果 SMT 上启用了新创建的 Vm 在物理主机也会启用，SMT，并将继承主机的 SMT 拓扑默认情况下，与拥有作为基础系统相同的每个核心的硬件线程的 VM。 这将会反映在 VM 的配置与 HwThreadCountPerCore = 的 0，0 指示 VM 应继承主机的 SMT 设置。

* 8.2 或更早版本将 VM 版本的现有虚拟机保留 HwThreadCountPerCore，其原始 VM 处理器设置和 8.2 VM 版本来宾的默认值是 HwThreadCountPerCore = 1。 当这些来宾运行 Windows Server 2019 主机上时，将处理它们，如下所示：

    1. 如果 VM 有的副总裁计数小于或等于的 LP 核心计数，VM 会将其视为作为非 SMT VM 核心计划程序。 当一个单一的 SMT 线程上运行来宾副总裁时，core 的同级 SMT 线程将空闲状态。 这是未优化，将导致整体会降低性能。

    2. 如果 VM 有更多 VPs 比 LP 内核，VM 将作为 SMT VM 被视为由 core 计划程序。 但是，VM 将不遵循其他表明它是 SMT VM 的特征。 例如，使用 CPUID 指令或 Windows Api 来查询由 OS 或应用程序的 CPU 拓扑将指示启用了 SMT。

* 当显式更新现有 VM 从前面 VM 版本更新 VM 操作通过 9.0 版时，VM 将为 HwThreadCountPerCore 保留其当前值。  VM 不会强制启用 SMT。

* Windows Server 2016 中，Microsoft 建议为来宾 Vm 中启用 SMT。  默认情况下，将禁用 SMT，Windows Server 2016 上创建的 Vm，是 HwThreadCountPerCore 设置为 1，除非显式更改。

>[!NOTE]
>Windows Server 2016 不支持设置 HwThreadCountPerCore 为 0。

#### <a name="managing-virtual-machine-smt-configuration"></a>管理虚拟机 SMT 配置

在每个 VM 的基础上设置来宾虚拟机 SMT 配置。 主机管理员可以检查并配置虚拟机的 SMT 配置从以下选项中进行选择：

    1. 配置 Vm 以运行为 SMT 已启用的根据需要自动继承主机 SMT 拓扑

    2. 配置 Vm 以运行为非 SMT

VM SMT 配置显示在摘要窗格中的 HYPER-V 管理器控制台中。  配置虚拟机的 SMT 设置可能会使用 VM 设置或 PowerShell 来完成。

#### <a name="configuring-vm-smt-settings-using-powershell"></a>使用 PowerShell 配置 VM SMT 设置

若要配置 SMT 设置来宾虚拟机，请使用足够的权限，然后键入打开 PowerShell 窗口：

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

其中：

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

若要阅读来宾虚拟机的 SMT 设置，请使用足够的权限，然后键入打开 PowerShell 窗口：

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

请注意该来宾虚拟机配置了 HwThreadCountPerCore = 0 指示 SMT 来宾，将启用，因为它们是在基础虚拟化主机上，通常 2 将公开到来宾的 SMT 线程的数量。

### <a name="guest-vms-may-observe-changes-to-cpu-topology-across-vm-mobility-scenarios"></a>来宾 Vm 可能会观察到 CPU 拓扑更改到跨 VM 移动性方案

操作系统和在 VM 中的应用程序可能会看到对主机和之前的 VM 设置和更改后 VM 生命周期事件如实时迁移或保存和还原操作。 在哪些 VM 中保存和还原状态的操作，期间将迁移虚拟机的 HwThreadCountPerCore 设置和实现的值 （即，计算虚拟机的设置和源主机的配置的组合）。 VM 将继续使用这些设置在目标主机上的运行。 在点关闭 VM 后，重新启动，可能实现的值观察到的 VM 会更改。 这应是良性的为 OS 和应用程序层软件应该寻找 CPU 拓扑信息作为其正常的启动和初始化代码流的一部分。 但是，因为序列在实时迁移或保存/还原操作，进行这些状态转换的虚拟机期间已跳过这些启动时初始化可以观察到最初计算实际值，直到他们关闭并重新启动。  

### <a name="alerts-regarding-non-optimal-vm-configurations"></a>有关 VM 配置未优化的警报

虚拟机配置了多个 VPs 比物理内核数对主机结果中的非最佳配置。 虚拟机监控程序计划程序会将这些虚拟机，就好像它们是 SMT 感知。 但是，操作系统在 VM 中的应用程序软件将会出现显示禁用 SMT 的 CPU 拓扑。 HYPER-V 工作进程检测到这种情况时，将记录一个事件，虚拟机的配置是否最，并且建议 SMT 会警告主机管理员在虚拟化主机上为虚拟机启用。

#### <a name="how-to-identify-non-optimally-configured-vms"></a>如何识别非最佳方式配置 Vm

您可以标识非 SMT Vm 通过检查系统日志事件查看器中的 HYPER-V 工作进程事件 ID 3498 VPs VM 中的数字大于物理核心计数时将触发 vm。 从事件查看器，或通过 PowerShell，可以获取工作进程事件。

#### <a name="querying-the-hyper-v-worker-process-vm-event-using-powershell"></a>查询使用 PowerShell 的 HYPER-V 工作进程 VM 事件

查询为 HYPER-V 工作进程事件 ID 3498 使用 PowerShell，输入以下命令从 PowerShell 提示符。

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### <a name="impacts-of-guest-smt-configuaration-on-the-use-of-hypervisor-enlightenments-for-guest-operating-systems"></a>有关使用的来宾操作系统的虚拟机监控程序自旋来宾 SMT 配置的影响

Microsoft 虚拟机监控程序提供了多个启发方法或提示，这在来宾 VM 中运行的 OS 可以查询，并使用触发优化，如那些可能有益于性能或运行时，否则提高处理各种不同的条件虚拟化。 一个最近引入的一种启发式涉及的虚拟处理器调度处理和使用旁路攻击，可利用 SMT OS 缓解措施。

>[!NOTE]
>Microsoft 建议主机管理员启用来宾 Vm 来优化工作负荷性能的 SMT。

此来宾一种启发式的详细信息都是提供下面，但是虚拟化主机管理员的关键点是虚拟机应具有 HwThreadCountPerCore 配置为与主机的物理 SMT 配置相匹配。 这允许虚拟机监控程序来报告是否有任何非体系结构的核心共享。 因此，可以启用需要一种启发式任何来宾 OS 支持优化。 在 Windows Server 2019 上创建新的 Vm 并保留默认值为 HwThreadCountPerCore (0)。 从 Windows Server 2016 迁移较旧的 Vm 主机可以更新到 Windows Server 2019 配置版本。 执行此操作，Microsoft 建议设置 HwThreadCountPerCore 之后 = 0。  在 Windows Server 2016 中，Microsoft 建议设置 HwThreadCountPerCore 以匹配的主机配置 (通常为 2)。

### <a name="nononarchitecturalcoresharing-enlightenment-details"></a>NoNonArchitecturalCoreSharing 一种启发式详细信息

从 Windows Server 2016 开始，虚拟机监控程序定义来描述其处理的副总裁计划和来宾操作系统的放置新一种启发式。 在中定义此一种启发式[虚拟机监控程序顶层功能规范 v5.0c](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs)。

虚拟机监控程序综合 CPUID 叶 CPUID.0x40000004.EAX:18[NoNonArchitecturalCoreSharing = 1] 指示虚拟处理器永远不会将共享物理核心使用另一个虚拟处理器，除了报告为同级 SMT 的虚拟处理器线程数。 例如，来宾副总裁将永远不会运行与根副总裁一起 SMT 线程上在同级节点上相同的处理器内核的 SMT 线程上同时运行。 这种情况时，才可以运行虚拟化时，因此表示一种非体系结构的 SMT 行为，还具有严重的安全隐患。 来宾操作系统可以使用 NoNonArchitecturalCoreSharing = 1 以指示它是安全启用优化，可帮助其避免设置 STIBP 的性能开销。

在某些配置中，虚拟机监控程序将不指明该 NoNonArchitecturalCoreSharing = 1。 例如，如果主机已启用的 SMT 并且配置为使用虚拟机监控程序经典计划程序中，NoNonArchitecturalCoreSharing 将为 0。 这可能会阻止启用某些优化已启用的来宾。 因此，Microsoft 建议使用 SMT 主机管理员依赖于虚拟机监控程序 core 计划程序，并确保虚拟机配置为继承其 SMT 配置从主机以确保实现最佳工作负载的性能。

## <a name="summary"></a>总结

安全威胁形态不断发展。 若要确保我们的客户安全默认情况下，Microsoft 正在更改的默认配置的虚拟机监控程序和虚拟机启动 Windows Server 2019 HYPER-V 中，并提供更新的指导和建议运行 Windows 的客户Server 2016 的 Hyper-v。 虚拟化主机管理员应：

* 阅读并理解本文档中提供的指南

* 仔细评估并调整其虚拟化部署以确保它们满足安全、 性能、 虚拟化密度和其独特需求的工作负荷响应能力目标

* 请考虑重新配置现有的 Windows Server 2016 HYPER-V 主机，可以利用虚拟机监控程序 core 计划程序提供的强大的安全优势

* 更新现有非 SMT Vm 以从计划解决了硬件安全漏洞的副总裁隔离所规定的约束减少性能影响
