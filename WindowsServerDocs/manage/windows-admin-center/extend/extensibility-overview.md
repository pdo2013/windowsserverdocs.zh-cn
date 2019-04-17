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
ms.openlocfilehash: fa3d7e75b32f0195346e58db54b7932c8d2fd3b9
ms.sourcegitcommit: 659544db1e19d6eecc52c7de07116ae735280544
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2019
ms.locfileid: "9001839"
---
# Windows Admin Center 的扩展

>适用于：Windows Admin Center、Windows Admin Center 预览版

将 Windows Admin Center 构建为一个可扩展平台的目的是：支持合作伙伴和开发人员利用 Windows Admin Center 内的现有功能、与其他 IT 管理产品和解决方案无缝集成，并向客户提供附加价值。 Windows Admin Center 中的每个解决方案和工具均使用可供合作伙伴和开发人员使用的相同可扩展功能构建为扩展，以便你可以构建如现在 Windows Admin Center 中提供的工具一样强大的工具。

Windows Admin Center 扩展使用 HTML5、CSS、Angular、TypeScript 和 jQuery 等现代的 Web 技术构建，可以通过 PowerShell 或 WMI 管理目标服务器。 你还可以通过构建 Windows Admin Center 网关插件，使用不同协议（如 REST）来管理目标服务器、服务或设备。

## 为什么应考虑开发 Windows Admin Center 的扩展

下面是可以通过开发 Windows Admin Center 的扩展为你的产品和客户提供的价值：

- **与 Windows Admin Center 工具集成：** 将你的产品和服务与 Windows Admin Center 中的服务器和群集管理工具集成，向客户提供统一、无缝的端到端监视、管理、疑难解答体验。
- **利用平台安全、身份和管理功能：** 让 Azure Active Directory (AAD) 通过利用 Windows Admin Center 平台功能来满足当今 IT 组织的复杂要求，来支持多重身份验证、基于角色的访问控制 (RBAC)、日志记录、产品和服务审核。
- **使用最新的 Web 技术开发：** 使用现代的 Web 技术（包括 HTML5、CSS、Angular、TypeScript 和 jQuery）以及 Windows Admin Center SDK 中包含的丰富、强大的 UI 控件来快速构建出色的用户体验。
- **扩大产品推广：** 通过向我们快速增长的客户群推广产品，加入新的 Windows Admin Center 生态系统，利用今年晚些时候 Windows Server 2019 的启动动力。

## 开始使用 Windows Admin Center SDK 进行开发

Windows Admin Center 开发入门很容易 ！  可以找到我们 SDK 文档中的[工具](develop-tool.md)、[解决方案](develop-solution.md)和[网关插件](develop-gateway-plugin.md)扩展类型的示例代码。 存在将利用 Windows Admin 中心 CLI 生成新的扩展项目，然后按照自定义项目以满足你需求的单个指南。

使用 Windows Admin Center 样式、 控件和页面模板，我们已进行[SDK 设计工具包](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)可帮助你快速地模仿中 PowerPoint 扩展 Windows Admin Center。 了解你的扩展可以在 Windows Admin Center 中开始编码前 ！

我们还可以托管在 GitHub 上的示例代码：[开发人员工具](https://aka.ms/wacsdk)是包含一丰富的控件，你可以浏览和使用自己的扩展中的一个示例解决方案扩展。 开发人员工具是一个完全正常运行的扩展，可在开发人员模式下旁加载到 Windows Admin Center。

请参阅下列主题，了解有关 SDK 的详细信息并开始：

- [了解扩展的工作原理](understand-extensions.md)
- [开发扩展](developing-extensions.md)
- [指南](guides.md)
- [发布扩展](publish-extensions.md)

## 合作伙伴聚焦

了解我们的合作伙伴已开始为 Windows Admin Center 生态系统带来的令人惊叹的价值，并立即试用这些扩展。 了解如何安装 Windows Admin Center 的[扩展](../configure/using-extensions.md)的详细信息。

### DataON

DataON 的 MUST 扩展带来了监视、 管理和端到端洞察 DataON 的超聚合基础架构和存储系统基于 Windows Server。 MUST 扩展添加如历史数据报告、 磁盘映射、 系统警报和 SAN 类似呼叫家庭服务，以 Windows Admin Center server 和超级集成基础架构管理功能，通过无缝的唯一值统一的体验。 [了解有关 DataON 的 MUST 扩展及其开发体验的详细信息](case-studies/dataon.md)。

![DataON MUST 扩展](../media/extensibility-overview/dataon-must-extension.png)

### Fujitsu

面向 Windows Admin Center 的 Fujitsu 的 ServerView 运行状况和 RAID 运行状况扩展提供对关键硬件组件（如 Fujitsu PRIMERGY 服务器的处理器、内存、电源和存储子系统）的深入监视和管理。 通过使用 Windows Admin Center UX 设计模式和 UI 控件，Fujitsu 已通过 Windows Admin Center 平台让我们对端到端洞察的愿景向前迈进了一大步，涉及服务器角色和服务、操作系统和硬件管理。 [了解有关 Fujitsu 的扩展及其开发体验的详细信息](case-studies/fujitsu.md)。

![Fujitsu ServerView 扩展](../media/extensibility-overview/fujitsu-serverview-extension.png)

### Lenovo

Lenovo 的 XClarity 系统集成商扩展将硬件管理带到下一级别，通过无缝集成到 Windows Admin Center 内各种应用体验。 XClarity 系统集成商解决方案可提供的所有 Lenovo 服务器，一个高级视图和不同的工具扩展提供硬件的详细信息，无论连接到一台服务器、 故障转移群集或超融合群集。 [了解有关 Lenovo XClarity 系统集成商扩展详细信息](case-studies/lenovo.md)。

![Lenovo 扩展](../media/extensibility-overview/lenovo-extension.png)

### 纯存储

纯存储提供了企业版提供以数据为中心的体系结构，可加速的竞争优势业务发展的全闪存数据存储解决方案。 Windows Admin Center 的纯存储扩展提供到纯 FlashArray 产品的单窗格视图，使用户能够执行监视任务、 查看实时性能指标和管理存储卷和启动器通过单个用户界面体验。 [了解有关纯的扩展和及其开发体验](case-studies/purestorage.md)。

![纯存储扩展](../media/extensibility-overview/purestorage-extension.png)

### Squared Up

Squared Up 基于 System Center Operations Manager 提供一流的监视体验，并与 Azure Log Analytics、Application Insights 和其他监视解决方案集成。 [Squared Up 扩展](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu)向 Windows Admin Center 提供的服务器和群集管理的环境提供历史性能数据和实时应用程序拓扑，早期客户已认可了将很多独立源的大量数据带入单一体验的价值。 [了解有关 Squared Up 的扩展及其开发体验的详细信息](case-studies/squared-up.md)。

![Squared Up 扩展](../media/extensibility-overview/squaredup-extension.png)