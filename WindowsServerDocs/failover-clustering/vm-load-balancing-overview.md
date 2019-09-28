---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: 虚拟机负载平衡概述
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 1fea9e6297399a5081eb8fef1876f1b11d23c745
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360978"
---
# <a name="virtual-machine-load-balancing-overview"></a>虚拟机负载平衡概述

> 适用于：Windows Server 2019、Windows Server 2016

私有云部署的重要注意事项是资本支出（<abbr title="资本支出">CapEx</abbr>）需要投入生产。 将冗余添加到私有云部署，以避免在生产高峰时段内容量不足，但这会增加 <abbr title="资本支出">CapEx</abbr>. 冗余的需求由不平衡的私有云驱动，其中某些节点托管更多的虚拟机（<abbr title="虚拟机">虚拟机</abbr>）和其他人都使用不足（如全新重启的服务器）。

<strong>快速视频概述</strong><br>（6分钟）<br>
> [!VIDEO https://channel9.msdn.com/Blogs/windowsserver/Virtual-Machine-Load-Balancing-in-Windows-Server-2016/player]

## <a id="what-is-vm-load-balancing"></a>什么是虚拟机负载平衡？
<abbr title="虚拟机">VM</abbr> 负载均衡是 Windows Server 2019 和 Windows Server 2016 中的内置功能，可用于优化故障转移群集中节点的使用。 它标识过度提交的节点并重新分发 <abbr title="虚拟机">虚拟机</abbr> 从这些节点到已提交的节点。 此功能的一些突出特性如下：

* *这是一种零停机解决方案*： <abbr title="虚拟机">虚拟机</abbr> 实时迁移到空闲节点。
* *与现有群集环境无缝集成*：故障策略如消除相关性、容错域和可能的所有者。
* *用于平衡的试探法*： <abbr title="虚拟机">VM</abbr> 节点的内存压力和 CPU 使用率。
* *精细控制*：默认情况下处于启用状态。 可以按需或定期激活。
* *入侵阈值*：基于你的部署特征提供三个阈值。

## <a id="feature-in-action"></a>操作中的功能
### <a id="new-node-added"></a>新节点将添加到故障转移群集
![要添加到故障转移群集中的新节点的图形](media/vm-load-balancing/overview-VM-load-balancing-1.png)

将新容量添加到故障转移群集时， <abbr title="虚拟机">VM</abbr> 负载平衡功能会按以下顺序自动将现有节点的容量平衡到新添加的节点：

1. 在故障转移群集中的现有节点上评估压力。
2. 标识超出阈值的所有节点。
3. 具有最高压力的节点确定确定均衡的优先级。
4. <abbr title="虚拟机">虚拟机</abbr> 从超出阈值的节点实时迁移（无停机时间）到故障转移群集中的新添加节点。

### <a id="recurring-load-balancing"></a>周期性负载平衡
![定期 VM 负载平衡的图形](media/vm-load-balancing/overview-VM-load-balancing-2.png)

配置为定期平衡时，对群集节点的压力评估每30分钟进行一次平衡。 或者，压力可以按需计算。 下面是这些步骤的流：

1. 在私有云中的所有节点上评估压力。
2. 标识了超出阈值和低于阈值的所有节点。
3. 具有最高压力的节点确定确定均衡的优先级。
4. <abbr title="虚拟机">虚拟机</abbr> 从超出阈值到节点低于最小阈值的节点实时迁移（无停机时间）。

## <a name="see-also"></a>请参阅
* [虚拟机负载平衡深入探讨](vm-load-balancing-deep-dive.md)
* [故障转移群集](failover-clustering-overview.md)
* [Hyper-v 概述](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
