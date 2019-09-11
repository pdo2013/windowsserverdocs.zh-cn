---
title: Windows 管理中心 SDK 案例研究-平方上
description: Windows 管理中心 SDK 案例研究-平方上
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 0d4469684ad9cbdadec5c40cb3b5178345b64a6d
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865343"
---
# <a name="squared-up-extension"></a>方形向上扩展

## <a name="bringing-scom-based-monitoring-server-dependency-visibility-and-external-data-insights-into-windows-admin-center"></a>将基于 SCOM 的监视、服务器依赖项可见性和外部数据见解引入 Windows 管理中心

"方形向上" 是与使用 "数据可视化" 这一构想，旨在帮助解决企业 IT 复杂性的挑战。 带方框的独特、轻量级、仅 UI 的软件构建在 Microsoft 功能强大的 System Center Operations Manager 平台之上，并与其他数据源集成-来自 Microsoft 自己的 Azure Log Analytics、Application Insights 和系统将 Service Manager 为第三方产品（如 ServiceNow、Splunk 和更多），以便在本地和跨混合云环境提供大规模企业基础结构和应用程序财产的可见性。

> <cite>"我们在技术预览版中一直在使用 Windows 管理中心，这已成为一个很大的问题，真正有助于我们的工程师轻松访问我们的配置实验室，并打算使其成为我们的主要管理控制台一旦进入完整版本即可。我们喜欢与平方集成的潜力，并将所有数据都放在一个位置。 "</cite>
>
> --David Acevedo，NuStar 能量 L.P. 上的专家

方形 Up 的客户端管理数百个，通常是成千上万个 Windows Server 和它们提供的各种应用程序组合，同时，这两者都是一项关键任务，使 IT 团队能够更快、最新式地利用 web UI，提供所需的见解。 这样一来，该团队就会立即看到与 Windows 管理中心的令人兴奋的一致，这将与下一代 Windows Server 管理相同的值和主体。 具体而言，团队认为，通过方形向上表现的长期性能数据、实时服务器依赖关系见解和应用程序上下文将完全补充提供的精美、实时的数据和服务器管理功能。Windows 管理中心。

![方形向上扩展](../../media/extend-case-study-squared-up/squared-up-1.png)

> <cite>"作为管理大型服务器场地的组织，可以通过方形向上/Windows 管理中心的集成，实现本地化和集中式工具的完美结婚，并从内部将服务器直接引发到维护模式Windows 管理中心非常适合我们 "</cite>
>
> --K Granson，Purdue 大学的虚拟化系统管理员

通过在 Windows 管理中心中心内无缝地显示这些数据的明确设想，你可以将其与 Windows 管理中心 SDK 的早期专用预览版本结合使用，并且可以很简单地进行记录和灵活地进行查找。

使用 Windows 管理中心 SDK，方形 Up 能够构建一个扩展，该扩展可在 Windows 管理中心体验中动态嵌入相关的方形向上视图。 例如，在特定服务器或群集的上下文中，将自动嵌入 "方形 Up" 视图以提供扩展可见性。 视图包括关键性能和容量指标（如 CPU、内存和磁盘）的历史趋势、托管堆栈（云平台或数据中心虚拟化）、应用程序组件（如 SQL 数据库和服务），甚至基于云的 log analytics和 ITSM 数据。

![方形向上扩展](../../media/extend-case-study-squared-up/squared-up-2.png)

方形 Up 和 Windows 管理中心共享了新式 web 体系结构和设计思维，它同时启用了简单的技术集成和无缝用户体验。 通过基于 web 的管理，我们相信不同系统之间的这种集成方法是用于解锁新式的统一管理体验的关键所在。

> <cite>"我们可以看到 Windows 管理中心中心作为新式 Windows Server 管理的前沿，因此非常适合我们与团队密切合作，而且他们在使用这种速度、热情、灵活性以及这种本质上新式开发范例使它们成为一种非常适合我们的方式，作为一家精益、敏捷、速度快的软件开发公司，亲自工作。 "</cite>
>
> --Richard Benwell，Product 建筑师

从这种自然的发展，到目前为止，开发团队能够迅速地在 Windows 管理中心体验中迅速地对原型集成进行显示，并使其成为自己早期采用者的技术预览客户端。 从客户的反应来看，这只是一种入选方。

> <cite>"跨3500服务器的环境中维护未完成的服务的主要难题之一是统一了不同的管理和监视工具的不同面，因此在方形向上和 Windows 管理中心之间进行集成-这带来了就如此多的数据而言，从多个不同的源到单个控制台–非常大。 "</cite>
>
> --圣马丁 Ehrnst，Azure Intility A/S 的技术主管

由于这种类型的热情，客户端已经有了很多丰富的新功能，并且在 Windows 管理中心仍有很多丰富的新功能，因此，在这种集成的未来，最重要的是极其为 IT 操作管理提供一个真正的单一窗格。

平方向上/Windows 管理中心集成当前为 Beta 版;如果需要访问权限，请查看方框的[专用页面](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-wac&utm_medium=public-relations&utm_campaign=honolulu)了解更多详细信息。 如果你的组织使用 Microsoft System Center Operations Manager 但你还没有方框（这对于该扩展来说是必需的），则你还可以从同一位置获得全功能的30天免费试用版。 
