---
title: Windows 管理中心 SDK 案例研究-Fujitsu
description: Windows 管理中心 SDK 案例研究-Fujitsu
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 9acfa873e4ce7d3e91a23abff726836f0e11ce59
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357216"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Fujitsu ServerView Health and RAID extension

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>将从操作系统到硬件的端到端可见性引入 Windows 管理中心

Fujitsu 是一家领先的日语信息和通信技术公司，以及[PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/)和[PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/)服务器产品的制造商。 [Fujitsu ServerView 管理套件](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/)为服务器生命周期管理提供了一个综合的工具集，其中包括一个服务器端代理，该代理提供用于硬件管理的 CIM 和 PowerShell 接口。

Fujitsu 看到了一种与 Windows 管理中心的集成机会，因为它提供了可与服务器端代理通信的 CIM 和 PowerShell 接口。 Fujitsu 的开发团队能够轻松地实现他们熟悉代理的 CIM 调用，并使用可用的 UI 组件在 Windows 管理中心内可视化信息。

![Fujitsu 扩展-运行状况树视图](../../media/extend-case-study-fujitsu/health-tree.png)

一旦团队熟悉 Windows 管理中心 SDK，添加 UI 以公开额外的硬件信息通常只需几行 HTML 代码，就能迅速从单个工具进行扩展以显示硬件组件的摘要视图运行状况、系统事件日志的详细视图、驱动程序监视器、处理器、内存、风扇、电源、温度和电压的单独视图，甚至用于 RAID 管理的其他工具。 使用 SDK 中提供的 UI 控件（如 "树"、"网格" 和 "详细信息" 窗格控件），团队可以快速生成 UI，还可以实现与 Windows 管理中心的其余部分类似的可视化和交互设计。

![Fujitsu 扩展-RAID 树视图](../../media/extend-case-study-fujitsu/raid-tree.png)

![Fujitsu 扩展-RAID 卷视图](../../media/extend-case-study-fujitsu/raid-volumes.png)

Fujitsu 与 Windows 管理中心团队之间的合作关系清晰地显示了 Windows 管理中心中的集成价值，使客户能够通过端到端深入了解服务器角色和服务、操作系统和硬件管理.