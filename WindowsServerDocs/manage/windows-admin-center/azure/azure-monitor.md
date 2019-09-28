---
title: 通过 Windows 管理中心的 Azure Monitor 监视服务器和配置警报
description: Windows 管理中心（Project Honolulu）与 Azure Monitor 集成
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 03/24/2019
ms.openlocfilehash: 28108a79bbdc654f6437a698c158a3f74d4423ba
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407018"
---
# <a name="monitor-servers-and-configure-alerts-with-azure-monitor-from-windows-admin-center"></a>通过 Windows 管理中心的 Azure Monitor 监视服务器和配置警报

[了解有关 Azure 与 Windows 管理中心的集成的详细信息。](../plan/azure-integration-options.md)

[Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview)是一种解决方案，可从各种资源（包括在本地和云中的 Windows 服务器和 vm）收集、分析和处理遥测数据。 尽管 Azure Monitor 从 Azure Vm 和其他 Azure 资源拉取数据，本文重点介绍 Azure Monitor 与本地服务器和 Vm 一起工作的方式，尤其是使用 Windows 管理中心。 如果你有兴趣了解如何使用 Azure Monitor 获取有关超聚合群集的电子邮件警报，请参阅[使用 Azure Monitor 发送电子邮件以运行状况服务错误](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor)。

## <a name="how-does-azure-monitor-work"></a>Azure Monitor 是如何工作的？
在 Azure Monitor 的 Log Analytics 工作区中收集从本地 Windows Server 生成的 @no__t 0img @ no__t 数据。 在工作区中，你可以启用各种监视解决方案，这是为特定方案提供见解的逻辑集。 例如，Azure 更新管理、Azure 安全中心和用于 VM 的 Azure Monitor 都是可在工作区中启用的所有监视解决方案。 

在 Log Analytics 工作区中启用监视解决方案时，所有向该工作区报告的服务器都将开始收集与该解决方案相关的数据，以便解决方案可以为工作区中的所有服务器生成见解。 

若要在本地服务器上收集遥测数据并将其推送到 Log Analytics 工作区，Azure Monitor 要求安装 Microsoft Monitoring Agent 或 MMA。 某些监视解决方案还需要辅助代理。 例如，用于 VM 的 Azure Monitor 还依赖于 ServiceMap 代理获取此解决方案提供的其他功能。 

某些解决方案（如 Azure 更新管理）还依赖于 Azure 自动化，使你能够跨 Azure 和非 Azure 环境集中管理资源。 例如，Azure 更新管理使用 Azure 自动化在环境中从 Azure 门户集中计划和安排更新的安装。


## <a name="how-does-windows-admin-center-enable-you-to-use-azure-monitor"></a>Windows 管理中心如何允许你使用 Azure Monitor？

在 WAC 中，你可以启用两个监视解决方案：

- [Azure 更新管理](azure-update-management.md)（在更新工具中）
- 用于 VM 的 Azure Monitor （在服务器设置中），即虚拟机见解

你可以开始使用这些工具中的 Azure Monitor。 如果以前从未使用过 Azure Monitor，则 WAC 会自动预配 Log Analytics 工作区（和 Azure 自动化帐户，如果需要），并在目标服务器上安装和配置 Microsoft Monitoring Agent （MMA）。 然后，它会将相应的解决方案安装到工作区中。 

例如，如果你首次中转到更新工具来设置 Azure 更新管理，WAC 将：

1. 在计算机上安装 MMA
2. 创建 Log Analytics 工作区和 Azure 自动化帐户（因为在这种情况下，必须使用 Azure 自动化帐户）
3. 在新创建的工作区中安装更新管理解决方案。

如果要在同一台服务器上的 WAC 中添加另一个监视解决方案，WAC 只需将该解决方案安装到该服务器所连接到的现有工作区中。 WAC 将另外安装其他任何所需的代理。

如果连接到其他服务器，但已通过 WAC 或在 Azure 门户中手动设置了 Log Analytics 工作区，则还可以在服务器上安装 MMA 代理并将其连接到现有工作区。 将服务器连接到工作区时，它会自动开始收集数据并报告到该工作区中安装的解决方案。

## <a name="azure-monitor-for-virtual-machines-aka-virtual-machine-insights"></a>虚拟机的 Azure Monitor （也称为 虚拟机见解）
>适用于：Windows Admin Center 预览版

当你在 "服务器设置" 中设置用于 VM 的 Azure Monitor 时，Windows 管理中心会启用用于 VM 的 Azure Monitor 解决方案，也称为虚拟机 insights。 此解决方案允许你监视服务器运行状况和事件、创建电子邮件警报、在整个环境中获取服务器性能的合并视图，以及直观显示连接到给定服务器的应用程序、系统和服务。

> [!NOTE]
> 不管名称如何，VM insights 都适用于物理服务器和虚拟机。

通过 Azure Monitor 的可用数据/月/客户额度，可以轻松地在一台或两台服务器上试用，无需担心。 继续阅读以了解载入服务器 Azure Monitor 的其他好处，如在环境中跨服务器获取系统性能的综合视图。

### <a name="set-up-your-server-for-use-with-azure-monitor"></a>**设置服务器以用于 Azure Monitor**

在服务器连接的 "概述" 页中，单击 "新建" 按钮 "管理警报"，或 "> 监视和警报" 中转到 "服务器设置"。 在此页中，通过单击 "设置" 并完成安装窗格，使你的服务器 Azure Monitor。 管理中心负责预配 Azure Log Analytics 工作区、安装所需的代理，并确保配置了 VM insights 解决方案。 完成后，服务器会将性能计数器数据发送到 Azure Monitor，使你能够从 Azure 门户中查看和创建基于此服务器的电子邮件警报。

### <a name="create-email-alerts"></a>**创建电子邮件警报**

将服务器连接到 Azure Monitor 后，可以使用 "设置 > 监视和警报" 页中的智能超链接导航到 Azure 门户。 管理中心自动启用要收集的性能计数器，因此你可以通过自定义多个预定义的查询之一或编写你自己的查询来轻松[创建新的警报](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)。

### <a name="get-a-consolidated-view-across-multiple-servers-"></a>\* * 跨多个服务器获取合并视图 * *

如果将多个服务器集成到 Azure Monitor 中的单个 Log Analytics 工作区，则可以从 Azure Monitor 内的[虚拟机 Insights 解决方案](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)获取所有这些服务器的合并视图。  （请注意，仅 Azure Monitor 的虚拟机见解的 "性能" 和 "映射" 选项卡可用于本地服务器– "运行状况" 选项卡仅适用于 Azure Vm。）若要在 Azure 门户中查看此信息，请转到 "Azure Monitor" > 虚拟机 "，并导航到" 性能 "或" 映射 "选项卡。

### <a name="visualize-apps-systems-and-services-connected-to-a-given-server"></a>**可视化连接到给定服务器的应用、系统和服务**

当管理中心将服务器加入到 Azure Monitor 中的 VM insights 解决方案中时，它还会亮起称为[服务映射](https://docs.microsoft.com/azure/azure-monitor/insights/service-map)功能。 此功能自动发现应用程序组件并映射服务之间的通信，以便您可以轻松地直观显示服务器之间的连接以及 Azure 门户的详细信息。 可以通过转到 "Azure 门户 > Azure Monitor" > "虚拟机"，并导航到 "映射" 选项卡来找到此信息。

> [!NOTE]
> 目前，有6个公共区域提供用于 Azure Monitor 的虚拟机见解的可视化。  有关最新信息，请查看[用于 VM 的 Azure Monitor 文档](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics)。  您必须将 Log Analytics 工作区部署在一个受支持的区域中，以获得上述虚拟机 Insights 解决方案提供的其他优势。

## <a name="disabling-monitoring"></a>禁用监视

若要从 Log Analytics 工作区完全断开服务器的连接，请卸载 MMA 代理。 这意味着，此服务器将不再向工作区发送数据，并且在该工作区中安装的所有解决方案将不再从该服务器收集和处理数据。 但是，这不会影响工作区本身–向该工作区报告的所有资源将继续执行此操作。 若要卸载 WAC 中的 MMA 代理，请 & 功能中转到应用，找到 Microsoft Monitoring Agent，并单击 "卸载"。

如果要关闭工作区中的特定解决方案，需要[从 Azure 门户中删除该监视解决方案](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution)。 删除监视解决方案意味着将不再为向该工作区报告的_任何_服务器生成该解决方案创建的 insights。 例如，如果我卸载用于 VM 的 Azure Monitor 解决方案，我将不再从连接到我的工作区的任何计算机中看到有关 VM 或服务器性能的见解。