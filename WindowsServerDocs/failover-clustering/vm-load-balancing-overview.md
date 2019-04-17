---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: "虚拟机负载平衡概述"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 0a106db25d476088898b914481e6041f20ce2e9e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="virtual-machine-load-balancing-overview"></a>虚拟机负载平衡概述

> 适用于：Windows Server（半年通道）、Windows Server 2016

关键专用云部署注意事项是首都支出 (<abbr title="首都支出">CapEx</abbr>) 需要转到生产。 它是专用云部署在高峰交通投入使用，以免的容量不足到添加冗余很常见，但这会增加<abbr title="首都支出">CapEx</abbr>。 需要冗余驱动通过不均衡专用海角，其中一些节点承载更多虚拟机 (<abbr title="虚拟机">虚拟机的功能</abbr>) 和其他得到充分利用 （例如服务器新鲜已重新启动）。

## <a id="what-is-vm-load-balancing"></a>什么是虚拟机负载平衡？
<abbr title="虚拟机">VM</abbr>负载平衡是 Windows Server 2016，以便你可以优化利用率百分比故障转移群集节点中的收件箱新功能。 它标识过度提交的节点，然后重新分发<abbr title="虚拟机">虚拟机的功能</abbr>从下提交节点这些节点。 此功能的突出方面的某些如下所示：

* *它是一个零宕机解决方案*:<abbr title="虚拟机">虚拟机的功能</abbr>实时迁移到空闲节点。
* *与你现有的群集环境无缝集成*： 将生效，但失败策略，如防关联故障域，可能的所有者。
* *对于平衡启发*:<abbr title="虚拟机">VM</abbr>内存压力并节点的 CPU 使用率。
* *精确地控制*： 默认启用。 按需或定期可激活。
* *进取阈值*： 三个阈值提供基于你部署的特征。

## <a id="feature-in-action"></a>操作功能
### <a id="new-node-added"></a>新的节点添加到你的故障转移群集
![添加到你的故障转移群集节点新的图形](media/vm-load-balancing/overview-VM-load-balancing-1.png)

当你将新的容量添加到你故障转移群集，<abbr title="虚拟机">VM</abbr>负载平衡功能自动余额容量从现有节点到新添加的节点按以下顺序：

1. 在故障转移群集节点现有上计算压力。
2. 标识超过 threshold 所有节点。
3. 最高的压力节点标识来确定平衡的优先级。
4. <abbr title="虚拟机">虚拟机的功能</abbr>实时迁移 （对有时间否） 从超过到新添加在故障转移群集节点 threshold 节点。

### <a id="recurring-load-balancing"></a>定期负载平衡
![定期虚拟机的图形负载平衡](media/vm-load-balancing/overview-VM-load-balancing-2.png)

配置定期平衡，当群集节点压力计算平衡每隔 30 分钟。 或者，压力可以点播计算。 下面介绍流程的步骤：

1. 在专用云中的所有节点计算压力。
2. 标识超过 threshold 以及下方 threshold 所有节点。
3. 最高的压力节点标识来确定平衡的优先级。
4. <abbr title="虚拟机">虚拟机的功能</abbr>实时迁移 （对有时间否） 从超过最低 threshold 下节点到 threshold 节点。

## <a name="see-also"></a>请参阅
* [虚拟机负载平衡深层](vm-load-balancing-deep-dive.md)
* [故障转移群集](failover-clustering-overview.md)
* [Hyper V 概述](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
