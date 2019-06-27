---
title: Windows Admin Center 的扩展
description: Windows Admin Center SDK (Project Honolulu) 的扩展
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/17/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: beb2b3d1eefc5d70e39baa461708938ac9c17be5
ms.sourcegitcommit: 3be280c8638214857dc355b201eb56a04499a5e5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2019
ms.locfileid: "67396692"
---
# <a name="extensions-for-windows-admin-center"></a>Windows Admin Center 的扩展

>适用于：Windows Admin Center，Windows Admin Center 预览版

将 Windows Admin Center 构建为一个可扩展平台的目的是：支持合作伙伴和开发人员利用 Windows Admin Center 内的现有功能、与其他 IT 管理产品和解决方案无缝集成，并向客户提供附加价值。 Windows Admin Center 中的每个解决方案和工具均使用可供合作伙伴和开发人员使用的相同可扩展功能构建为扩展，以便你可以构建如现在 Windows Admin Center 中提供的工具一样强大的工具。

Windows Admin Center 扩展使用 HTML5、CSS、Angular、TypeScript 和 jQuery 等现代的 Web 技术构建，可以通过 PowerShell 或 WMI 管理目标服务器。 你还可以通过构建 Windows Admin Center 网关插件，使用不同协议（如 REST）来管理目标服务器、服务或设备。

## <a name="why-you-should-consider-developing-an-extension-for-windows-admin-center"></a>为什么应考虑开发 Windows Admin Center 的扩展

下面是可以通过开发 Windows Admin Center 的扩展为你的产品和客户提供的价值：

- **与 Windows Admin Center 工具集成：** 与 Windows Admin Center 中的服务器和群集管理工具集成，你的产品和服务并提供统一和无缝的端到端监视、 管理、 故障排除体验为您的客户。
- **利用平台安全、 标识和管理功能：** 启用 Azure Active Directory (AAD) 支持，多重身份验证，基于角色的访问控制 (RBAC)，日志记录、 审核你的产品和服务通过利用 Windows Admin Center 平台功能，以满足当今的复杂要求IT 组织。
- **使用最新的 web 技术进行开发：** 快速构建使用现代 web 技术，包括 HTML5、 CSS、 Angular、 TypeScript 和 jQuery，令人惊叹的用户体验和丰富、 功能强大的 Windows Admin Center SDK 中包含的 UI 控件。
- **扩展产品覆盖范围：** 今年晚些时候成为推广到我们的快速增长客户群和利用 Windows Server 2019 启动动量的新 Windows Admin Center 生态系统的一部分。

## <a name="start-developing-with-the-windows-admin-center-sdk"></a>开始使用 Windows Admin Center SDK 进行开发

获取 Windows Admin Center 开发的入门非常容易 ！  示例代码可找到有关[工具](develop-tool.md)，[解决方案](develop-solution.md)，并[网关插件](develop-gateway-plugin.md)我们 SDK 文档中的扩展类型。 存在将利用 Windows Admin Center CLI 以生成新的扩展插件项目，然后按照单独的指南，以自定义项目以满足你的需求。

我们让 Windows Admin Center [SDK 设计工具包](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)可帮助您快速模拟在 PowerPoint 中使用 Windows Admin Center 样式、 控件和页面模板的扩展。 请参阅什么扩展看起来像是在 Windows Admin Center 中开始编码之前 ！

我们还提供了托管在 GitHub 上示例代码：[开发人员工具](https://aka.ms/wacsdk)是包含一系列丰富的控件，您可以浏览并在自己的扩展中使用的示例解决方案扩展。 开发人员工具是一个完全正常运行的扩展，可在开发人员模式下旁加载到 Windows Admin Center。

请参阅下列主题，了解有关 SDK 的详细信息并开始：

- [了解如何扩展工作](understand-extensions.md)
- [开发扩展](developing-extensions.md)
- [指南](guides.md)
- [发布扩展](publish-extensions.md)

## <a name="partner-spotlight"></a>合作伙伴聚焦

了解我们的合作伙伴已开始为 Windows Admin Center 生态系统带来的令人惊叹的价值，并立即试用这些扩展。 了解如何安装 Windows Admin Center 的[扩展](../configure/using-extensions.md)的详细信息。

### <a name="biitops"></a>BiitOps
BiitOps 更改扩展提供了更改跟踪，将 Windows Server 物理/虚拟机的硬件、 软件和配置设置。 BiitOps 更改扩展将显示准确地说有哪些新增、 所发生的更改和单一窗格-的-虚拟管理平台来帮助跟踪问题中已被删除，什么相关法规遵从性、 可靠性和安全性。 [了解有关 BiitOps 更改扩展的详细信息](case-studies/biitops.md)。

![BiitOps 扩展](../media/extensibility-overview/biitops-1.png)

### <a name="dataon"></a>DataON

监视、 管理和端到端了解 DataON 的超聚合基础结构和存储系统基于 Windows Server 带来了 DataON 必须扩展。 必须扩展添加唯一的值如历史数据报表、 磁盘映射、 系统警报和类似于 SAN 的调用主服务，借助 Windows Admin Center 服务器和超聚合基础结构管理功能，通过无缝的统一的体验。 [了解有关 DataON 的 MUST 扩展及其开发体验的详细信息](case-studies/dataon.md)。

![DataON MUST 扩展](../media/extensibility-overview/dataon-must-extension.png)

### <a name="fujitsu"></a>Fujitsu

Fujitsu 的 ServerView 运行状况和对 Windows Admin Center RAID 运行状况扩展为 Fujitsu PRIMERGY 服务器提供深入的监视和管理关键硬件组件，如处理器、 内存、 能力和存储子系统。 通过使用 Windows Admin Center UX 设计模式和 UI 控件，Fujitsu 已通过 Windows Admin Center 平台让我们对端到端洞察的愿景向前迈进了一大步，涉及服务器角色和服务、操作系统和硬件管理。 [了解有关 Fujitsu 的扩展及其开发体验的详细信息](case-studies/fujitsu.md)。

![Fujitsu ServerView 扩展](../media/extensibility-overview/fujitsu-serverview-extension.png)

### <a name="lenovo"></a>Lenovo

Lenovo XClarity 集成器扩展通过无缝集成到 Windows Admin Center 中的各种体验将硬件管理带到下一级别。 XClarity 系统集成商解决方案提供的所有 Lenovo 服务器、 简要概述和不同的工具扩展提供硬件的详细信息，无论您连接到一台服务器、 故障转移群集或超聚合群集。 [了解有关 Lenovo XClarity 系统集成商扩展的详细信息](case-studies/lenovo.md)。

![Lenovo 扩展](../media/extensibility-overview/lenovo-extension.png)

### <a name="pure-storage"></a>Pure Storage

纯粹的存储提供了企业，提供以数据为中心的体系结构，以提升你的业务的竞争优势的所有闪存数据存储解决方案。 适用于 Windows Admin Center 的纯存储扩展提供到纯 FlashArray 的产品中的单一窗格视图，使用户能够执行监视任务、 查看实时性能度量值和管理存储卷和通过单一用户界面的发起程序体验。 [了解有关纯的扩展和他们的开发体验的详细信息](case-studies/purestorage.md)。

![纯存储扩展](../media/extensibility-overview/purestorage-extension.png)

### <a name="qct"></a>QCT

QCT Management Suite 扩展是通过物理服务器监视和管理 QCT Azure Stack HCI 认证的系统提供的补充 Windows Admin Center。 QCT Management Suite 扩展显示服务器硬件信息，并提供直观的向导 UI，以帮助将物理磁盘有效地，硬件事件日志工具和 S.M.A.R.T. 基于预测磁盘管理。 [了解有关 QCT Management Suite 扩展](case-studies/qct.md)。

![QCT 扩展](../media/extensibility-overview/qct-extension.png)

### <a name="squared-up"></a>Squared Up

Squared Up 基于 System Center Operations Manager 提供一流的监视体验，并与 Azure Log Analytics、Application Insights 和其他监视解决方案集成。 [Squared Up 扩展](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu)向 Windows Admin Center 提供的服务器和群集管理的环境提供历史性能数据和实时应用程序拓扑，早期客户已认可了将很多独立源的大量数据带入单一体验的价值。 [了解有关 Squared Up 的扩展及其开发体验的详细信息](case-studies/squared-up.md)。

![Squared Up 扩展](../media/extensibility-overview/squaredup-extension.png)