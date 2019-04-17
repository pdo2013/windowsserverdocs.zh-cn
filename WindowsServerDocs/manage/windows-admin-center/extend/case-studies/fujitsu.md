---
title: Windows Admin Center SDK 案例研究-富士通
description: Windows Admin Center SDK 案例研究-富士通
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6d916920b187dd3c637644a0f40ae9f9cca72b66
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "2052371"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>富士通 ServerView 运行状况和 RAID 扩展

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>从到硬件，操作系统的端到端可见性，将置于 Windows 管理中心

前导日语信息和通信技术公司[PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/)和[PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/)服务器产品的制造商富士通。 [富士通 ServerView 管理套件](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/)提供了服务器生命周期管理包括为硬件管理提供 CIM 和 PowerShell 界面的服务器端代理的全面的工具集。

富士通看到如 CIM 和 PowerShell 无法进行通信的接口附带的服务器端代理，与 Windows 管理中心轻松集成的机会。 开发团队在富士通就能够轻松地实现它们是一样的代理熟悉 CIM 调用和可视化中使用的可用的用户界面组件的 Windows Admin Center 此信息。

![富士通扩展-运行状况树视图](../../media/extend-case-study-fujitsu/health-tree.png)

一旦团队变为熟悉 Windows Admin Center SDK，添加用户界面来公开其他硬件信息已通常只需几行的 HTML 代码和他们都快速能够从单个工具扩展到显示硬件组件的摘要视图运行状况，系统事件日志，驱动程序监视器的详细视图分隔的处理器、 内存、 风扇、 电源、 温度和电压，视图和甚至附加工具用于 RAID 管理。 使用如树 SDK 中提供的用户界面控件，网格和详细信息窗格中控制已启用团队来快速构建 UI 和还实现可视和交互的设计非常类似于 Windows Admin Center 的其余部分。

![富士通扩展-RAID 树视图](../../media/extend-case-study-fujitsu/raid-tree.png)

![富士通扩展-RAID 卷查看](../../media/extend-case-study-fujitsu/raid-volumes.png)

之间富士通和 Windows Admin Center 团队合作关系清楚地显示值集成的 Windows 管理中心中，在启用客户端到端深入了解服务器角色和服务，向操作系统和硬件管理.