---
title: 有关 HYPER-V 虚拟机监控程序计划程序类型选择
description: 提供有关 HYPER-V 的调度程序的使用模式的信息适用于 HYPER-V 主机管理员
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: c5360d8e2fdc23f8b05c6be0f665407eebedeba2
ms.sourcegitcommit: 546229d6b5fa7e16f725c6c35f4dcc272711b811
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2018
ms.locfileid: "4905124"
---
# 有关 HYPER-V 虚拟机监控程序计划程序类型选择

适用于：

* Windows Server 2016
* Windows Server 版本 1709
* Windows Server 版本 1803
* Windows Server 2019

本文档介绍了为 HYPER-V 的默认值的重要更改，并建议的虚拟机监控程序使用调度程序类型。 这些更改对这两个系统安全和虚拟化性能影响。 虚拟化主机管理员应检查和了解的更改和本文档中所述的含义和仔细评估影响、 建议的部署指南和风险因素涉及可以最好地了解如何部署和管理在快速变化的安全环境面临的 HYPER-V 主机。

>[!IMPORTANT]
>当前已知侧信道漏洞视力正常在多个处理器体系结构中的漏洞通过在具有同时进行的主机上运行时的传统的虚拟机监控程序经典计划程序类型的计划行为恶意来宾虚拟机的安全启用多线程 (SMT)。  成功利用，如果是恶意的工作负荷可以观察数据及其分区边界外。 可以通过配置 HYPER-V 虚拟机监控程序以充分利用虚拟机监控程序核心计划程序类型和重新配置来宾虚拟机缓解此类攻击。 与核心计划程序，虚拟机监控程序限制来宾虚拟机的 Vp 相同的物理处理器核心，因此强烈隔离到运行它的物理核心边界访问数据虚拟机的能力上运行。  这是一种高效缓解对这些侧信道攻击，这将阻止 VM 是否观察从其他分区，任何项目根或另一个来宾分区。  因此，Microsoft 更改默认值，并推荐用于虚拟化主机和来宾 Vm 的配置设置。

## Background

从 Windows Server 2016 开始，HYPER-V 支持计划和管理虚拟处理器，称为虚拟机监控程序调度程序类型的几种方法。  [了解和使用 HYPER-V 虚拟机监控程序调度程序类型](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types)中找不到的所有虚拟机监控程序调度程序类型的详细的说明。

>[!NOTE]
>新虚拟机监控程序调度程序类型已首次引入 Windows Server 2016 中，并在以前的版本中不可用。 HYPER-V 在 Windows Server 2016 之前的所有版本都支持仅经典调度程序。 仅最近发布的核心计划的支持。

## 有关虚拟机监控程序调度程序类型

本文主要介绍特定于使用传统的"传统"计划程序，与新虚拟机监控程序核心计划程序类型和这些调度程序类型如何与使用对称多线程或 SMT 相交。  请务必了解核心和经典调度程序和如何在基础系统处理器上放置从来宾虚拟机的工作，每个之间的差异。

### 经典计划程序

经典调度程序引用的计划整个系统-包括根 Vp 以及 Vp 属于来宾虚拟机的虚拟处理器 (Vp) 上的工作的公平共享轮循它方法。 经典计划程序已被 （之前 Windows Server 2019，此处所述)，使用所有版本的 HYPER-V 上的默认计划程序类型。  也可以理解经典调度程序的性能特征，，经典计划程序进行了演示 ably 支持的工作负载的主机的 VP:LP 比通过合理的边距，即过度订阅过度订阅 (具体取决于类型被虚拟化的工作负荷、 总体资源使用量等。）。

当与 SMT 启用虚拟化主机上运行，经典计划程序将计划从独立属于核心每个 SMT 线程上的任何 VM 来宾 Vp。 因此，不同的虚拟机可以运行相同的核心上在同一时间 (在其他线程上运行另一台 VM 时，在一个线程的核心上运行一个 VM)。

### 核心计划程序

核心计划程序利用 SMT 提供隔离的来宾工作负载，这将影响安全性和系统性能的属性。 核心计划程序可确保在同级 SMT 线程上计划的 Vp 从 VM。 这是对称，以便如果 Lp 在计划的两个，Vp 组的组中的第二个，并且系统 CPU 核心永远不会共享 Vm 之间。

通过计划基础 SMT 对来宾 Vp，核心调度程序提供的工作负荷隔离强安全边界，并且还可用于减少延迟敏感的工作负荷的性能可变性。

请注意，当没有 SMT 启用，虚拟机计划副总裁副总裁将使用的所有核心，它运行，且在核心同级 SMT 线程将处于空闲状态时。  这需要提供正确的工作负荷隔离，但会影响整个系统性能，尤其是当系统 Lp 成为过度订阅-即，当总 VP:LP 比超过 1:1。 因此，运行来宾虚拟机配置每个核心的多个线程不是子最佳配置。

### 使用核心计划的优点

核心调度程序提供以下优势：

* 被限制为来宾的工作负荷隔离的来宾 Vp 强安全边界，以在基础物理核心对，减少对搜索攻击侧信道漏洞上运行。

* 显著降低减少的工作负荷可变性-来宾的工作负荷吞吐量可变性，提供更好地工作负荷一致性。

* 在来宾虚拟机的操作系统和来宾虚拟机中运行的应用程序中使用 SMT 可以利用 SMT 行为和程序编程接口 (Api) 以控制和跨 SMT 线程分配工作，就像它们时将运行非虚拟化。

核心计划程序当前在 Azure 虚拟化主机上专门用来充分利用强大的安全边界和低的工作负荷 variabilty。 Microsoft 认为核心计划程序类型应该并且将继续处于默认虚拟机监控程序计划虚拟化方案的大部分的类型。  因此，若要确保我们的客户安全默认情况下，Microsoft 进行此更改为 Windows Server 2019 现在。

### 核心上来宾工作负荷的计划程序性能影响

而是必需的有效地缓解某些类漏洞，核心计划程序可能会也可能降低性能。 客户可能会看到较使用其虚拟机和影响的性能特征到其虚拟化主机的总体的工作负荷能力的差异。 在核心计划程序必须运行非 SMT VP 的位置的情况下，仅有一个在基础逻辑核心中的指令流执行，而其他必须保留处于空闲状态。 这将限制为来宾工作负荷的总主机容量。

可以通过遵循此文档中的部署指南最小化这些性能影响。 主机管理员必须仔细考虑部署方案使用其特定虚拟化，并平衡的安全风险需最大的工作负荷密度、 虚拟化主机等的过度整合针对其容差。

## 默认和 Windows Server 2016 和 Windows Server 2019 的推荐的配置更改

部署具有最大安全状况的 HYPER-V 主机需要使用虚拟机监控程序核心计划程序类型。 若要确保我们的客户安全默认情况下，Microsoft 更改以下默认值，并推荐的设置。

>[!NOTE]
>时对调度程序类型的虚拟机监控程序的内部支持已包含 Windows Server 2016、 Windows Server 1709 和 Windows Server 1803 的初始版本中，存在才能访问配置控件允许选择所需的更新虚拟机监控程序计划程序类型。  请有关这些更新的详细信息参阅[了解](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types)和使用 HYPER-V 虚拟机监控程序调度程序类型。

### 虚拟化主机更改

* 虚拟机监控程序将使用通过 Windows Server 2019 的默认开头的核心调度程序。

* Microsoft Windows Server 2016 上配置的核心计划程序 reccommends。 虚拟机监控程序核心计划程序类型支持在 Windows Server 2016 中，但默认值是经典计划程序。 核心计划程序是可选的必须在显式启用 HYPER-V 主机管理员。

### 虚拟机配置更改

* 在 Windows Server 2019，使用默认 VM 版本 9.0 创建新的虚拟机将自动继承 SMT 的属性 （启用或禁用） 的虚拟化主机。 即如果上启用了 SMT 物理主机，新创建的虚拟机还将启用，SMT，并将继承的主机 SMT 拓扑默认情况下，与具有相同数量的每个核心的硬件线程作为基础系统 VM。 这将反映在虚拟机的配置与 HwThreadCountPerCore = 的 0，其中 0 表示 VM 应当继承主机的 SMT 设置。

* 现有的虚拟机的 VM 版本的 8.2 或更早版本将保留 HwThreadCountPerCore，其原始 VM 处理器设置和 8.2 VM 版本来宾的默认值是 HwThreadCountPerCore = 1。 当这些来宾一个主机上运行 Windows Server 2019 时，它们将视为，如下所示：

    1. 如果 VM 具有副总裁计数小于或等于的 LP 核心计数，VM 时被视为作为非 SMT 虚拟机的核心计划程序。 来宾副总裁上运行时单个 SMT 线程，将 idled 的核心同级 SMT 线程。 这是最佳，并且将导致整体性能损失。

    2. 如果 VM 具有多个 Vp 比 LP 内核，VM 将被视为 SMT VM 核心调度程序。 但是，VM 将不遵循它是 SMT VM 其他指示。 例如，使用要查询 CPU 拓扑由操作系统或应用程序的 CPUID 指令或 Windows Api 将不会指示 SMT 已启用。

* 当现有的虚拟机显式从 eariler VM 版本更新到版本 9.0 通过更新 VM 操作时，VM 将为 HwThreadCountPerCore 保留其当前值。  VM 将没有 SMT 强制启用。

* 在 Windows Server 2016 中，Microsoft 建议启用 SMT 适用于来宾虚拟机。  默认情况下，将禁用 SMT，在 Windows Server 2016 上创建的虚拟机的是 HwThreadCountPerCore 设置为 1，除非显式更改。

>[!NOTE]
>Windows Server 2016 不支持 HwThreadCountPerCore 设置为 0。

#### 管理虚拟机 SMT 配置

在每台 VM 的基础上设置的来宾虚拟机 SMT 配置。 主机管理员可以检查和配置虚拟机的 SMT 配置，以选择以下选项之一：

    1. 配置 Vm 运行为 SMT 启用的可以选择自动继承主机 SMT 拓扑

    2. 配置非 SMT 作为运行的 Vm

Vm SMT 配置显示在 HYPER-V 管理器控制台在摘要窗格中。  可以通过使用虚拟机设置或 PowerShell 配置虚拟机的 SMT 设置。

#### 使用 PowerShell 配置 VM SMT 设置

若要配置来宾虚拟机的 SMT 设置，请使用足够的权限，然后键入打开 PowerShell 窗口：

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

其中：

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

若要读取来宾虚拟机的 SMT 设置，请使用足够的权限，然后键入打开 PowerShell 窗口：

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

请注意该来宾虚拟机配置 HwThreadCountPerCore = 0 指示 SMT 将为来宾中，启用，在基础虚拟化主机上，通常 2，因为他们将公开到来宾 SMT 线程的相同数量。

### 来宾虚拟机可能通过虚拟机移动方案观察对 CPU 拓扑的更改

操作系统和应用程序在 VM 中的可能会看到对主机和虚拟机设置之前和更改后 VM 生命周期事件如实时迁移或保存和还原操作。 在操作期间哪些虚拟机中保存和还原状态，迁移的虚拟机 HwThreadCountPerCore 设置并已实现的值 （即，虚拟机的设置和源主机配置的计算组合）。 VM 将继续使用这些设置目标主机上的运行。 此时虚拟机已关闭并重新启动，将更改可能已实现的值观测到 vm。 这应该是良性，作为操作系统和应用程序层软件应当寻找 CPU 拓扑信息作为其正常的启动和初始化代码流程的一部分。 但是，因为这些启动时初始化序列将跳过实时迁移或保存/还原在操作期间，经过这些状态转换的 Vm 可以观察最初计算实现值，直到它们关闭并重新启动。  

### 有关非最佳的虚拟机配置的警报

使用多个 Vp 比物理核心上有主机结果非最佳配置中配置的虚拟机。 虚拟机监控程序计划程序会将这些虚拟机，就像它们是 SMT 感知。 但是，OS 和 VM 中的应用程序软件将看到显示 SMT 处于禁用状态的 CPU 拓扑。 HYPER-V 工作进程检测到这种情况时，会记录事件，虚拟机的配置是非最佳和推荐 SMT 可警告主机管理员在虚拟化主机上启用的虚拟机。

#### 如何识别非最佳方式配置 Vm

你可以确定非-SMT Vm 通过 HYPER-V 工作进程事件 ID 3498 检查系统日志事件查看器中将触发虚拟机中，每当 VM 中 Vp 数大于物理核心计数。 从事件查看器，或通过 PowerShell，可以获得工作进程事件。

#### 查询使用 PowerShell 的 HYPER-V 工作进程 VM 事件

查询对于 HYPER-V 工作进程事件 ID 3498 使用 PowerShell，输入以下命令在 PowerShell 提示符下。

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### 使用来宾操作系统的虚拟机监控程序 enlightenments 来宾 SMT 配置的影响

Microsoft 虚拟机监控程序提供多个 enlightenments 或提示，这在来宾虚拟机中运行的操作系统可能查询并使用触发优化，如那些可能受益性能或否则改进对的各种条件处理运行时虚拟化。 一个最近引入的启发涉及处理的虚拟处理器计划和操作系统缓解用于攻击 SMT 侧信道攻击。

>[!NOTE]
>Microsoft 建议主机管理员启用 SMT 来宾虚拟机进行优化的工作负荷性能。

提供在下面，但是关键的虚拟化主机管理员是虚拟机应具有 HwThreadCountPerCore 配置为主机的物理 SMT 配置匹配，此来宾启发的详细信息。 这允许虚拟机监控程序报告，如果没有任何非体系结构的核心共享。 因此，需要启发任何来宾操作系统支持优化可能会启用。 在 Windows Server 2019，创建新的 Vm 和保留 HwThreadCountPerCore (0) 的默认值。 从 Windows Server 2016 迁移较旧的 Vm 主机可以更新到 Windows Server 2019 的配置版本。 执行此操作，Microsoft 建议设置 HwThreadCountPerCore 后 = 0。  在 Windows Server 2016 中，Microsoft 建议设置 HwThreadCountPerCore 以匹配主机配置 (通常为 2)。

### NoNonArchitecturalCoreSharing 启发详细信息

从 Windows Server 2016 开始，虚拟机监控程序定义来描述其处理副总裁计划和放置到来宾操作系统的新启发。 此启发是[虚拟机监控程序顶部级别 Functional Specification v5.0c](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs)中定义的。

虚拟机监控程序综合 CPUID 叶 CPUID.0x40000004.EAX:18[NoNonArchitecturalCoreSharing = 1] 表示虚拟处理器永远不会将与另一个虚拟处理器，除了报告为同级 SMT 的虚拟处理器共享物理核心线程。 例如，来宾副总裁将永远不会运行旁边根副总裁 SMT 线程上同时在同级在相同的处理器核心上 SMT 线程上运行。 这种情况时，才可以运行虚拟化，并因此表示还有严重的安全含义的非体系结构 SMT 行为。 来宾操作系统可以使用 NoNonArchitecturalCoreSharing = 1，则可以安全地启用优化，这可能有助于避免设置 STIBP 的性能开销它用于指示。

在某些配置中，虚拟机监控程序将不会指示该 NoNonArchitecturalCoreSharing = 1。 例如，如果主机已启用的 SMT 并且配置为使用虚拟机监控程序经典 scheduler，NoNonArchitecturalCoreSharing 将为 0。 这可能会阻止启用某些优化启发式的来宾。 因此，Microsoft 建议使用 SMT 主机管理员依赖于虚拟机监控程序核心计划程序，并确保虚拟机配置其 SMT 配置继承主机以确保最佳的工作负荷的性能。

## 摘要

安全威胁环境不断发展。 若要确保我们的客户安全默认情况下，Microsoft 已更改的默认配置为虚拟机监控程序和启动在 Windows Server 2019 HYPER-V，虚拟机和指南和建议，运行 Windows 的客户提供更新Server 2016 Hyper-v。 虚拟化主机管理员应该：

* 阅读和了解本文档中提供的指南

* 仔细评估并调整其虚拟化部署，以确保它们满足安全性、 性能、 虚拟化密度和其独特要求的工作负荷响应目标

* 请考虑重新配置现有的 Windows Server 2016 HYPER-V 主机能够利用虚拟机监控程序核心调度程序提供的强大的安全优势

* 更新现有非-SMT Vm 调度由地址硬件的安全漏洞的副总裁隔离强制实施的限制从减少性能影响
