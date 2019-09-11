---
title: 了解和使用 Hyper-v 虚拟机监控程序计划程序类型
description: 提供有关 hyper-v 主机管理员使用 Hyper-v 计划程序模式的信息
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: c7c2de8354d067faf0dcf1787c3e178421e2ac03
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872027"
---
# <a name="managing-hyper-v-hypervisor-scheduler-types"></a>管理 Hyper-v 虚拟机监控程序计划程序类型

>适用于：Windows 10，Windows Server 2016，Windows Server，版本1709，Windows Server，版本1803，Windows Server 2019

本文介绍了在 Windows Server 2016 中首次引入的虚拟处理器计划逻辑的新模式。 这些模式或计划程序类型确定 Hyper-v 虚拟机监控程序如何分配并管理来宾虚拟处理器上的工作。 Hyper-v 主机管理员可以选择最适合来宾虚拟机（Vm）的虚拟机监控程序计划程序类型，并将 Vm 配置为利用计划逻辑。

>[!NOTE]
>若要使用本文档中所述的虚拟机监控程序计划程序功能，需要进行更新。 有关详细信息，请参阅[所需更新](#required-updates)。

## <a name="background"></a>后台

在从 Hyper-v 虚拟处理器计划后面讨论逻辑和控件之前，请查看本文中所述的基本概念，这会很有帮助。

### <a name="understanding-smt"></a>了解 SMT

同时多线程处理（SMT）是新式处理器设计中利用的一种技术，它允许独立的独立执行线程共享处理器的资源。 SMT 通常通过并行化计算为大多数工作负荷提供适度的性能提升，但如果在两个线程之间发生争用，共享处理器资源出现。
Intel 和 AMD 均提供支持 SMT 的处理器。 Intel 将其 SMT 的产品/服务称为 Intel 超线程技术或 Intel HT。

在本文中，Hyper-v 的 SMT 说明及其使用方式同样适用于 Intel 和 AMD 系统。

* 有关 Intel HT 技术的详细信息，请参阅[Intel 超线程技术](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* 有关 AMD SMT 的详细信息，请参阅["Zen" 核心体系结构](https://www.amd.com/en/technologies/zen-core)

## <a name="understanding-how-hyper-v-virtualizes-processors"></a>了解 Hyper-v 虚拟化处理器的方式

在考虑虚拟机监控程序计划程序类型之前，了解 Hyper-v 体系结构也很有帮助。 可在[Hyper-v 技术概述](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview)中找到一般摘要。 下面是本文的重要概念：

* Hyper-v 创建和管理虚拟机分区，在这些分区中分配和共享计算资源，以控制虚拟机监控程序。 分区在所有来宾虚拟机之间以及来宾 Vm 与根分区之间提供强大的隔离边界。

* 根分区本身是虚拟机分区，尽管它具有唯一的属性和比来宾虚拟机更高的特权。 根分区提供控制所有来宾虚拟机的管理服务，为来宾提供虚拟设备支持，并管理来宾虚拟机的所有设备 i/o。 Microsoft 强烈建议不要在根分区中运行任何应用程序工作负荷。

* 根分区的每个虚拟处理器（副总裁）映射到基础逻辑处理器（LP）1:1。 主机副总裁始终在同一基础 LP 上运行–不会迁移根分区的 VPs。

* 默认情况下，主机 VPs 运行的 LPs 还可以运行来宾 VPs。

* 来宾副总裁可以安排在任何可用的逻辑处理器上运行的虚拟机监控程序。 虽然虚拟机监控程序计划程序在计划来宾副总裁时，请注意考虑时态缓存区域、NUMA 拓扑和许多其他因素，最终可以在任何主机 LP 上计划副总裁。

## <a name="hypervisor-scheduler-types"></a>虚拟机监控程序计划程序类型

从 Windows Server 2016 开始，Hyper-v 虚拟机监控程序支持多个计划程序逻辑模式，确定虚拟机监控程序如何计划底层逻辑处理器上的虚拟处理器。 这些计划程序类型为：

- [经典、公平共享计划程序](#the-classic-scheduler)
- [核心计划程序](#the-core-scheduler)
- [根计划程序](#the-root-scheduler)

### <a name="the-classic-scheduler"></a>经典计划程序

经典计划程序是自其开始后的所有版本 Windows Hyper-v 虚拟机监控程序（包括 Windows Server 2016 Hyper-v）的默认值。 经典计划程序为来宾虚拟处理器提供公平共享的抢先式循环计划模型。

经典计划程序类型最适合大多数传统 Hyper-v 使用–适用于私有云、托管提供程序等。 性能特征非常了解并经过优化，可支持多种虚拟化方案，例如，VPs 到 LPs 的过度订阅、同时运行多个异类 Vm 和工作负荷、运行更大的高缩放性性能 Vm，支持无限制的 Hyper-v 的全部功能集，等等。

### <a name="the-core-scheduler"></a>核心计划程序

虚拟机监控程序核心计划程序是在 Windows Server 2016 和 Windows 10 版本1607中引入的经典计划程序逻辑的新替代方案。 核心计划程序为来宾工作负荷隔离提供强大的安全边界，降低了在启用 SMT 的虚拟化主机上运行的 Vm 内的工作负荷的性能变化。 核心计划程序允许同时在启用了同一 SMT 的虚拟化主机上同时运行 SMT 和非 SMT 虚拟机。

核心计划程序利用虚拟化主机的 SMT 拓扑，并选择性地向来宾虚拟机公开 SMT 对，并将同一虚拟机中的来宾虚拟处理器组计划到 SMT 逻辑处理器组。 这是以对称方式完成的，因此，如果 LPs 分为两组，则会按两组计划 VPs，而不会在 Vm 之间共享核心。
如果为未启用 SMT 的虚拟机计划副总裁，该副总裁将在运行时使用整个核心。

核心计划程序的总体结果是：

* 来宾 VPs 被限制为在基础物理内核对上运行，将 VM 隔离到处理器核心边界，从而减少了恶意 Vm 的端通道侦听攻击的漏洞。

* 吞吐量变化大大降低。

* 性能可能会降低，因为如果只能运行一组 VPs 中的一个，那么在另一个中，只会执行核心中的一个指令流，而另一个则会处于空闲状态。

* 在来宾虚拟机中运行的操作系统和应用程序可以利用 SMT 行为和编程接口（Api）跨 SMT 线程控制和分配工作，就像在运行非虚拟化时一样。

* 来宾工作负荷隔离的强大安全边界-来宾 VPs 被限制为在底层物理内核对上运行，从而降低了向端通道侦听攻击的漏洞。

默认情况下，将使用核心计划程序，从 Windows Server 2019 开始。 在 Windows Server 2016 上，核心计划程序是可选的，并且必须由 Hyper-v 主机管理员显式启用，而经典计划程序是默认值。

#### <a name="core-scheduler-behavior-with-host-smt-disabled"></a>禁用了主机 SMT 的核心计划程序行为

如果虚拟机监控程序配置为使用核心计划程序类型，但 SMT 功能在虚拟化主机上被禁用或不存在，则虚拟机监控程序将使用经典计划程序行为，而不考虑 "虚拟机监控程序计划程序类型" 设置。

### <a name="the-root-scheduler"></a>根计划程序

Windows 10 版本1803引入了根计划程序。 启用根计划程序类型后，虚拟机监控程序将工作计划移交给控制到根分区。 根分区的 OS 实例中的 NT 计划程序管理将工作计划给系统 LPs 的所有方面。

根计划程序解决了支持实用工具分区以提供强大的工作负荷隔离（与 Windows Defender 应用程序防护一起使用）（WDAG）所固有的独特要求。 在这种情况下，将计划责任留给根 OS 具有几个优点。 例如，适用于容器方案的 CPU 资源控制可与实用工具分区结合使用，从而简化了管理和部署。 此外，根操作系统计划程序可以很容易地收集容器内有关工作负荷 CPU 使用率的指标，并使用此数据作为输入，以适用于系统中所有其他工作负荷的相同计划策略。 这些相同的指标还有助于清楚地将应用程序容器中完成的工作应用到主机系统。 对于传统的虚拟机工作负荷，跟踪这些指标会更难，在这种情况下，代表所有正在运行的 VM 的一些工作会在根分区中发生。

#### <a name="root-scheduler-use-on-client-systems"></a>根计划程序在客户端系统上使用

从 Windows 10 版本1803开始，默认情况下，仅在客户端系统上使用根计划程序，在这种情况下，可以在支持基于虚拟化的安全和 WDAG 工作负荷隔离的情况下启用虚拟机监控程序，并使用异类内核体系结构。 这是客户端系统唯一支持的虚拟机监控程序计划程序配置。 管理员不应尝试替代 Windows 10 客户端系统上的默认虚拟机监控程序计划程序类型。

#### <a name="virtual-machine-cpu-resource-controls-and-the-root-scheduler"></a>虚拟机 CPU 资源控件和根计划程序

当启用虚拟机监控程序根计划程序时，Hyper-v 提供的虚拟机处理器资源控件不受支持，因为根操作系统的计划程序逻辑是全局管理主机资源，不知道 VM 的特定的配置设置。 Hyper-v 每个虚拟机处理器资源控制（如大写字母、权重和准备金）仅适用于虚拟机监控程序直接控制副总裁计划的情况，例如经典计划和核心计划程序类型。

#### <a name="root-scheduler-use-on-server-systems"></a>根计划程序在服务器系统上使用

此时，不建议将根计划程序与服务器上的 Hyper-v 一起使用，因为其性能特征尚未完全体现并经过优化，无法适应许多服务器虚拟化部署中的各种典型工作负荷。

## <a name="enabling-smt-in-guest-virtual-machines"></a>在来宾虚拟机中启用 SMT

将虚拟化主机的虚拟机监控程序配置为使用核心计划程序类型后，可将来宾虚拟机配置为使用 SMT （如果需要）。 公开 VPs 被超线程为来宾虚拟机的事实，使来宾操作系统中的计划程序和 VM 中运行的工作负荷可以在其自己的工作计划中检测并使用 SMT 拓扑。 在 Windows Server 2016 上，来宾 SMT 默认情况下未配置，Hyper-v 主机管理员必须显式启用它。 从 Windows Server 2019 开始，主机上创建的新 Vm 默认继承主机的 SMT 拓扑。  也就是说，在每个核心具有2个 SMT 线程的主机上创建的9.0 版 VM 还会看到每个内核2个 SMT 线程。

PowerShell 必须用于在来宾虚拟机中启用 SMT;Hyper-v 管理器中没有提供用户界面。
若要在来宾虚拟机中启用 SMT，请打开具有足够权限的 PowerShell 窗口，然后键入：

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

其中<n> ，是来宾 VM 将看到的每个核心的 SMT 线程数。  
请注意<n> ，= 0 会将 HwThreadCountPerCore 值设置为与主机的每个核心的 SMT 线程计数值相匹配。

>[!NOTE] 
>从 Windows Server 2019 开始，支持设置 HwThreadCountPerCore = 0。

下面是一个从运行在包含2个虚拟处理器和 SMT 的虚拟机中运行的来宾操作系统获取的系统信息的示例。 来宾操作系统检测到属于同一核心的2个逻辑处理器。

![在启用了 SMT 的来宾 VM 中显示 msinfo32 的屏幕截图](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## <a name="configuring-the-hypervisor-scheduler-type-on-windows-server-2016-hyper-v"></a>在 Windows Server 2016 Hyper-v 上配置虚拟机监控程序计划程序类型

默认情况下，Windows Server 2016 Hyper-v 使用经典虚拟机监控程序计划程序模型。 可以选择将虚拟机监控程序配置为使用核心计划程序，通过限制来宾 VPs 在相应的物理 SMT 对上运行来提高安全性，并支持使用 SMT 计划为其来宾 VPs 使用虚拟机。

>[!NOTE]
>Microsoft 建议所有运行 Windows Server 2016 Hyper-v 的客户选择核心计划程序，以确保以最佳方式保护其虚拟化主机免受潜在的恶意来宾 Vm 的侵害。

## <a name="windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler"></a>Windows Server 2019 Hyper-v 默认使用核心计划程序

为了帮助确保以最佳安全配置部署 Hyper-v 主机，Windows Server 2019 Hyper-v 现在会默认使用核心虚拟机监控程序计划程序模型。 主机管理员可以选择将主机配置为使用旧的经典计划程序。 在重写计划程序类型默认设置之前，管理员应仔细阅读、了解并考虑每个计划程序类型对虚拟化主机的安全性和性能的影响。  有关详细信息，请参阅[了解 hyper-v 计划程序类型选择](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection)。

### <a name="required-updates"></a>所需更新

>[!NOTE]
>使用本文档中所述的虚拟机监控程序计划程序功能需要以下更新。 这些更新包括支持新的 "hypervisorschedulertype" BCD 选项的更改，这对于主机配置是必需的。

| Version | 发行版本  | 需要更新 | 知识库文章 |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018.07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018.07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1709 | 2018.07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | 无 | 无 |

## <a name="selecting-the-hypervisor-scheduler-type-on-windows-server"></a>选择 Windows Server 上的虚拟机监控程序计划程序类型

虚拟机监控程序计划程序配置通过 hypervisorschedulertype BCD 条目进行控制。

若要选择计划程序类型，请使用管理员权限打开命令提示符：

``` command
     bcdedit /set hypervisorschedulertype type
```

其中`type`是以下项之一：

* 经典
* Core
* 根

必须重新启动系统才能使虚拟机监控程序计划程序类型的任何更改生效。

>[!NOTE]
>Windows Server Hyper-v 目前不支持虚拟机监控程序根计划程序。 Hyper-v 管理员不应尝试配置根计划程序以用于服务器虚拟化方案。

## <a name="determining-the-current-scheduler-type"></a>确定当前计划程序类型

可以通过检查最新的虚拟机监控程序启动事件 ID 2 事件查看器中的系统日志来确定当前正在使用的虚拟机监控程序计划程序类型，该事件报告在虚拟机监控程序启动时配置的虚拟机监控程序计划程序类型。 可以从 Windows 事件查看器或通过 PowerShell 获取虚拟机监控程序启动事件。

虚拟机监控程序启动事件 ID 2 表示虚拟机监控程序计划程序类型，其中：

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![显示虚拟机监控程序启动事件 ID 2 详细信息的屏幕截图](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![显示显示虚拟机监控程序启动事件 ID 2 事件查看器的屏幕截图](media/Hyper-V-CoreScheduler-EventViewer.png)

### <a name="querying-the-hyper-v-hypervisor-scheduler-type-launch-event-using-powershell"></a>使用 PowerShell 查询 Hyper-v 虚拟机监控程序计划程序类型启动事件

若要使用 PowerShell 查询虚拟机监控程序事件 ID 2，请在 PowerShell 提示符下输入以下命令。

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![显示 PowerShell 查询和虚拟机监控程序启动事件 ID 2 的结果的屏幕截图](media/Hyper-V-CoreScheduler-PowerShell.png)
