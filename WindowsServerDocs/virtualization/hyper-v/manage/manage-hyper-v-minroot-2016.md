---
title: Minroot
description: 配置主机 CPU 资源控制
keywords: windows 10, hyper-v
author: allenma
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: ''
ms.openlocfilehash: 92de899a39aed05e2f598fcb3aae3fbae3f1cb67
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872036"
---
# <a name="hyper-v-host-cpu-resource-management"></a>Hyper-v 主机 CPU 资源管理

Windows Server 2016 或更高版本中引入的 hyper-v 主机 CPU 资源控制允许 Hyper-v 管理员更好地管理和分配 "根"、管理分区和来宾 Vm 之间的主机服务器 CPU 资源。 管理员可以使用这些控件将主机系统的一部分处理器专用于根分区。 通过在 Hyper-v 主机上运行的工作负荷在系统处理器的单独子集上运行，这可以将 Hyper-v 主机中完成的工作与这些工作负荷隔离开来。

有关 Hyper-v 主机硬件的详细信息，请参阅[Windows 10 Hyper-v 系统要求](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements)。

## <a name="background"></a>后台

在设置 Hyper-v 主机 CPU 资源的控制之前，查看 Hyper-v 体系结构的基础知识会很有帮助。  
你可以在 " [Hyper-v 体系结构](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/hyper-v-server/architecture)" 部分找到一般摘要。
下面是本文的重要概念：

* Hyper-v 创建和管理虚拟机分区，在这些分区中分配和共享计算资源，以控制虚拟机监控程序。  分区在所有来宾虚拟机之间以及来宾 Vm 与根分区之间提供强大的隔离边界。

* 根分区本身是虚拟机分区，尽管它具有唯一的属性和比来宾虚拟机更高的特权。  根分区提供控制所有来宾虚拟机的管理服务，为来宾提供虚拟设备支持，并管理来宾虚拟机的所有设备 i/o。  Microsoft 强烈建议不要在主机分区中运行任何应用程序工作负荷。

* 根分区的每个虚拟处理器（副总裁）映射到基础逻辑处理器（LP）1:1。  主机副总裁始终在同一基础 LP 上运行–不会迁移根分区的 VPs。  

* 默认情况下，主机 VPs 运行的 LPs 还可以运行来宾 VPs。

* 来宾副总裁可以安排在任何可用的逻辑处理器上运行的虚拟机监控程序。  虽然虚拟机监控程序计划程序在计划来宾副总裁时，请注意考虑时态缓存区域、NUMA 拓扑和许多其他因素，最终可以在任何主机 LP 上计划副总裁。

## <a name="the-minimum-root-or-minroot-configuration"></a>最小根或 "Minroot" 配置

早期版本的 Hyper-v 每个分区最多具有 64 VPs 的体系结构。  这同时适用于根分区和来宾分区。  当高端服务器上出现具有超过64个逻辑处理器的系统时，Hyper-v 还会发展其主机缩放限制，以支持这些较大的系统，同时支持最多 320 LPs 的主机。  但是，打破每个分区的每个分区限制的64副总裁面临着几个难题，并带来了对每个分区的支持超过 64 VPs 的复杂性。  若要解决此情况，Hyper-v 会将向根分区提供的 VPs 数量限制为64，即使基础计算机有更多可用的逻辑处理器。  虚拟机监控程序将继续使用所有可用的 LPs，以运行来宾 VPs，但人为在64的情况下限制根分区。  此配置称为 "最小根" 或 "minroot" 配置。  性能测试已确认，即使在 LPs 超过64的大型系统中，根也不需要超过64个根 VPs 即可为大量来宾 Vm 和来宾 VPs 提供足够的支持-实际上，几乎不会有64的根 VPs，具体取决于来宾 Vm 的数量和大小、正在运行的特定工作负荷等。

此 "minroot" 概念现在继续使用。  事实上，即使 Windows Server 2016 Hyper-v 增加了主机 LPs 到 512 LPs 的最大体系结构支持限制，根分区也会限制为最多 320 LPs。

## <a name="using-minroot-to-constrain-and-isolate-host-compute-resources"></a>使用 Minroot 来约束和隔离主机计算资源
使用 Windows Server 2016 Hyper-v 中的高默认阈值 320 LPs，minroot 配置将仅在非常大的服务器系统上使用。  但是，Hyper-v 主机管理员可以将此功能配置为比阈值大得多的阈值，因此可以利用它来大幅限制根分区可用的主机 CPU 资源量。  当然，必须仔细选择要利用的根 LPs 的特定数量，以支持分配给主机的 Vm 和工作负荷的最大需求。  但是，可以通过仔细评估和监视生产工作负荷，并在非生产环境中进行广泛的部署，来确定主机 LPs 数的合理值。

## <a name="enabling-and-configuring-minroot"></a>启用和配置 Minroot

Minroot 配置是通过虚拟机监控程序 BCD 条目控制的。 若要从具有管理员权限的 cmd 提示符启用 minroot，请执行以下操作：

```
    bcdedit /set hypervisorrootproc n
```
其中，n 是根 VPs 的数目。 

系统必须重新启动，并且新的根处理器数量将在 OS 启动的生存期内保持不变。  Minroot 配置不能在运行时动态更改。

如果有多个 NUMA 节点，则每个节点`n/NumaNodeCount`都将获得处理器。

请注意，对于多个 NUMA 节点，你必须确保 VM 的拓扑为：在每个 NUMA 节点上有足够的可用 LPs （即，LPs 无根 VPs）来运行相应 VM 的 NUMA 节点 VPs。

## <a name="verifying-the-minroot-configuration"></a>验证 Minroot 配置

你可以使用任务管理器来验证主机的 minroot 配置，如下所示。

![](./media/minroot-taskman.png)

当 Minroot 处于活动状态时，除了系统中的逻辑处理器总数外，任务管理器还将显示当前分配给主机的逻辑处理器的数量。
 
