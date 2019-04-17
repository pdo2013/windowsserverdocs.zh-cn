---
title: 了解并使用 HYPER-V 虚拟机监控程序调度程序类型
description: 提供有关 HYPER-V 的调度程序的使用模式的信息适用于 HYPER-V 主机管理员
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: 7af6d68b02367d349580eacb27405c6f37e97ff8
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783689"
---
# 管理 HYPER-V 虚拟机监控程序调度程序类型

>适用于： Windows 10、 Windows Server 2016、 Windows Server 版本 1709年、 Windows Server 版本 1803、 Windows Server 2019

本文介绍了新模式的虚拟处理器计划首次引入 Windows Server 2016 中的逻辑。 这些模式或调度程序类型确定 HYPER-V 虚拟机监控程序如何分配和管理在来宾虚拟处理器的工作。 HYPER-V 主机管理员可以选择最适合用于来宾虚拟机 (Vm) 和配置 Vm 以充分利用计划逻辑的虚拟机监控程序计划程序类型。

>[!NOTE]
>更新需要使用此文档中所述的虚拟机监控程序计划程序功能。 有关详细信息，请参阅[所需更新](#required-updates)。

## Background

在讨论之前的逻辑和 HYPER-V 虚拟处理器计划背后的控件，最好先查看本文中介绍的基本概念。

### 了解 SMT

同时进行多线程处理或 SMT，是一种现代处理器设计中利用允许通过独立的独立执行线程共享处理器的资源。 SMT 通常增加指令吞吐量并行执行计算时，提供适当的性能提升到大多数工作负载，但没有性能获得甚至性能轻微损失时可能会出现争用之间的线程共享的处理器资源，会发生。
提供从 Intel 和 AMD 支持 SMT 处理器。 Intel 指的是 Intel 超线程技术或 Intel 超线程作为其 SMT 产品/服务。

对于本文而言，SMT 和如何利用 hyper-v 所描述的平均分配 Intel 和 AMD 系统应用。

* Intel 超线程技术的详细信息，请参阅[Intel 超线程技术](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* 有关 AMD SMT 的详细信息，请参阅["Zen"核心体系结构](https://www.amd.com/en/technologies/zen-core)

## 了解如何 HYPER-V 虚拟处理器

在考虑之前虚拟机监控程序调度程序类型，它还了解很有帮助的 HYPER-V 体系结构。 你可以在[HYPER-V 技术概述](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview)中找到常规摘要。 以下是本文中的重要概念：

* HYPER-V 创建和管理虚拟机分区，跨哪些计算资源分配和共享的虚拟机监控程序控制。 分区提供强大的隔离边界在所有来宾虚拟机之间以及在来宾虚拟机和根分区。

* 根分区本身就是一个虚拟机分区，尽管它具有唯一属性和比来宾虚拟机等更高的权限。 根分区提供控制所有来宾虚拟机管理服务、 提供对来宾虚拟设备支持和管理用于来宾虚拟机的所有设备 I/O。 Microsoft 强烈建议不根分区中运行任何应用程序工作负荷。

* 根分区的每个虚拟处理器 (VP) 为基础的逻辑处理器 (LP) 的映射的 1:1。 主机副总裁将始终在相同的基础 LP 上运行 – 根分区 Vp 无需迁移。

* 默认情况下，在其运行主机 Vp Lp 还可以运行来宾 Vp。

* 来宾副总裁可能由虚拟机监控程序计划，任何可用的逻辑处理器上运行。 虽然虚拟机监控程序计划程序负责时需要考虑临时缓存所在地、 NUMA 拓扑和许多其他因素计划来宾副总裁，最终可能 LP 任何主机上计划副总裁。

## 虚拟机监控程序调度程序类型

从 Windows Server 2016 开始，HYPER-V 虚拟机监控程序支持多个模式的计划程序逻辑，用于确定虚拟机监控程序如何计划上的基础的逻辑处理器的虚拟处理器。 这些计划程序类型包括：

- [经典且公平共享计划程序](#the-classic-scheduler)
- [核心计划程序](#the-core-scheduler)
- [根计划程序](#the-root-scheduler)

### 经典计划程序

经典计划程序已自问世以来，包括 Windows Server 2016 HYPER-V 的所有版本的 Windows HYPER-V 虚拟机监控程序的默认值。 经典调度程序提供公平共享，来宾虚拟处理器的预防轮循它调度模式。

经典计划程序类型是最合适的传统 HYPER-V 使用绝大多数-对于私有云、 托管提供程序中，依次类推。 性能特征很好地理解和最佳优化可支持广泛的虚拟化方案，例如过度订阅到 LPs，同时运行多个异类 Vm 和工作负载，运行较大的比例高 Vp性能支持功能全面的 Vm 的 HYPER-V 中不受限制，以及更多内容集。

### 核心计划程序

虚拟机监控程序核心计划程序经典计划程序逻辑，在 Windows Server 2016 和 Windows 10 版本 1607年中引入的新的替代方法。 核心调度程序提供来宾的工作负荷隔离的强大安全边界，并为 SMT 启用虚拟化主机运行的虚拟机内部的工作负荷的性能降低的可变性。 核心调度程序允许同一个 SMT 启用虚拟化主机上同时运行 SMT 和非 SMT 虚拟机。

核心 scheduler 利用虚拟化主机 SMT 拓扑，并 （可选） 公开 SMT 对来宾虚拟机，并计划组的来宾虚拟处理器从同一个虚拟机上的 SMT 逻辑处理器组。 这是对称，以便如果 Lp 在组中的第二个，计划的两个 Vp 组的虚拟机之间永远不会共享的核心。
计划在何时虚拟机，而 SMT 副总裁启用，副总裁将在运行时使用的所有核心。

核心调度程序的整体结果是：

* 来宾 Vp 被限制在基础物理核心对，隔离处理器核心边界的 VM，从而降低漏洞侧信道侦听攻击的恶意的虚拟机上运行。

* 大大减少吞吐量变化。

* 可能会降低性能，因为如果只有一个 Vp 的一组可运行，核心中的指令流之一执行，而其他处于空闲状态。

* 操作系统和来宾虚拟机中运行的应用程序可以利用 SMT 行为和程序编程接口 (Api) 以控制和分发工作 SMT 线程，就像它们将运行时非虚拟化。

* 来宾的工作负荷隔离的来宾 Vp 的强大安全边界被受限，以减少对侧信道侦听攻击漏洞的基础物理核心对上运行。

核心计划程序将使用默认情况下，从 Windows Server 2019 开始。 Windows Server 2016 上核心计划程序是可选的必须显式启用 HYPER-V 主机管理员，并且经典计划程序是默认设置。

#### 与主机的核心 scheduler 行为 SMT 禁用

如果虚拟机监控程序被配置为使用核心计划程序类型，但 SMT 功能已禁用或不存在虚拟化主机上，虚拟机监控程序将使用经典 scheduler 行为，而不考虑设置虚拟机监控程序计划程序类型。

### 根计划程序

根计划程序已随 Windows 10 版本 1803年。 启用根计划程序类型后，虚拟机监控程序 cedes 工作安排到根分区的控件。 根分区操作系统实例中的 NT 计划程序管理调度到系统 Lp 工作的所有的方面。

根 scheduler 地址的独特需求固有支持实用工具分区提供强大的工作负荷隔离，如使用与 Windows Defender 应用程序防护 (WDAG)。 在此方案中，保留调度到根操作系统的责任提供多项优势。 例如，CPU 资源控制适用于容器方案可能使用实用程序分区，从而简化了管理和部署。 此外，根操作系统 scheduler 随时可以收集指标工作负荷容器内的 CPU 使用率和在系统作为输入到同一个适用于所有其他工作负荷的计划策略中使用此数据。 这些相同的指标还帮助清楚地工作到主系统的应用程序容器中的属性。 跟踪这些指标将更难使用传统的虚拟机工作负载，其中一些适用于所有运行的 VM 的代表，便会根分区中。

#### 客户端系统上的根计划使用

从 Windows 10 版本 1803年开始，根计划使用默认情况下，客户端系统，以支持基于虚拟化的安全性和 WDAG 工作负荷隔离，并且具有未来系统中的正常操作可能会启用虚拟机监控程序异类核心体系结构。 这是客户端系统的唯一受支持的虚拟机监控程序计划程序配置。 管理员不应尝试重写 Windows 10 客户端系统上的默认虚拟机监控程序计划程序类型。

#### 虚拟机 CPU 资源控制和根计划程序

为根操作系统的计划程序逻辑管理全球主机资源，并且没有知识的虚拟机的虚拟机监控程序根计划程序启用时，不支持由 HYPER-V 虚拟机处理器资源控制特定配置设置。 在虚拟机监控程序直接控制副总裁计划时，此类与经典和核心调度程序类型的 HYPER-V 每台 VM 处理器资源控制，如 caps、 重量和保留，才适用。

#### 服务器的系统上的根计划使用

此时，在服务器上使用 HYPER-V 不建议根调度程序，因为其性能特征尚不支持已完全特征和调整以适应各种工作负荷典型的多个服务器虚拟化部署。

## 在来宾虚拟机中启用 SMT

一旦虚拟化主机虚拟机监控程序配置为使用核心计划程序类型，来宾虚拟机可能配置为利用 SMT，如果需要。 公开 Vp 是超线程到来宾虚拟机这一事实允许中的来宾操作系统和 VM 来检测和利用自己工作计划中的 SMT 拓扑中运行的工作负荷的计划程序。 Windows Server 2016 上来宾 SMT 不配置默认情况下，并且必须在显式启用 HYPER-V 主机管理员。 从 Windows Server 2019，在主机上创建的新虚拟机将继承主机的 SMT 拓扑中，默认情况下。  也就是说，每个核心的 2 个 SMT 线程与主机创建 9.0 VM 版本另请参阅每个核心的 2 个 SMT 线程。

必须使用 PowerShell 启用 SMT 在来宾虚拟机。没有用户界面在 HYPER-V 管理器中提供。
若要启用 SMT 来宾虚拟机中，打开 PowerShell 窗口足够的权限，然后键入：

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

其中<n>是的来宾虚拟机将看到的每个核心的 SMT 线程的数量。  
请注意， <n> = 0 将设置 HwThreadCountPerCore 值以匹配每个核心值的主机的 SMT 线程计数。

>[!NOTE] 
>设置 HwThreadCountPerCore = 0 支持 Windows Server 2019 开头。

下面是摘自具有 2 个虚拟处理器的虚拟机中运行的来宾操作系统的系统信息的示例和 SMT 启用。 来宾操作系统检测属于相同的核心的 2 个逻辑处理器。

![显示 msinfo32 中来宾虚拟机与 SMT 启用的屏幕截图](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## 配置在 Windows Server 2016 HYPER-V 的虚拟机监控程序计划程序类型

默认情况下，Windows Server 2016 HYPER-V 使用经典虚拟机监控程序计划程序模型。 虚拟机监控程序可以选择性地配置为使用核心调度程序，以提高安全限制来宾 Vp 对应的物理 SMT 对上运行并支持其来宾 Vp SMT 计划的虚拟机的使用。

>[!NOTE]
>Microsoft 建议所有运行 Windows Server 2016 HYPER-V 的客户选择核心调度程序，以确保他们的虚拟化主机以最佳方式免受恶意来宾虚拟机。

## Windows Server 2019 HYPER-V 在默认情况下使用核心计划程序

为了帮助保证在最佳安全配置中部署 HYPER-V 主机时，Windows Server 2019 HYPER-V 现在将默认使用核心虚拟机监控程序计划程序模型。 主机管理员可能 （可选） 将主机配置为使用传统的经典调度程序。 管理员应仔细阅读，了解和考虑每个计划程序类型的安全性和性能的前重写计划程序类型的默认设置的虚拟化主机具有的影响。  有关详细信息，请参阅[了解 HYPER-V 计划程序类型选择](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection)。

### 所需的更新

>[!NOTE]
>使用本文档中所述的虚拟机监控程序计划程序功能所需的以下更新。 这些更新包括更改以支持新的 hypervisorschedulertype BCD 选项，这是必需的主机配置。

| 版本 | 版本  | 需要更新 | 知识库文章 |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018.07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018.07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1709 | 2018.07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | 无 | 无 |

## 选择 Windows Server 上的虚拟机监控程序计划程序类型

通过 hypervisorschedulertype BCD 条目控制的虚拟机监控程序计划程序配置。

若要选择的计划类型，请使用管理员权限打开命令提示符下：

``` command
     bcdedit /set hypervisorschedulertype type
```

其中`type`是之一：

* 经典
* 核心版

若要生效的虚拟机监控程序计划程序类型的任何更改，必须重新启动系统。

>[!NOTE]
>虚拟机监控程序根 scheduler 目前不支持在 Windows Server HYPER-V。 HYPER-V 管理员不应尝试使用服务器虚拟化方案中配置的根调度程序使用。

## 确定当前的计划类型

你可以确定当前的虚拟机监控程序计划程序类型通过检查最新的虚拟机监控程序启动事件 ID 为 2，事件查看器中的系统日志的使用该报告在虚拟机监控程序启动配置的虚拟机监控程序计划程序类型。 可以从 Windows 事件查看器中，或通过 PowerShell 获取虚拟机监控程序启动事件。

虚拟机监控程序启动事件 ID 为 2 表示虚拟机监控程序计划程序类型，其中：

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![事件 ID 为 2 显示虚拟机监控程序启动的详细信息的屏幕截图](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![显示显示虚拟机监控程序启动事件 ID 为 2 的事件查看器的屏幕截图](media/Hyper-V-CoreScheduler-EventViewer.png)

### 查询 HYPER-V 虚拟机监控程序计划程序类型启动事件使用 PowerShell

查询的虚拟机监控程序事件 ID 为 2 使用 PowerShell 中，输入以下命令，从 PowerShell 提示符。

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![显示 PowerShell 查询和结果的虚拟机监控程序启动事件 ID 为 2 的屏幕截图](media/Hyper-V-CoreScheduler-PowerShell.png)
