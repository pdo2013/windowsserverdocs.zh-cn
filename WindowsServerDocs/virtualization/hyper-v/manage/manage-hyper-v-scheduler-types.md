---
title: 了解和使用的 HYPER-V 虚拟机监控程序计划程序类型
description: 提供有关 HYPER-V 的计划程序的使用模式的 HYPER-V 主机管理员的信息
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: 7af6d68b02367d349580eacb27405c6f37e97ff8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871988"
---
# <a name="managing-hyper-v-hypervisor-scheduler-types"></a>管理的 HYPER-V 虚拟机监控程序计划程序类型

>适用于：Windows 10、 Windows Server 2016，Windows Server 1709 版、 Windows Server 版本 1803，Windows Server 2019

本文介绍计划逻辑首次引入 Windows Server 2016 中的虚拟处理器的新模式。 这些模式或计划程序类型，确定如何在 HYPER-V 虚拟机监控程序分配和管理在来宾虚拟处理器上的工作。 HYPER-V 主机管理员可以选择最适合用于来宾虚拟机 (Vm) 和配置 Vm 以充分利用计划逻辑的虚拟机监控程序计划程序类型。

>[!NOTE]
>使用本文档中所述的虚拟机监控程序计划程序功能需要更新。 有关详细信息，请参阅[所需的更新](#required-updates)。

## <a name="background"></a>后台

在讨论之前的逻辑和 HYPER-V 虚拟处理器计划后的控件，很有帮助，若要查看本文中所述的基本概念。

### <a name="understanding-smt"></a>了解 SMT

同时进行多线程处理或 SMT，是在现代处理器设计中利用该技术允许共享的单独、 独立的执行线程的处理器的资源。 SMT 通常提供对大多数工作负荷的适度的性能提升了并行执行计算，如果可能，请增加指令的吞吐量，但无性能获得或甚至是在性能略有下降可能会发生时的线程之间的争用共享的处理器资源时发生。
可从 Intel 和 AMD 处理器支持 SMT。 Intel 是指其 SMT 产品/服务为 Intel 超线程技术或 Intel 超线程。

出于本文的目的，SMT 和 HYPER-V 如何利用它的说明适用于 Intel 和 AMD 系统。

* 有关 Intel 超线程技术的详细信息，请参阅[Intel 超线程技术](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* 有关 AMD SMT 的详细信息，请参阅["处理"核心体系结构](https://www.amd.com/en/technologies/zen-core)

## <a name="understanding-how-hyper-v-virtualizes-processors"></a>了解如何 HYPER-V 虚拟化的处理器

在考虑之前虚拟机监控程序计划程序类型，该技术还有助于了解 HYPER-V 体系结构。 您可以找到以常规摘要[HYPER-V 技术概述](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview)。 以下是有关本文的重要概念：

* HYPER-V 创建和管理虚拟机分区，跨计算资源分配和共享下的虚拟机监控程序的控件。 分区提供强大的隔离边界之间所有来宾虚拟机和来宾虚拟机和根分区之间。

* 根分区本身就是一个虚拟机的分区，尽管它具有唯一的属性和多更高特权要低于来宾虚拟机。 根分区提供控制所有来宾虚拟机管理服务，提供虚拟设备支持的来宾，并管理来宾虚拟机的所有设备 I/O。 Microsoft 强烈建议不在根分区中运行任何应用程序工作负荷。

* 根分区的每个虚拟处理器 (VP) 为基础的逻辑处理器 (LP) 到映射的 1:1。 将始终在同一个基础 LP 上运行主机副总裁 – 根分区 VPs 无需迁移。

* 默认情况下，在其运行主机 VPs LPs 还可以运行来宾 VPs。

* 虚拟机监控程序可以计划来宾副总裁任何可用的逻辑处理器上运行。 而虚拟机监控程序计划程序负责计划来宾副总裁时应考虑临时缓存区域、 NUMA 拓扑和许多其他因素，最终可能 LP 的任何主机上计划副总裁。

## <a name="hypervisor-scheduler-types"></a>虚拟机监控程序计划程序类型

从 Windows Server 2016 开始，HYPER-V 虚拟机监控程序支持计划程序逻辑的多个的模式，确定如何在虚拟机监控程序计划基础的逻辑处理器上的虚拟处理器。 这些计划程序类型包括：

- [经典、 公平共享计划程序](#the-classic-scheduler)
- [核心计划程序](#the-core-scheduler)
- [根计划程序](#the-root-scheduler)

### <a name="the-classic-scheduler"></a>经典的计划程序

经典的计划程序已问世，包括 Windows Server 2016 HYPER-V 的所有版本的 Windows HYPER-V 虚拟机监控程序的默认值。 经典的计划程序提供公平份额，来宾虚拟处理器的 preemptive 轮循机制计划模型。

经典的计划程序类型是最合适的绝大多数传统的 HYPER-V 使用 – 对于私有云、 托管提供商，等等。 性能特点很好地理解和最大的优化，以支持广泛的虚拟化方案，如到 LPs，同时运行多个异源 Vm 和工作负荷、 运行大规模高 VPs 的过度订阅性能支持完整的功能的 Vm 集不受限制，HYPER-V 和的详细信息。

### <a name="the-core-scheduler"></a>核心计划程序

虚拟机监控程序 core 计划程序是经典的计划程序逻辑，在 Windows Server 2016 和 Windows 10 版本 1607年中引入的新的替代方法。 核心计划程序提供的来宾工作负荷隔离的强安全边界和内部支持 SMT 的虚拟化主机运行的 Vm 工作负荷的性能降低的可变性。 核心计划程序允许同一 SMT 启用虚拟化主机上同时运行 SMT 和非 SMT 虚拟机。

核心计划程序利用虚拟化主机的 SMT 拓扑，并根据需要公开 SMT 对到来宾虚拟机和来宾虚拟处理器的计划组从到 SMT 逻辑处理器组上相同的虚拟机。 这是对称，以便如果 LPs 中组的两个，VPs 计划两个，一组和 Vm 之间永远不会共享一个内核。
副总裁而无需 SMT 的虚拟机的计划时启用，副总裁，将在运行时消耗的所有核心。

核心计划程序的总体结果是：

* 来宾 VPs 限制为在基础物理核心对，隔离的 VM 与处理器核心边界，从而减少受攻击可能性旁路窥探攻击，恶意虚拟机上运行。

* 显著减少吞吐量的可变性。

* 可能会降低性能，因为如果只有一个 VPs 一组可以运行，只有一个核心中的指令流执行，而其他处于空闲状态。

* OS 和来宾虚拟机中运行的应用程序可以利用 SMT 行为和编程接口 (Api) 来控制和在 SMT 线程间分发工作，就像它们时将运行非虚拟化。

* 若要在基础物理核心对，减少到窥探攻击旁, 道漏洞上运行大容量限制来宾工作负荷隔离-来宾 VPs 的强大的安全边界。

核心计划程序将使用默认情况下在 Windows Server 2019 中启动。 在 Windows Server 2016 core 计划程序是可选的必须显式启用的 HYPER-V 主机管理员和经典的计划程序是默认值。

#### <a name="core-scheduler-behavior-with-host-smt-disabled"></a>与主机禁用 SMT 的核心计划程序行为

如果虚拟机监控程序配置为使用核心的计划程序类型，但 SMT 功能已禁用或虚拟化主机上不存在，则虚拟机监控程序将使用经典的计划程序行为，而不考虑虚拟机监控程序计划程序类型设置。

### <a name="the-root-scheduler"></a>根计划程序

根计划程序中引入了 Windows 10 1803年的版本。 启用根计划程序类型后，虚拟机监控程序控制权放到根分区计划工作。 根分区操作系统实例中的 NT 计划程序管理调度到系统 LPs 工作的所有的方面。

根计划程序解决的独特要求使用支持实用工具分区固有提供强的工作负荷隔离，因为使用与 Windows Defender 应用程序防护 (WDAG)。 在此方案中，保留计划任务转交给了根操作系统具有许多优点。 例如，CPU 资源控制适用于容器方案可能用于实用程序分区，从而简化了管理和部署。 此外，根 OS 计划程序可以轻松地收集有关工作负荷在容器内的 CPU 利用率指标，并使用此数据作为输入相同的计划策略适用于所有其他工作负荷在系统中。 这些相同的度量值还有助于清楚地完成应用程序容器到主机系统的工作的属性。 跟踪这些度量值是更加困难与传统虚拟机工作负荷，代表所有正在运行 VM 的一些工作发生的根分区中的地方。

#### <a name="root-scheduler-use-on-client-systems"></a>客户端系统上的根计划程序使用

从 Windows 10 1803年版开始，此根计划程序用默认情况下，客户端系统上以支持基于虚拟化的安全性和隔离 WDAG 工作负荷，并正确操作与未来的系统可能启用虚拟机监控程序异类核体系结构。 这是客户端系统的唯一受支持的虚拟机监控程序调度器配置。 管理员不应尝试重写在 Windows 10 客户端系统上的默认虚拟机监控程序计划程序类型。

#### <a name="virtual-machine-cpu-resource-controls-and-the-root-scheduler"></a>虚拟机 CPU 资源控制和根计划程序

虚拟机监控程序根计划程序已启用了与根操作系统的计划程序逻辑管理在全局基础上的主机资源，并不具备的虚拟机的知识时，不支持提供的 HYPER-V 虚拟机处理器资源控制特定的配置设置。 在虚拟机监控程序直接控制副总裁计划时，此类与经典部署模型和核心的计划程序类型一样，HYPER-V 每个 VM 处理器资源控件，如帽子、 权重和预留，才适用。

#### <a name="root-scheduler-use-on-server-systems"></a>根服务器系统上的计划程序使用

不建议用于 HYPER-V 的服务器上时，根计划程序，因为其性能特征尚不支持已完全特征并进行优化以适应广泛的许多服务器虚拟化部署的典型工作负荷。

## <a name="enabling-smt-in-guest-virtual-machines"></a>在来宾虚拟机中启用 SMT

虚拟化主机的虚拟机监控程序配置为使用核心的计划程序类型，可能配置来宾虚拟机根据需要利用 SMT。 公开 VPs 是超线程到来宾虚拟机的这一事实允许来宾操作系统和运行在 VM 中，若要检测并使用其自己的工作计划中的 SMT 拓扑的工作负荷中的计划程序。 Windows Server 2016 上来宾 SMT 未配置默认情况下，必须显式启用的 HYPER-V 主机管理员。 从 Windows Server 2019 开始，在主机上创建的新 Vm 将继承主机的 SMT 拓扑中，默认情况下。  也就是说，9.0 VM 创建的每个内核 2 个 SMT 线程的主机的版本另请参阅每个内核 2 个 SMT 线程。

必须使用 PowerShell 在来宾虚拟机; 中启用 SMT没有用户界面中的 HYPER-V 管理器提供。
若要在来宾虚拟机中启用 SMT，PowerShell 窗口使用打开足够的权限，并键入：

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

其中<n>是的来宾 VM 会看到的每个核心的 SMT 线程数。  
请注意， <n> = 0 将设置要匹配的每个核心值的主机的 SMT 线程计数的 HwThreadCountPerCore 值。

>[!NOTE] 
>设置 HwThreadCountPerCore = 0 支持从 Windows Server 2019 开始。

下面是来自具有 2 个虚拟处理器的虚拟机中运行的来宾操作系统的系统信息的示例和 SMT 启用。 来宾操作系统检测属于同一内核的 2 个逻辑处理器。

![屏幕截图，显示 msinfo32 来宾与 SMT 启用的 VM 中](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## <a name="configuring-the-hypervisor-scheduler-type-on-windows-server-2016-hyper-v"></a>配置 Windows Server 2016 HYPER-V 上的虚拟机监控程序计划程序类型

Windows Server 2016 HYPER-V 默认情况下使用经典虚拟机监控程序计划程序模型。 可以根据需要配置虚拟机监控程序为使用核心的计划程序，通过限制来宾 VPs 运行上相应的物理 SMT 对，并支持使用具有为其来宾 VPs 计划 SMT 的虚拟机，从而提高安全性。

>[!NOTE]
>Microsoft 建议运行 Windows Server 2016 HYPER-V 的所有客户都选择 core 计划程序，以确保其虚拟化主机以最佳方式会受到潜在的恶意的来宾虚拟机。

## <a name="windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler"></a>Windows Server 2019 HYPER-V 在默认情况下使用核心计划程序

若要帮助确保获得最佳安全配置中部署的 HYPER-V 主机，Windows Server 2019 HYPER-V 现在将默认情况下使用的核心虚拟机监控程序计划程序模型。 主机管理员可能可以选择将主机配置为使用旧的经典计划程序。 管理员应仔细阅读、 了解并考虑每个计划程序类型具有安全性和性能的虚拟化主机之前重写的计划程序类型默认设置的影响。  请参阅[了解 HYPER-V 计划程序类型选择](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection)有关详细信息。

### <a name="required-updates"></a>所需更新

>[!NOTE]
>使用本文档中所述的虚拟机监控程序计划程序功能需要以下更新。 这些更新包括更改以支持新的 hypervisorschedulertype BCD 选项，这是必需的主机配置。

| Version | 发行版本  | 所需的更新 | 知识库文章 |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018.07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018.07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1709 | 2018.07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | 无 | 无 |

## <a name="selecting-the-hypervisor-scheduler-type-on-windows-server"></a>选择 Windows 服务器上的虚拟机监控程序计划程序类型

通过 hypervisorschedulertype BCD 条目来控制虚拟机监控程序调度器配置。

若要选择的计划程序类型，请使用管理员特权打开命令提示符：

``` command
     bcdedit /set hypervisorschedulertype type
```

其中`type`是之一：

* 经典
* 核心版

有关对虚拟机监控程序计划程序类型才会生效的任何更改，必须重新启动系统。

>[!NOTE]
>虚拟机监控程序根计划程序目前不支持 Windows Server HYPER-V 上。 HYPER-V 管理员不应尝试使用服务器虚拟化方案配置为使用根计划程序。

## <a name="determining-the-current-scheduler-type"></a>确定当前的计划程序类型

您可以确定当前的虚拟机监控程序计划程序类型在使用通过检查事件查看器中的列值日志的最新的虚拟机监控程序启动事件 ID 为 2，它报告在虚拟机监控程序启动配置的虚拟机监控程序计划程序类型。 从 Windows 事件查看器中，或通过 PowerShell，可以获取虚拟机监控程序启动事件。

虚拟机监控程序启动事件 ID 为 2 表示虚拟机监控程序计划程序类型，其中：

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![屏幕截图显示虚拟机监控程序启动事件 ID 为 2 详细信息](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![显示事件查看器显示虚拟机监控程序启动事件 ID 为 2 的屏幕截图](media/Hyper-V-CoreScheduler-EventViewer.png)

### <a name="querying-the-hyper-v-hypervisor-scheduler-type-launch-event-using-powershell"></a>查询的 HYPER-V 虚拟机监控程序计划程序类型启动事件使用 PowerShell

查询到的虚拟机监控程序事件 ID 为 2，使用 PowerShell 输入以下命令从 PowerShell 提示符。

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![显示 PowerShell 查询和结果为虚拟机监控程序启动事件 ID 为 2 的屏幕截图](media/Hyper-V-CoreScheduler-PowerShell.png)
