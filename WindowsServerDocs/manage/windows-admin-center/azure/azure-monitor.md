---
title: 监视服务器和使用 Windows Admin Center 从 Azure Monitor 配置警报
description: 与 Azure Monitor 集成，Windows Admin Center (项目 Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/24/2019
ms.openlocfilehash: 8f7baba465071cc95ab7492037ff25c5cd58219e
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280438"
---
# <a name="monitor-servers-and-configure-alerts-with-azure-monitor-from-windows-admin-center"></a>监视服务器和使用 Windows Admin Center 从 Azure Monitor 配置警报

[了解有关 Windows Admin Center 与 Azure 集成的详细信息。](../plan/azure-integration-options.md)

[Azure 监视器](https://docs.microsoft.com/azure/azure-monitor/overview)是一种解决方案，可以收集、 分析，并使用不同的资源，包括 Windows 服务器和 Vm，同时在本地和云中的遥测数据的作用。 尽管 Azure Monitor 将从 Azure Vm 和其他 Azure 资源拉取数据，这篇文章重点介绍 Azure 监视器与在本地服务器和 Vm，特别是与 Windows Admin Center 的配合方式。 如果您感兴趣，了解如何使用 Azure Monitor 获取超聚合群集有关的电子邮件警报，请阅读有关[使用 Azure Monitor 将电子邮件发送运行状况服务故障的](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor)。

## <a name="how-does-azure-monitor-work"></a>Azure Monitor 是如何工作的？
![i m g](../media/azure-monitor-diagram.png)从本地 Windows 服务器生成的数据收集在 Azure Monitor 中的 Log Analytics 工作区中。 在工作区中，可以启用各种监视解决方案-设置为特定方案提供的见解的逻辑。 例如，Azure 更新管理、 Azure 安全中心和 Vm 的 Azure Monitor 是可以在工作区中启用的所有监视解决方案。 

当启用 Log Analytics 工作区中的监视解决方案时，向报告的所有服务器该工作区将都开始收集到该解决方案相关的数据，以便该解决方案可以在工作区中生成的所有服务器的见解。 

若要在本地服务器上收集遥测数据并将其推送到 Log Analytics 工作区，Azure 监视器要求安装 Microsoft 监视代理或 MMA。 某些监视解决方案还需要辅助代理。 例如，Vm 的 Azure Monitor 还取决于用于此解决方案提供的其他功能的 ServiceMap 代理。 

某些解决方案，如 Azure 更新管理，也依赖于 Azure 自动化中，这样就可以跨 Azure 和非 Azure 环境中集中管理的资源。 例如，Azure 更新管理使用 Azure 自动化来调度和安排更新的安装你的环境中的计算机上集中，从 Azure 门户。


## <a name="how-does-windows-admin-center-enable-you-to-use-azure-monitor"></a>如何 Windows Admin Center 确实能使您使用 Azure Monitor？

从在 WAC，可以启用这两个监视解决方案：

- [Azure 更新管理](azure-update-management.md)（中的更新工具）
- Azure 监视的 Vm （在服务器设置），即虚拟机见解

您可以了解如何从这些工具使用 Azure Monitor。 如果你从未使用过 Azure 监视器之前，WAC 将自动配置 Log Analytics 工作区 （和 Azure 自动化帐户，如果需要），安装并配置目标服务器上的 Microsoft Monitoring Agent (MMA)。 它会随后安装到工作区中的相应的解决方案。 

例如，如果您首先转到要安装 Azure 更新管理的更新工具，WAC 将：

1. 在计算机上安装 MMA
2. 创建 Log Analytics 工作区和 Azure 自动化帐户 （因为在这种情况下，Azure 自动化帐户是有必要）
3. 在新创建的工作区中安装的更新管理解决方案。

如果你想要在同一服务器上添加另一个从 WAC 中的监视解决方案，WAC 只需将到该服务器连接到现有工作区中安装该解决方案。 此外，WAC 将安装任何其他必需的代理。

如果连接到其他服务器，但已设置 Log Analytics 工作区 （不管是通过 WAC 或在 Azure 门户中手动），您还可以在服务器上安装 MMA 代理并将其连接到现有工作区。 当将服务器连接到工作区中时，将自动开始收集数据并报告给该工作区中安装的解决方案。

## <a name="azure-monitor-for-virtual-machines-aka-virtual-machine-insights"></a>Azure 监视虚拟机 （也称为 虚拟机 insights)
>适用于：Windows Admin Center 预览版

当你为 Azure Monitor 的 Vm 安装在服务器设置中时，Windows Admin Center 允许 Vm 解决方案，也称为虚拟机 insights Azure Monitor。 此解决方案，可监视服务器运行状况和事件、 创建电子邮件警报，在环境中获取服务器性能的合并的视图和可视化应用程序、 系统和服务连接到给定的服务器。

> [!NOTE]
> 尽管其名称如此，VM insights 适用于物理服务器，以及虚拟机。

Azure Monitor 的免费的 5 GB 的数据/月/客户限额，您可以轻松地试用这对于服务器或两个无需担心被收费。 例如，在您的环境中的服务器获取系统性能的综合的视图，请参阅其他权益的将服务器载入到读取到 Azure Monitor。

### <a name="set-up-your-server-for-use-with-azure-monitor"></a>**在设置服务器以用于 Azure Monitor**

从服务器连接的概述页上，单击新建按钮"管理警报"，或转到服务器设置 > 监视和警报。 在此页上，载入 Azure Monitor 您服务器可以通过单击"设置"并完成设置窗格。 管理中心负责预配 Azure Log Analytics 工作区中，安装必要的代理，并确保 VM insights 解决方案配置。 完成后，你的服务器将性能计数器数据发送到 Azure Monitor，使你能够查看和创建基于此服务器，从 Azure 门户上的电子邮件警报。

### <a name="create-email-alerts"></a>**创建电子邮件警报**

一旦完成你的服务器附加到 Azure Monitor 后，可以使用在设置中的智能超链接 > 监视和警报页后，可以导航到 Azure 门户。 管理员中心会自动启用性能计数器来收集一次，因此您可以轻松地[创建新的警报](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)通过自定义其中一个许多预定义查询，或编写你自己。

### <a name="get-a-consolidated-view-across-multiple-servers-"></a>\* * 获取跨多个服务器的合并的视图 * *

如果你载入到单个 Log Analytics 工作区在 Azure Monitor 中的多个服务器，你可以获取从所有这些服务器的合并的视图[虚拟机 Insights 解决方案](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)Azure 监视器中。  （请注意仅的性能和映射选项卡适用于 Azure Monitor 的虚拟机 Insights 将使用在本地服务器 – 仅与 Azure Vm 的运行状况选项卡函数）。若要在 Azure 门户中查看它，请转到 Azure Monitor > （下 Insights)，虚拟机并导航到"性能"或"地图"选项卡。

### <a name="visualize-apps-systems-and-services-connected-to-a-given-server"></a>**可视化应用程序、 系统和服务连接到给定的服务器**

当管理员中心用于加入到 Azure Monitor 中的 VM insights 解决方案的服务器，它还方便了称作的功能[服务映射](https://docs.microsoft.com/azure/azure-monitor/insights/service-map)。 此功能会自动发现应用程序组件并映射服务之间的通信，以便可以轻松地看到大量的详细信息，从 Azure 门户的服务器之间的连接。 您可以通过转到 Azure 门户中找到此 > Azure Monitor > （下 Insights)，虚拟机并导航到"映射"选项卡。

> [!NOTE]
> Azure Monitor 的可视化效果的虚拟机 Insights 目前提供在 6 个公共区域中。  有关最新信息，请[Vm 文档的 Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics)。  您必须部署的 Log Analytics 工作区中受支持的区域，以获得其他上面所述的虚拟机 Insights 解决方案提供的优势之一。

## <a name="disabling-monitoring"></a>禁用监视

若要完全从 Log Analytics 工作区中断开您的服务器，请卸载 MMA 代理。 这意味着此服务器将无法再将数据发送到工作区中，并且所有安装在该工作区中的解决方案将无法再收集和处理来自该服务器的数据。 但是，这不影响工作区本身 – 所有报告到该工作区的资源将继续执行此操作。 若要卸载内 WAC MMA 代理，请转到应用和功能，查找 Microsoft Monitoring Agent，并单击卸载。

如果你想要关闭工作区中的特定解决方案，你将需要[从 Azure 门户中删除的监视解决方案](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution)。 删除监视解决方案意味着为将无法再生成该解决方案创建的见解_任何_向该工作区报告的服务器。 例如，如果卸载 Vm 解决方案 Azure Monitor，我将无法再看到有关的任何计算机连接到我的工作区的 VM 或服务器性能的见解。