---
title: Windows Admin Center SDK 案例研究-Thomas Krenn
description: Windows Admin Center SDK 案例研究-Thomas Krenn
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/24/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93b8a450aa86a454ec6febd349fcaa35df590266
ms.sourcegitcommit: 3be280c8638214857dc355b201eb56a04499a5e5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2019
ms.locfileid: "67396757"
---
# <a name="thomas-krennag-extension"></a>Thomas Krenn.AG 扩展

## <a name="intuitive-server-and-storage-health-management"></a>直观的服务器和存储运行状况管理

Thomas Krenn.AG Windows Admin Center 扩展专为高度可用的 2 节点[S2D Micro 群集](https://www.thomas-krenn.com/en/products/application/software-defined-storage/s2d-micro-cluster.html)设备。 用户友好的图形的 web 界面直观显示通过简单的仪表板的 Micro 群集的运行状况状态，并允许您向下钻取存储设备、 网络接口或整个群集，若要查看更多详细信息。

该扩展提供直观的访问通常所需的第一级别的服务和支持调用，例如序列号、 软件版本、 存储利用率和的详细信息。 它旨在对具有使用 Windows Server 超聚合基础结构没有经验的管理员非常有用。

几个可用见解包括：
- 有关微节点和 Micro 群集的常规信息
- OS / 启动设备状态
- 容量 HDD 和 SSD 状态缓存
- 群集事件
- 网络状态和信息

使用仪表板来确定群集的运行状况状态和重要系统信息，例如序列号、 模型、 操作系统版本和使用率。 此外，还在仪表板上显示风扇、 NIC 和总体节点硬件运行状况。

![Thomas Krenn 扩展](../../media/extend-case-study-thomas-krenn/thomas-krenn-1.png)

您可以向下钻取到存储设备，若要查看序列号、 智能的状态和容量使用量。 启动设备还显示出指示器，重新扇区和电源分配时，这是 SSD 运行状况的最好指标穿戴设备。

![Thomas Krenn 扩展](../../media/extend-case-study-thomas-krenn/thomas-krenn-2.png)

群集状态图标展开以显示群集的操作的详细信息的摘要。

![Thomas Krenn 扩展](../../media/extend-case-study-thomas-krenn/thomas-krenn-3.png)

此的 Micro 群集的 Azure 云见证服务器处于不可用的整个晚上后，快速浏览就足以确定问题。 立即单击"通知"列出了用于快速修补的相关事件。 群集事件本地化，然后由基本的操作系统语言。 扩展本身支持英语和德语。

![Thomas Krenn 扩展](../../media/extend-case-study-thomas-krenn/thomas-krenn-4.png)

网络信息也是随时可用。

![Thomas Krenn 扩展](../../media/extend-case-study-thomas-krenn/thomas-krenn-5.png)

根据客户反馈，我们还实现了 Windows Admin Center v1904 中提供的"深色模式"。 这是舒缓深数据中心和效果不佳亮起 cabinet 文件和的储物柜中。 它还使 Windows Admin Center 更易于访问通过减少炫光有某些视觉障碍的管理员。

![Thomas Krenn 扩展](../../media/extend-case-study-thomas-krenn/thomas-krenn-6.png)

Thomas Krenn 立即实现可用性和可访问性的未训练的管理员将出色的客户体验在小型和中型企业市场中的超聚合基础结构的键。 Thomas-Krenn Micro 群集扩展完全可以通过包括在仪表板上的专用硬件信息和重新分组中的新的重要的群集运行状况信息补充 Windows Admin Center 本机 HCI 管理功能用户友好界面。

在开发过程中它决定在上群集本身，确保即使在节点故障后的可管理性的高可用性配置中部署 Windows Admin Center 1904年。 该扩展可能已预安装，就像整个 OS。

扩展是构建与 Microsoft 开发的 Windows Admin Center 1904年并行的。 关闭的协作和持续反馈公开双方共同解决之前已成功在 2019 年 4 月推出的产品的问题。 Thomas Krenn 是非常自豪地为第一个完全支持和实现 Windows Admin Center 1904年新功能之一。
