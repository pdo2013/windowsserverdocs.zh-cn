---
title: Windows Admin Center SDK 案例研究-Fujitsu
description: Windows Admin Center SDK 案例研究-Fujitsu
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6d916920b187dd3c637644a0f40ae9f9cca72b66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814988"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Fujitsu ServerView 运行状况和 RAID 扩展

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>从操作系统到硬件，将端到端可见性添加到 Windows Admin Center

Fujitsu 是一家领先日语信息和通信技术公司和的制造商[PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/)并[PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/)服务器产品。 [Fujitsu ServerView 管理套件](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/)服务器包括为硬件管理提供了 CIM 和 PowerShell 接口的服务器端代理的生命周期管理提供全面的工具集。

Fujitsu 发现了一个机会，因为无法进行通信的 CIM 和 PowerShell 接口提供与服务器端代理，与 Windows Admin Center 轻松地集成。 Fujitsu 的开发团队就能够轻松地实现他们已熟悉到代理的 CIM 调用和可视化内 Windows Admin Center 使用的可用 UI 组件的信息。

![Fujitsu 扩展的运行状况树视图](../../media/extend-case-study-fujitsu/health-tree.png)

一旦该小组已熟悉 Windows Admin Center SDK，添加 UI，以显示额外的硬件信息通常是只需几行的 HTML 代码，他们就快速能够通过单个工具扩展到显示的硬件组件的摘要视图运行状况，对于系统事件日志，驱动程序监视器的详细视图分隔处理器、 内存、 风扇、 电源、 温度和电压，视图和 RAID 管理甚至一个额外的工具。 使用 UI 控件树如 SDK 中提供，启用团队以便快速构建用户界面并实现非常类似于 Windows Admin Center 的其余部分的视觉和交互设计网格和详细信息窗格控件。

![Fujitsu 扩展-RAID 树视图](../../media/extend-case-study-fujitsu/raid-tree.png)

![Fujitsu 扩展-RAID 卷查看](../../media/extend-case-study-fujitsu/raid-volumes.png)

Fujitsu Windows Admin Center 团队之间的合作关系清楚地显示的值的集成 Windows Admin Center 内使客户能够以具有端到端了解服务器角色和服务，对操作系统和硬件管理.