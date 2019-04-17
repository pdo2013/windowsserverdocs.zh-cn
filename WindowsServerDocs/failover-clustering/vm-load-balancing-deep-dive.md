---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: "虚拟机负载平衡深层"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 6ee092a2e51e2181a2203bc263377c4a8ec60246
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>虚拟机负载平衡深层

> 适用于：Windows Server（半年通道）、Windows Server 2016

Windows Server 2016 引入[虚拟机负载平衡功能](vm-load-balancing-overview.md)优化利用率百分比故障转移群集节点。 本文介绍了如何配置和控制<abbr title="虚拟机">VM</abbr>负载平衡。 

## <a id="heuristics-for-balancing"></a>对于平衡启发
<abbr title="虚拟机">VM</abbr>虚拟机负载平衡评估节点负载根据以下启发：
1. 当前**内存压力**： 内存是 HYPER-V 主机上的最常见的资源约束
2. <abbr title="中央处理器">CPU</abbr> **利用率**节点平均值 5 分钟窗口的： 减少中变得过高提交群集节点

## <a id="controlling-aggressiveness-of-balancing"></a>控制平衡，快速
可使用配置，快速平衡基于内存和 CPU 启发通过群集常见财产 'AutoBalancerLevel。 若要控制运行以下命令在 PowerShell 中的是，快速：

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | 进取 | 行为 |
|-------------------|----------------|----------|
| 1 （默认） | 低 | 主机时加载 80%以上移动 |
| 2 | 中等 | 主机时加载 70%以上移动 |
| 3 | 高 | 主机时加载 60%以上移动 | 

![图中的配置平衡，快速 PowerShell](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-abbr-titlevirtual-machinevmabbr-load-balancing"></a>控制<abbr title="虚拟机">VM</abbr>负载平衡
<abbr title="虚拟机">VM</abbr>负载平衡默认处于启用状态并负载平衡发生时可通过群集常见财产 AutoBalancerMode' 配置。 若要控制时节点公平余额群集：

### <a name="using-failover-cluster-manager"></a>使用故障转移群集管理器：
1. 右键单击你群集的名称，然后选择"属性"选项  
    ![图中选择群集故障转移群集管理器通过属性](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  选择"平衡"窗格  
    ![选择平衡选项通过故障转移群集管理器中的图形](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>使用 PowerShell:
运行以下命令：
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |行为| 
|:----------------:|:----------:|
|0| 已禁用| 
|1| 加载余额节点加入| 
|2 （默认）| 加载余额节点加入和每隔 30 分钟 |

## <a name="abbr-titlevirtual-machinevmabbr-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a><abbr title="虚拟机">VM</abbr>负载平衡与 System Center 虚拟机管理器动态优化
节点公平功能，提供收件箱功能，面向部署不 System Center 虚拟机 Manager (<abbr title="System Center 虚拟机 Manager">SCVMM</abbr>)。 <abbr title="System Center 虚拟机管理器">SCVMM</abbr>动态优化是平衡群集以在虚拟机加载的推荐的机制<abbr title="System Center 虚拟机 Manager">SCVMM</abbr>部署。 <abbr title="System Center 虚拟机管理器">SCVMM</abbr>自动禁用 Windows Server<abbr title="虚拟机">VM</abbr>负载平衡动态优化启用。

## <a name="see-also"></a>请参阅
* [虚拟机平衡概述](vm-load-balancing-overview.md)
* [故障转移群集](failover-clustering-overview.md)
* [Hyper V 概述](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
