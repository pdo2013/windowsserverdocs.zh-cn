---
title: 监视服务器，并使用从 Windows Admin Center 的 Azure 监视器配置警报
description: 与 Azure 监视器集成 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/24/2019
ms.openlocfilehash: 6ada708bf7dd8cd08e1bc2620be5244a07beac7d
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296884"
---
# 监视服务器，并使用从 Windows Admin Center 的 Azure 监视器配置警报

[了解有关 Azure 与 Windows Admin Center 集成的详细信息。](../plan/azure-integration-options.md)

[Azure 监视器](https://docs.microsoft.com/azure/azure-monitor/overview)是收集、 分析和作用于来自各种资源，包括 Windows 服务器和虚拟机，在本地和云中的遥测的解决方案。 Azure 监视 Azure 虚拟机和其他 Azure 资源中提取数据，但本文重点介绍使用本地服务器和虚拟机，特别是使用 Windows Admin Center 的 Azure 监视工作原理。 如果你不感兴趣，若要了解如何使用 Azure 监视器以获取有关超聚合群集的电子邮件警报，阅读有关[使用 Azure 监视器发送电子邮件的运行状况服务错误](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor)。

## Azure 监视器是如何工作的？
![img](../media/azure-monitor-diagram.png)从本地 Windows Server 中生成的数据收集在 Azure 监视器在 Log Analytics 工作区中。 在工作区中，你可以启用各种监视解决方案-适用于特定方案提供见解的逻辑的设置。 例如，Azure 更新管理、 Azure 安全中心和 Azure 虚拟机监视是可以启用的工作区中的所有监视解决方案。 

当你启用在 Log Analytics 工作区，监视解决方案报告至该工作区的所有服务器将开始收集数据的相关的解决方案，以便解决方案可以在工作区中生成的所有服务器的见解。 

在本地服务器上收集遥测数据并将其推送至 Log Analytics 工作区，Azure 监视器需要安装 Microsoft Monitoring Agent 或 MMA。 某些监视解决方案还需要辅助代理。 例如，虚拟机的 Azure 监视器也依赖于此解决方案可提供的其他功能的 ServiceMap 代理。 

某些解决方案，如 Azure 更新管理，还取决于 Azure 自动化，使你能够跨 Azure 和非 Azure 环境来集中管理资源。 例如，Azure 更新管理使用 Azure 自动化计划和协调更新的安装在你的环境中的计算机上集中，在 Azure 门户中。


## 如何 Windows Admin Center 会使你能够使用 Azure 监视器？

从内 WAC，你可以启用两个监视解决方案：

- [Azure 更新管理](azure-update-management.md)（在更新工具中）
- （在服务器设置中） 的虚拟机的 azure 监视、 a.k.a 虚拟机见解

你可以开始使用 Azure 监视从这些工具之一。 如果你从未使用过 Azure 监视器之前，WAC 将自动预配 Log Analytics 工作区 （和 Azure 自动化帐户，如果需要） 并安装和目标服务器上配置 Microsoft Monitoring Agent (MMA)。 然后，它将将相应的解决方案安装到工作区。 

例如，如果你先转到更新工具设置 Azure 更新管理，WAC 将：

1. 在计算机上安装 MMA
2. 创建 Log Analytics 工作区和 Azure 自动化帐户 （因为 Azure 自动化帐户需要在此情况下）
3. 在新建工作区中安装的更新管理解决方案。

如果你想要在同一台服务器添加 WAC 内从的其他监视解决方案，WAC 只需将到该服务器连接到现有工作区中安装该解决方案。 此外，WAC 将安装其他任何必要的代理。

如果你连接到其他服务器上，但已设置为 Log Analytics 工作区 （无论是通过 WAC 或手动在 Azure 门户中），还可以在服务器上安装 MMA 代理中，然后将其连接到现有工作区中。 当将服务器连接到工作区中时，将自动开始收集数据，并且报告给该工作区中安装的解决方案。

## Azure 监视的虚拟机 （也称为 虚拟机见解）
>适用于： Windows Admin Center 预览版

当你中设置了 Azure 监视器 Vm 的服务器设置时，Windows Admin Center 支持 Azure 显示器上的虚拟机解决方案，也称为虚拟机见解。 此解决方案可以监视服务器运行状况和事件、 创建电子邮件通知，在你的环境中获取服务器性能的统一的视图和可视化应用、 系统和连接到给定服务器的服务。

> [!NOTE]
> 尽管名称，VM 见解适用于物理服务器，以及虚拟机。

使用 Azure 监视器的免费 5 GB 的数据/月/客户余量，你可以轻松地试用这对于服务器或两个而无需担心获取收费。 如你的环境中的服务器上获取系统性能的统一的视图，请参阅的载入服务器的其他好处继续阅读到 Azure 监视器。

### **设置服务器以供与 Azure 监视器**

从服务器连接概述页面中，单击新建按钮"管理警报"，或转到服务器设置 > 监视和警报。 在此页面上，载入到 Azure 监视器服务器通过单击"设置"并完成设置窗格。 Admin Center 可以进行预配 Azure Log Analytics 工作区中，安装必要的代理，并确保 VM 见解解决方案配置。 完成后，你的服务器将发送性能计数器数据到 Azure 监视器，使你能够查看和创建基于在此服务器上，在 Azure 门户中的电子邮件通知。

### **创建电子邮件警报**

一旦你已连接到 Azure 监视你的服务器，你可以使用设置 > 监视和警报页面内的智能超链接导航到 Azure 门户。 管理中心自动启用要收集，以便你可以轻松地[创建新警报](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)的自定义多个预定义的查询，之一或编写自己的性能计数器。

### * * 在多个服务器获取统一的视图 * *

如果你载入到单个 Log Analytics 工作区，Azure 监视程序中的多个服务器，你可以获取所有这些服务器的统一的视图从[虚拟机见解解决方案](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)Azure 监视程序中。  （请注意只有性能和地图选项卡的虚拟机见解 Azure 监视器将使用本地服务器 – 仅与 Azure 虚拟机的运行状况选项卡功能）。若要在 Azure 门户中查看此，请转到 Azure 监视器 > （下见解），虚拟机并导航到"性能"或"地图"选项卡。

### **可视化应用、 系统和服务连接到给定服务器**

当管理中心 onboards 到 VM 见解解决方案 Azure 监视程序中的服务器，它还方便了调用[服务映射](https://docs.microsoft.com/azure/azure-monitor/insights/service-map)的功能。 此功能自动发现应用程序组件和地图服务之间的通信，以便你可以轻松地将可视化与大量的详细信息在 Azure 门户中的服务器之间的连接。 可以通过转到 Azure 门户 > Azure 监视器 > （下见解），虚拟机并导航到"地图"选项卡中找到此数据。

> [!NOTE]
> Azure 监视应用于虚拟机洞察可视化当前提供 6 个公共地区。  最新信息，请检查[Azure 监视虚拟机文档](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics)。  你必须部署中受支持的区域，以获取由上面所述的虚拟机见解解决方案提供的其他优势之一的 Log Analytics 工作区。

## 禁用监视

若要完全从 Log Analytics 工作区断开你的服务器，卸载 MMA 代理。 这意味着此服务器不会再将数据发送到工作区中，并且所有安装在该工作区的解决方案将无法再收集并处理来自该服务器的数据。 但是，这不会影响工作区本身 – 所有报告至该工作区的资源将继续执行此操作。 若要卸载 MMA 代理 WAC 内的，转到应用 & 功能、 查找 Microsoft Monitoring Agent，然后单击卸载。

如果你想要关闭特定解决方案工作区中，你将需要[删除从 Azure 门户监视解决方案](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution)。 删除监视解决方案意味着见解创建的_任何_报告至该工作区的服务器为不会再生成解决方案。 例如，如果卸载 Azure 显示器上的虚拟机解决方案，我将不会再看到有关从任何连接到我的工作区的计算机的虚拟机或服务器性能的见解。