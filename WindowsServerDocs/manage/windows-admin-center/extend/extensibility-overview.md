---
title: Windows Admin Center 的扩展
description: Windows Admin Center SDK (Project Honolulu) 的扩展
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/17/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: ee8c0203be25b30f173b1887de506844d5b58738
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406918"
---
# <a name="extensions-for-windows-admin-center"></a>Windows Admin Center 的扩展

>适用于：Windows Admin Center、Windows Admin Center 预览版

将 Windows Admin Center 构建为一个可扩展平台的目的是：支持合作伙伴和开发人员利用 Windows Admin Center 内的现有功能、与其他 IT 管理产品和解决方案无缝集成，并向客户提供附加价值。 Windows Admin Center 中的每个解决方案和工具均使用可供合作伙伴和开发人员使用的相同可扩展功能构建为扩展，以便你可以构建如现在 Windows Admin Center 中提供的工具一样强大的工具。

Windows Admin Center 扩展使用 HTML5、CSS、Angular、TypeScript 和 jQuery 等现代的 Web 技术构建，可以通过 PowerShell 或 WMI 管理目标服务器。 你还可以通过构建 Windows Admin Center 网关插件，使用不同协议（如 REST）来管理目标服务器、服务或设备。

## <a name="why-you-should-consider-developing-an-extension-for-windows-admin-center"></a>为什么应考虑开发 Windows Admin Center 的扩展

下面是通过开发 Windows 管理中心的扩展可为产品和客户带来的价值：

- **与 Windows 管理中心工具集成：** 将你的产品和服务与 Windows 管理中心中的服务器和群集管理工具集成，并为你的客户提供统一且无缝的端到端监视、管理和故障排除体验。
- **利用平台安全、标识和管理功能：** 利用 Windows 管理中心平台功能满足当今的复杂要求，为你的产品和服务启用 Azure Active Directory （AAD）支持、多重身份验证、基于角色的访问控制（RBAC）、日志记录和审核IT 组织。
- **使用最新的 web 技术进行开发：** 使用新式 web 技术（包括 HTML5、CSS、角度、TypeScript 和 jQuery）和 Windows 管理中心 SDK 中包含的丰富、功能强大的 UI 控件，快速生成令人惊叹的用户体验。
- **扩展产品推广：** 成为新的 Windows 管理中心中心生态系统的一部分，并在今年晚些时候利用 Windows Server 2019 启动动力。

## <a name="start-developing-with-the-windows-admin-center-sdk"></a>开始通过 Windows 管理中心 SDK 进行开发

Windows 管理中心开发的入门非常简单！  可在 SDK 文档中找到[工具](develop-tool.md)、[解决方案](develop-solution.md)和[网关插件](develop-gateway-plugin.md)扩展类型的示例代码。 在这里，你将利用 Windows 管理中心 CLI 生成新的扩展项目，然后按照各个指南自定义你的项目以满足你的需求。

我们创建了一个 Windows 管理中心[SDK 设计工具包](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)，可帮助你使用 Windows 管理中心样式、控件和页面模板在 PowerPoint 中快速模拟扩展。 开始编码之前，请查看你的扩展在 Windows 管理中心中的外观。

我们还在 GitHub 上托管了示例代码：[开发人员工具](https://aka.ms/wacsdk)是一个示例解决方案扩展，其中包含一组丰富的控件，你可以在自己的扩展中浏览和使用这些控件。 开发人员工具是一个完全正常运行的扩展，可在开发人员模式下旁加载到 Windows Admin Center。

请参阅下列主题，了解有关 SDK 的详细信息并开始：

- [了解扩展的工作方式](understand-extensions.md)
- [开发扩展](developing-extensions.md)
- [指南](guides.md)
- [发布扩展](publish-extensions.md)

## <a name="partner-spotlight"></a>合作伙伴聚焦

了解我们的合作伙伴已开始为 Windows Admin Center 生态系统带来的令人惊叹的价值，并立即试用这些扩展。 了解如何安装 Windows Admin Center 的[扩展](../configure/using-extensions.md)的详细信息。

### <a name="biitops"></a>BiitOps
BiitOps 更改扩展插件为 Windows Server 物理/虚拟机上的硬件、软件和配置设置提供更改跟踪。 BiitOps 更改扩展将精确显示新增内容、更改的内容以及在单窗格中删除的内容，以帮助跟踪与符合性、可靠性和安全性相关的问题。 [了解有关 BiitOps 更改扩展的详细信息](case-studies/biitops.md)。

![BiitOps 扩展](../media/extensibility-overview/biitops-1.png)

### <a name="dataon"></a>DataON

DataON 必须扩展使监视、管理和端到端深入了解 DataON 的超聚合基础结构和基于 Windows Server 的存储系统。 必须扩展添加了唯一值，如历史数据报告、磁盘映射、系统警报和类似 SAN 的呼叫 home service、通过无缝地补充 Windows 管理中心服务器和超聚合基础结构管理功能统一体验。 [了解有关 DataON 的 MUST 扩展及其开发体验的详细信息](case-studies/dataon.md)。

![DataON MUST 扩展](../media/extensibility-overview/dataon-must-extension.png)

### <a name="fujitsu"></a>Fujitsu

适用于 Windows 管理中心的 Fujitsu 的 ServerView 运行状况和 RAID 运行状况扩展为 Fujitsu PRIMERGY 服务器的处理器、内存、电源和存储子系统等关键硬件组件提供了深入的监视和管理。 通过使用 Windows Admin Center UX 设计模式和 UI 控件，Fujitsu 已通过 Windows Admin Center 平台让我们对端到端洞察的愿景向前迈进了一大步，涉及服务器角色和服务、操作系统和硬件管理。 [了解有关 Fujitsu 的扩展及其开发体验的详细信息](case-studies/fujitsu.md)。

![Fujitsu ServerView 扩展](../media/extensibility-overview/fujitsu-serverview-extension.png)

### <a name="lenovo"></a>Lenovo

联想 XClarity 集成器扩展通过无缝集成到 Windows 管理中心内的各种体验，将硬件管理投入到下一级别。 XClarity 集成器解决方案提供所有联想服务器的高级视图，而无论你连接到单一服务器、故障转移群集还是超聚合群集，不同的工具扩展都提供硬件详细信息。 [了解有关联想 XClarity 集成器扩展的详细信息](case-studies/lenovo.md)。

![联想扩展](../media/extensibility-overview/lenovo-extension.png)

### <a name="pure-storage"></a>Pure Storage

纯粹的存储提供企业的、所有闪存的数据存储解决方案，可提供以数据为中心的体系结构，从而加快你的业务的竞争优势。 适用于 Windows 管理中心的纯粹存储扩展提供了单窗格视图，使用户能够执行监视任务、查看实时性能度量值，以及通过单一 UI 管理存储卷和发起程序接触. [了解有关纯扩展和开发体验的详细信息](case-studies/purestorage.md)。

![纯粹的存储扩展](../media/extensibility-overview/purestorage-extension.png)

### <a name="qct"></a>QCT

QCT Management Suite 扩展通过为 QCT Azure Stack HCI 认证系统提供物理服务器监视和管理，来补充 Windows 管理中心。 QCT 管理套件扩展显示服务器硬件信息，并提供直观的向导 UI，帮助你有效地替换物理磁盘、硬件事件日志工具和 S.M.A.R.T。 基于预测磁盘管理。 [了解有关 QCT 管理套件扩展的详细信息](case-studies/qct.md)。

![QCT 扩展](../media/extensibility-overview/qct-extension.png)

### <a name="squared-up"></a>Squared Up

Squared Up 基于 System Center Operations Manager 提供一流的监视体验，并与 Azure Log Analytics、Application Insights 和其他监视解决方案集成。 [Squared Up 扩展](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu)向 Windows Admin Center 提供的服务器和群集管理的环境提供历史性能数据和实时应用程序拓扑，早期客户已认可了将很多独立源的大量数据带入单一体验的价值。 [了解有关 Squared Up 的扩展及其开发体验的详细信息](case-studies/squared-up.md)。

![Squared Up 扩展](../media/extensibility-overview/squaredup-extension.png)