---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: 虚拟机负载平衡概述
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 125dd7421cc1876c07983016498a9689d8a507ac
ms.sourcegitcommit: 6fec3ca19ddaecbc936320d98cca0736dd8505d1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2019
ms.locfileid: "65475991"
---
# <a name="virtual-machine-load-balancing-overview"></a>虚拟机负载平衡概述

> 适用于：Windows Server 2019、Windows Server 2016

私有云部署关键的考虑因素是资本支出 (<abbr title="会产生资本支出">资本支出</abbr>) 需转到生产环境。 是很常见，以将冗余添加到私有云部署，以避免在生产环境中的峰值流量期间未得到充分的容量，但这会增加<abbr title="会产生资本支出">资本支出</abbr>。 由不均衡其中某些节点承载更多虚拟机的私有云驱动的冗余需求 (<abbr title="虚拟机">Vm</abbr>) 和其他人 （如全新重新启动服务器） 使用不足。

<strong>快速视频概述</strong><br>（6 分钟）<br>
> [!VIDEO https://channel9.msdn.com/Blogs/windowsserver/Virtual-Machine-Load-Balancing-in-Windows-Server-2016/player]

## <a id="what-is-vm-load-balancing"></a>什么是虚拟机负载平衡？
<abbr title="虚拟机">VM</abbr>负载平衡是 Windows Server 2019 和 Windows Server 2016，可用于优化使用情况的故障转移群集中的节点中的现成功能。 它标识处于过载状态的节点，并重新分发<abbr title="虚拟机">Vm</abbr>从这些下提交节点到节点。 此功能的突出方面的一些如下所示：

* *它是一个零停机时间解决方案*:<abbr title="虚拟机">Vm</abbr>实时迁移到空闲的节点。
* *与你现有的群集环境无缝集成*:将生效，但失败策略，如反相关性、 容错域和可能的所有者。
* *用来平衡试探法*:<abbr title="虚拟机">VM</abbr>内存压力和节点的 CPU 使用率。
* *精细的控制*:默认情况下处于启用状态。 可以按需或按定期间隔激活。
* *入侵阈值*:三个阈值可根据您的部署的特征。

## <a id="feature-in-action"></a>操作中的功能
### <a id="new-node-added"></a>将新节点添加到故障转移群集
![图： 的新节点添加到故障转移群集](media/vm-load-balancing/overview-VM-load-balancing-1.png)

当将新的容量添加到故障转移群集，<abbr title="虚拟机">VM</abbr>负载平衡功能自动均衡来自现有节点，按以下顺序新添加的节点的容量：

1. 在故障转移群集中的现有节点上计算方面的压力。
2. 超过阈值的所有节点都被都标识。
3. 具有最高压力的节点进行标识，以确定优先级的平衡。
4. <abbr title="虚拟机">Vm</abbr>取自实时迁移 （使用无需停机时间） 超出阈值为故障转移群集中新添加的节点的节点。

### <a id="recurring-load-balancing"></a>重复执行负载平衡
![图定期 VM 进行负载均衡](media/vm-load-balancing/overview-VM-load-balancing-2.png)

当配置为定期均衡，群集节点上的压力平衡每隔 30 分钟的评估。 或者，在压力可以按需评估。 下面是步骤的流程：

1. 在私有云中的所有节点上计算方面的压力。
2. 超过阈值和低于阈值的所有节点都被都标识。
3. 具有最高压力的节点进行标识，以确定优先级的平衡。
4. <abbr title="虚拟机">Vm</abbr>取自实时迁移 （使用无需停机时间） 超出了阈值到低于最小阈值的节点的节点。

## <a name="see-also"></a>请参阅
* [虚拟机负载平衡深度解析](vm-load-balancing-deep-dive.md)
* [故障转移群集](failover-clustering-overview.md)
* [HYPER-V 概述](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
