---
title: 关于 Hyper-v 虚拟机监控程序计划程序类型选择
description: 提供有关 hyper-v 主机管理员使用 Hyper-v 计划程序模式的信息
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: 1e77535548cccd1c821163dabbad381f35d2948a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872058"
---
# <a name="about-hyper-v-hypervisor-scheduler-type-selection"></a>关于 Hyper-v 虚拟机监控程序计划程序类型选择

适用于：

* Windows Server 2016
* Windows Server 版本 1709
* Windows Server 版本 1803
* Windows Server 2019

本文档介绍 Hyper-v 默认情况下对虚拟机监控程序计划程序类型的建议使用的重要更改。 这些更改会影响系统安全性和虚拟化性能。 虚拟化主机管理员应查看并了解本文档中所述的更改和影响，并仔细评估所涉及的影响、建议的部署指南和风险因素，以最大程度地了解如何部署和管理Hyper-v 主机面对快速变化的安全环境。

>[!IMPORTANT]
>在同时运行的主机上运行时，可能会通过旧的虚拟机监控程序经典计划程序类型的计划行为，在多个处理器体系结构中短视在多个处理器体系结构中对已知的端通道安全漏洞进行攻击已启用多线程处理（SMT）。  如果成功利用了，恶意工作负荷可能会观察到其分区边界之外的数据。 通过配置 Hyper-v 虚拟机监控程序来利用虚拟机监控程序核心计划程序类型和重新配置来宾 Vm，可以减轻此类攻击。 使用核心计划程序，虚拟机监控程序限制来宾 VM 的 VPs 在相同的物理处理器核心上运行，因此，将 VM 的能力限制在其运行的物理内核的边界之外。  这是对这些方通道攻击的一种非常有效的缓解措施，这会阻止 VM 观察其他分区中的任何项目，无论根还是其他来宾分区。  因此，Microsoft 正在更改虚拟化主机和来宾 Vm 的默认配置设置和建议配置设置。

## <a name="background"></a>后台

从 Windows Server 2016 开始，Hyper-v 支持多种计划和管理虚拟处理器的方法，称为虚拟机监控程序计划程序类型。  有关所有虚拟机监控程序计划程序类型的详细说明，请参阅[了解和使用 hyper-v 虚拟机监控程序计划程序类型](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types)。

>[!NOTE]
>新的虚拟机监控程序计划程序类型最初随 Windows Server 2016 一起引入，在以前的版本中不可用。 Windows Server 2016 之前的所有 Hyper-v 版本仅支持经典计划程序。 仅最近发布了对核心计划程序的支持。

## <a name="about-hypervisor-scheduler-types"></a>关于虚拟机监控程序计划程序类型

本文重点介绍如何使用新的虚拟机监控程序核心计划程序类型与旧的 "经典" 计划程序，以及这些计划程序类型与对称多线程处理或 SMT 的使用的交集。  务必了解核心计划程序和经典计划程序之间的差异，以及每个位置在基础系统处理器上的来宾 Vm 中的工作方式。

### <a name="the-classic-scheduler"></a>经典计划程序

经典计划程序是一种公平共享的循环法，用于计划系统中的虚拟处理器（VPs）上的工作，包括 root VPs 以及属于来宾 Vm 的 VPs。 经典计划程序是在所有版本的 Hyper-v 上使用的默认计划程序类型（在 Windows Server 2019 之前，如此处所述）。  经典计划程序的性能特征非常了解，并演示了经典计划程序来 ably 支持多个工作负荷，即，主机的 VP： LP 比率按合理的边距进行订阅（取决于要虚拟化的工作负荷的类型、总体资源利用率等。

在启用了 SMT 的虚拟化主机上运行时，经典计划程序将从属于核心的每个 SMT 线程上的任何 VM 计划来宾 VPs。 因此，不同的 Vm 可以同时在同一内核上运行（一个 VM 运行在一个核心线程上，另一个 VM 在另一个线程上运行）。

### <a name="the-core-scheduler"></a>核心计划程序

核心计划程序利用 SMT 的属性来提供来宾工作负荷的隔离，这会影响安全和系统性能。 核心计划程序可确保 VM 上的 VPs 在同级 SMT 线程上计划。 这是以对称方式完成的，因此，如果 LPs 分为两组，则会按两组计划 VPs，而不会在 Vm 之间共享系统 CPU 内核。

通过在基础 SMT 对上计划来宾 VPs，核心计划程序为工作负荷隔离提供强大的安全边界，还可用于减少延迟敏感工作负荷的性能变化。

请注意，在未启用 SMT 的情况下为虚拟机计划副总裁后，该副总裁将在运行时使用整个内核，并且核心的同级 SMT 线程将处于空闲状态。  这是提供正确的工作负荷隔离所必需的，但会影响系统的整体性能，尤其是在系统 LPs 的情况下，这是因为总 VP： LP 比率超过1:1。 因此，每个核心配置为不带多个线程的来宾 Vm 都是一项最佳配置。

### <a name="benefits-of-the-using-the-core-scheduler"></a>使用核心计划程序的优点

核心计划程序具有以下优势：

* 来宾工作负荷隔离的强大安全边界-来宾 VPs 被限制为在底层物理内核对上运行，从而降低了向端通道侦听攻击的漏洞。

* 减少工作负荷变化-来宾工作负荷吞吐量变化大大降低，从而提供更高的工作负荷一致性。

* 在来宾 Vm 中使用 SMT-在来宾虚拟机中运行的操作系统和应用程序可以利用 SMT 行为和编程接口（Api）来跨 SMT 线程控制和分配工作，就像运行非虚拟化时的情况一样。

核心计划程序当前在 Azure 虚拟化主机上使用，专门用于充分利用强安全边界和低工作负荷 variabilty。 Microsoft 认为核心计划程序类型应为，并将继续作为大多数虚拟化方案的默认虚拟机监控程序计划类型。  因此，为了确保客户在默认情况下是安全的，Microsoft 现在正在对 Windows Server 2019 做出此更改。

### <a name="core-scheduler-performance-impacts-on-guest-workloads"></a>对来宾工作负荷的核心计划程序性能影响

尽管需要有效地减少某些类的漏洞，但核心计划程序可能也会降低性能。 客户可能会发现其 Vm 的性能特征存在差异，并影响其虚拟化主机的总体工作负荷容量。 如果核心计划程序必须运行非 SMT 副总裁，则基础逻辑核心中仅会执行其中一个指令流，而另一个必须处于空闲状态。 这将限制来宾工作负荷的主机总容量。

按照本文档中的部署指南，可以最大程度地降低这些性能影响。 主机管理员必须认真考虑其特定的虚拟化部署方案，并在安全风险的容错范围内权衡最大工作负载密度、虚拟化主机的过度合并等需求。

## <a name="changes-to-the-default-and-recommended-configurations-for-windows-server-2016-and-windows-server-2019"></a>Windows Server 2016 和 Windows Server 2019 的默认配置和推荐配置更改

部署具有最大安全状况的 Hyper-v 主机要求使用虚拟机监控程序核心计划程序类型。 为了确保客户在默认情况下是安全的，Microsoft 更改了以下默认设置和推荐设置。

>[!NOTE]
>尽管 Windows Server 2016、Windows Server 1709 和 Windows Server 1803 的初始版本中包含了虚拟机监控程序对计划程序类型的内部支持，但需要进行更新才能访问配置控件，该控件允许选择虚拟机监控程序计划程序类型。  有关这些更新的详细信息，请参阅[了解和使用 hyper-v 虚拟机监控程序计划程序类型](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types)。

### <a name="virtualization-host-changes"></a>虚拟化主机更改

* 默认情况下，虚拟机监控程序将使用内核计划程序，从 Windows Server 2019 开始。

* Microsoft reccommends 在 Windows Server 2016 上配置核心计划程序。 Windows Server 2016 支持虚拟机监控程序核心计划程序类型，但默认值为经典计划程序。 核心计划程序是可选的，并且必须由 Hyper-v 主机管理员显式启用。

### <a name="virtual-machine-configuration-changes"></a>虚拟机配置更改

* 在 Windows Server 2019 上，使用默认 VM 版本9.0 创建的新虚拟机将自动继承虚拟化主机的 SMT 属性（启用或禁用）。 也就是说，如果在物理主机上启用了 SMT，则在默认情况下，新创建的 Vm 还将启用 SMT，并将继承主机的 SMT 拓扑，并且 VM 的硬件线程数与基础系统的每个内核数量相同。 这将反映在 VM 的配置中，HwThreadCountPerCore = 0，其中0表示 VM 应继承主机的 SMT 设置。

* VM 版本为8.2 或更低的现有虚拟机将保留其原始 VM 处理器设置用于 HwThreadCountPerCore，8.2 VM 版本来宾的默认值为 HwThreadCountPerCore = 1。 当这些来宾在 Windows Server 2019 主机上运行时，它们将被视为如下：

    1. 如果 VM 的 VP 计数小于或等于 LP 内核计数，则核心计划程序会将该 VM 视为非 SMT VM。 当来宾 VP 在单个 SMT 线程上运行时，核心的同级 SMT 线程将为空闲。 这不是最佳的，将导致总体性能损失。

    2. 如果 VM 具有比 LP 内核更多的 VPs，则核心计划程序会将 VM 视为 SMT VM。 但是，VM 不会观察到它是 SMT VM 的其他指示。 例如，使用 CPUID 指令或 Windows Api 来查询 CPU 拓扑的操作系统或应用程序将不会指示启用了 SMT。

* 如果现有 VM 从更低版本 VM 版本显式更新到版本9.0，则该 VM 将保留其当前的 HwThreadCountPerCore 值。  VM 不会启用 SMT。

* 在 Windows Server 2016 上，Microsoft 建议为来宾 Vm 启用 SMT。  默认情况下，在 Windows Server 2016 上创建的 Vm 将禁用 SMT，除非显式更改，否则 HwThreadCountPerCore 设置为1。

>[!NOTE]
>Windows Server 2016 不支持将 HwThreadCountPerCore 设置为0。

#### <a name="managing-virtual-machine-smt-configuration"></a>管理虚拟机 SMT 配置

基于每个 VM 设置来宾虚拟机 SMT 配置。 主机管理员可以检查并配置 VM 的 SMT 配置，从以下选项中进行选择：

    1. 将 Vm 配置为以 SMT 身份运行，可选择自动继承 host SMT 拓扑

    2. 将 Vm 配置为以非 SMT 方式运行

VM 的 SMT 配置显示在 Hyper-v 管理器控制台的 "摘要" 窗格中。  可以使用 VM 设置或 PowerShell 来配置 VM 的 SMT 设置。

#### <a name="configuring-vm-smt-settings-using-powershell"></a>使用 PowerShell 配置 VM SMT 设置

若要为来宾虚拟机配置 SMT 设置，请打开具有足够权限的 PowerShell 窗口，并键入：

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

其中：

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

若要读取来宾虚拟机的 SMT 设置，请打开具有足够权限的 PowerShell 窗口，然后键入：

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

请注意，配置了 HwThreadCountPerCore = 0 的来宾 Vm 指示将为来宾启用 SMT，并将相同数量的 SMT 线程公开给来宾，因为它们位于基础虚拟化主机上（通常为2）。

### <a name="guest-vms-may-observe-changes-to-cpu-topology-across-vm-mobility-scenarios"></a>来宾 Vm 可能会观察到跨 VM 移动方案的 CPU 拓扑变化

VM 中的 OS 和应用程序可能会在 VM 生命周期事件（如实时迁移或保存和还原操作）前后更改主机和 VM 设置。 在保存和还原 VM 状态的操作过程中，VM 的 HwThreadCountPerCore 设置和实现的值（即 VM 设置和源主机配置的计算组合）都将被迁移。 VM 将继续在目标主机上通过这些设置运行。 VM 关闭并重新启动时，VM 观测到的可用值可能会改变。 这应该是良性的，因为 OS 和应用程序层软件应在其正常启动和初始化代码流中查找 CPU 拓扑信息。 但是，由于在实时迁移或保存/还原操作过程中，将跳过这些启动时间初始化序列，因此，在关闭并重新启动之前，执行这些状态转换的 Vm 可能会观察到最初计算得出的已实现值。  

### <a name="alerts-regarding-non-optimal-vm-configurations"></a>有关非最佳 VM 配置的警报

使用比主机上的物理内核数更多的 VPs 配置的虚拟机将导致不是最佳配置。 虚拟机监控程序计划程序会将这些 Vm 视为 SMT。 但是，VM 中的 OS 和应用程序软件将显示一个 CPU 拓扑，显示 SMT 已禁用。 检测到此条件时，Hyper-v 工作进程将在虚拟化主机上记录一个事件，警告主机管理员 VM 的配置不是最佳的，并建议为 VM 启用 SMT。

#### <a name="how-to-identify-non-optimally-configured-vms"></a>如何识别非优化配置的 Vm

你可以通过检查 Hyper-v 工作进程事件 ID 3498 事件查看器中的系统日志来识别非 SMT Vm，只要 VM 中的 VPs 数量大于物理核心计数，就会对 VM 触发此事件。 可以从事件查看器或通过 PowerShell 获取工作进程事件。

#### <a name="querying-the-hyper-v-worker-process-vm-event-using-powershell"></a>使用 PowerShell 查询 Hyper-v 工作进程 VM 事件

若要使用 PowerShell 查询 Hyper-v 工作进程事件 ID 3498，请在 PowerShell 提示符下输入以下命令。

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### <a name="impacts-of-guest-smt-configuaration-on-the-use-of-hypervisor-enlightenments-for-guest-operating-systems"></a>来宾 SMT 配置对来宾操作系统的使用虚拟机监控程序自旋的影响

Microsoft 虚拟机管理程序提供了多个自旋或提示，在来宾 VM 中运行的 OS 可以查询并使用来触发优化，例如那些可能会提高性能或在运行时提高各种条件处理的方式虚拟. 最近引入的悟道涉及处理虚拟处理器计划，并对利用 SMT 的侧通道攻击使用 OS 缓解。

>[!NOTE]
>Microsoft 建议主机管理员为来宾 Vm 启用 SMT，以优化工作负荷性能。

下面提供了此来宾悟道的详细信息，但虚拟化主机管理员的关键要点在于是虚拟机应将 HwThreadCountPerCore 配置为与主机的物理 SMT 配置匹配。 这允许虚拟机监控程序报告不存在非结构核心共享。 因此，可能会启用任何需要悟道的支持优化的来宾 OS。 在 Windows Server 2019 上，创建新的 Vm，并保留默认值 HwThreadCountPerCore （0）。 从 Windows Server 2016 主机迁移的旧 Vm 可更新为 Windows Server 2019 配置版本。 完成此操作后，Microsoft 建议设置 HwThreadCountPerCore = 0。  在 Windows Server 2016 上，Microsoft 建议将 HwThreadCountPerCore 设置为与主机配置匹配（通常为2）。

### <a name="nononarchitecturalcoresharing-enlightenment-details"></a>NoNonArchitecturalCoreSharing 悟道详细信息

从 Windows Server 2016 开始，虚拟机监控程序定义了一个新的悟道，用于描述如何处理来宾操作系统的副总裁计划和位置。 此悟道在[虚拟机监控程序顶级功能规范 v 5.0 c](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs)中定义。

虚拟机监控程序合成 CPUID 叶0x40000004： 18 [NoNonArchitecturalCoreSharing = 1] 表示虚拟处理器将永远不会与另一个虚拟处理器共享物理内核，并将报告为同级 SMT 的虚拟处理器除外线程. 例如，来宾副总裁决不会在 SMT 线程上运行，同时在同一处理器核心上的同级 SMT 线程上同时运行根副总裁。 仅当运行虚拟化时，这种情况才是可能的，因此表示一个也有严重安全隐患的非体系结构 SMT 行为。 来宾操作系统可以使用 NoNonArchitecturalCoreSharing = 1 作为安全启用优化的指示，这可能有助于避免设置 STIBP 的性能开销。

在某些配置中，虚拟机监控程序不会指示 NoNonArchitecturalCoreSharing = 1。 例如，如果主机已启用 SMT，并且配置为使用虚拟机监控程序经典计划程序，则 NoNonArchitecturalCoreSharing 将为0。 这可能会阻止启用来宾启用某些优化。 因此，Microsoft 建议使用 SMT 的主机管理员依赖于虚拟机监控程序核心计划程序并确保将虚拟机配置为从主机继承其 SMT 配置，以确保最佳工作负荷性能。

## <a name="summary"></a>总结

安全威胁的发展不断发展。 为了确保客户在默认情况下是安全的，Microsoft 正在更改从 Windows Server 2019 Hyper-v 开始的虚拟机监控程序和虚拟机的默认配置，并为运行 Windows 的客户提供更新的指导和建议服务器 2016 Hyper-v。 虚拟化主机管理员应：

* 阅读并了解本文档中提供的指南

* 仔细评估并调整其虚拟化部署，确保它们满足其独特要求的安全性、性能、虚拟化密度和工作负荷响应目标

* 请考虑重新配置现有 Windows Server 2016 Hyper-v 主机，以利用虚拟机监控程序核心计划程序提供的强大安全权益

* 更新现有的非 SMT Vm，以降低用于解决硬件安全漏洞的副总裁隔离施加的计划约束的性能影响
