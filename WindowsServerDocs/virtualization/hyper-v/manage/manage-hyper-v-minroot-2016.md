---
title: Minroot
description: 配置主机 CPU 资源控件
keywords: windows 10, hyper-v
author: allenma
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: ''
ms.openlocfilehash: e1269c11df32c8ce95cc7455d47d7170e9d0b1c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844328"
---
# <a name="hyper-v-host-cpu-resource-management"></a>Hyper V 主机 CPU 资源管理

Windows Server 2016 中引入或更高版本允许 HYPER-V 管理员能够更好地管理和分配"root"，或管理分区与来宾 Vm 之间的 CPU 资源的主机服务器的 HYPER-V 主机 CPU 资源控件。 使用这些控件，管理员可以将子集的主机系统的处理器专用于根分区。 这可以分离通过在单独的系统处理器的子集上运行这些来宾虚拟机中运行的工作负荷的 HYPER-V 主机中完成的工作。

有关 HYPER-V 主机的硬件的详细信息，请参阅[Windows 10 HYPER-V 系统要求](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements)。

## <a name="background"></a>后台

设置将控制 hyper-v 主机的 CPU 资源之前，最好查看 HYPER-V 体系结构的基础知识。  
您可以找到以常规摘要[HYPER-V 体系结构](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/hyper-v-server/architecture)部分。
以下是有关本文的重要概念：

* HYPER-V 创建和管理虚拟机分区，跨计算资源分配和共享下的虚拟机监控程序的控件。  分区提供强大的隔离边界之间所有来宾虚拟机和来宾虚拟机和根分区之间。

* 根分区本身就是一个虚拟机的分区，尽管它具有唯一的属性和多更高特权要低于来宾虚拟机。  根分区提供控制所有来宾虚拟机管理服务，提供虚拟设备支持的来宾，并管理来宾虚拟机的所有设备 I/O。  Microsoft 强烈建议不在主分区中运行任何应用程序工作负荷。

* 根分区的每个虚拟处理器 (VP) 为基础的逻辑处理器 (LP) 到映射的 1:1。  将始终在同一个基础 LP 上运行主机副总裁 – 根分区 VPs 无需迁移。  

* 默认情况下，在其运行主机 VPs LPs 还可以运行来宾 VPs。

* 虚拟机监控程序可以计划来宾副总裁任何可用的逻辑处理器上运行。  而虚拟机监控程序计划程序负责计划来宾副总裁时应考虑临时缓存区域、 NUMA 拓扑和许多其他因素，最终可能 LP 的任何主机上计划副总裁。

## <a name="the-minimum-root-or-minroot-configuration"></a>最小根或者"Minroot"配置

早期版本的 HYPER-V 有一个体系结构的最大限制为 64 VPs 每个分区。  此应用于根和来宾的分区。  高端服务器上出现具有超过 64 个逻辑处理器的系统，如 HYPER-V 还发展以支持这些更大的系统，支持具有最多 320 个 LPs 的主机的某一点其主机规模限制。  但是，重大 64 每个分区限制在该时间的副总裁提出了几个挑战，引入进行支持 64 个以上 VPs 每个分区过高的复杂性。  若要解决此问题，HYPER-V 有限即使基础计算机有许多可用的多个逻辑处理器 VPs 提供给为 64，根分区数。  虚拟机监控程序将继续运行来宾 VPs 利用所有可用 LPs 但是人为上限为 64 在根分区。  此配置称为"最小 root"或"minroot"配置。  性能测试确认，甚至在使用 64 个以上 LPs 大型系统中，根不需要 64 个以上的根 VPs 实际上提供足够支持大量来宾虚拟机和来宾 VPs –、 远少于 64 根 VPs 通常已足够当然根据的数量和大小的来宾虚拟机的正在运行的特定工作负荷，等等。

此"minroot"概念继续立即使用。  实际上，即使在 Windows Server 2016 HYPER-V 增加到 512 LPs 主机 LPs 其体系结构支持，最大限制，根分区仍将限制为最多 320 LPs。

## <a name="using-minroot-to-constrain-and-isolate-host-compute-resources"></a>使用 Minroot 限制和隔离主机计算资源
320 LPs 在 Windows Server 2016 HYPER-V 的高默认阈值，minroot 配置仅将利用在非常大的服务器系统上。  但是，此功能可以配置为多较低的阈值，HYPER-V 主机管理员，因此利用极大地限制到根分区的可用的主机 CPU 资源量。  特定数量的根 LPs 利用必须当然慎重选择以支持最大的虚拟机和分配给主机的工作负荷需求。  但是，主机 LPs 数的合理值可以确定通过仔细评估和监视的生产工作负荷，并在广泛部署前的非生产环境中已验证。

## <a name="enabling-and-configuring-minroot"></a>启用和配置 Minroot

Minroot 配置控制通过虚拟机监控程序 BCD 条目。 若要启用 minroot，从使用管理员权限的 cmd 提示符：

```
    bcdedit /set hypervisorrootproc n
```
其中，n 是根 VPs 数。 

必须重新启动系统，并且新的根的处理器数将会保留 OS 引导的生存期内。  不能在运行时动态更改 minroot 配置。

如果有多个 NUMA 节点，每个节点将获取`n/NumaNodeCount`处理器。

若要运行相应虚拟机的 NUMA 节点 VPs 每个 NUMA 节点上，请注意，使用多个 NUMA 节点，您必须确保 VM 的拓扑，以便有足够可用 LPs (即，未指定根 VPs LPs)。

## <a name="verifying-the-minroot-configuration"></a>正在验证 Minroot 配置

如下所示，可以验证主机的 minroot 配置使用任务管理器。

![](./media/minroot-taskman.png)

当 Minroot 处于活动状态时，任务管理器将显示当前分配给主机，除了在系统中的逻辑处理器的总数的逻辑处理器数。
 
