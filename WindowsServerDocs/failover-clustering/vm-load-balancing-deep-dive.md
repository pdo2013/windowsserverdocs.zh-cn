---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: 虚拟机负载平衡深度解析
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 50213cf47c2c59f1775ae704e82ed51794715ac0
ms.sourcegitcommit: 276a480b470482cba4682caa3df4cd07ba5b7801
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66198554"
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>虚拟机负载平衡深度解析

> 适用于：Windows Server 2019、Windows Server 2016

[虚拟机负载平衡功能](vm-load-balancing-overview.md)优化故障转移群集中节点的利用率。 本文档介绍如何配置和控制虚拟机负载平衡。 

## <a id="heuristics-for-balancing"></a>用来平衡试探法
虚拟机负载均衡的计算结果根据以下试探法的节点的负载：
1. 当前**内存压力**:内存是在 HYPER-V 主机上的最常见资源限制
2. CPU**利用率**的 5 分钟窗口内的平均的节点：可缓解变得过载群集中的节点

## <a id="controlling-aggressiveness-of-balancing"></a>控制平衡的入侵
可以使用配置的均衡基于内存和 CPU 启发入侵通过群集公用属性 'AutoBalancerLevel。 若要控制在 PowerShell 中运行以下入侵：

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | 入侵 | 行为 |
|-------------------|----------------|----------|
| 1（默认值） | 低 | 当主机是多个加载的 80%时，移动 |
| 2 | 中等 | 当主机是多个加载的 70%时，移动 |
| 3 | 高 | 平均节点和移动主机时超过 5%高于平均水平 | 

![PowerShell 的配置的平衡入侵的图形](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-vm-load-balancing"></a>控制 VM 负载平衡
默认情况下启用 VM 负载平衡和负载平衡发生时可以通过群集公用属性 'AutoBalancerMode 配置。 若要控制节点公平度时平衡群集：

### <a name="using-failover-cluster-manager"></a>使用故障转移群集管理器：
1. 右键单击群集名称，然后选择"属性"选项  
    ![选择群集通过故障转移群集管理器的属性的图：](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  选择"均衡器"窗格  
    ![选择平衡器选项通过故障转移群集管理器的图：](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>使用 PowerShell:
运行以下命令：
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |行为| 
|:----------------:|:----------:|
|0| Disabled| 
|1| 节点联接上实现负载均衡| 
|2 （默认值）| 负载平衡节点加入并每隔 30 分钟 |

## <a name="vm-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a>VM 负载平衡 vs。System Center Virtual Machine Manager 动态优化
节点公平度功能，提供了内置功能，主要用于部署而无需 System Center Virtual Machine Manager (SCVMM)。 SCVMM 动态优化是建议使用的 SCVMM 部署在群集中的虚拟机负载平衡机制。 SCVMM 会自动禁用 Windows Server VM 负载平衡时启用动态优化。

## <a name="see-also"></a>请参阅
* [虚拟机负载平衡概述](vm-load-balancing-overview.md)
* [故障转移群集](failover-clustering-overview.md)
* [HYPER-V 概述](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
