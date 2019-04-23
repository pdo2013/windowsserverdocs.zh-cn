---
title: Windows Admin Center SDK 案例研究-平方
description: Windows Admin Center SDK 案例研究-平方
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ab0a7bdcf2388ffc867763c04e183b7388fd13e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863938"
---
# <a name="squared-up-extension"></a>平方向上扩展

## <a name="bringing-scom-based-monitoring-server-dependency-visibility-and-external-data-insights-into-windows-admin-center"></a>引入了基于 SCOM 的监视、 服务器依赖项可见性和外部数据深入了解 Windows Admin Center

平方向上被成立的使用数据可视化效果以帮助解决企业 IT 复杂性挑战的愿景。 Microsoft 的强大 System Center Operations Manager 平台，以及与其他数据源-从 Microsoft 自己的 Azure Log Analytics、 Application Insights 和系统集成的基础上构建的平方向上的唯一的轻型 UI 仅软件第三方产品，例如 ServiceNow、 Splunk 和更多-以提供可见性，大型企业级基础结构和应用程序财产，同时在本地和跨混合云环境到中心服务管理器。

> <cite>"我们已一直很大程度利用整个其 Technical Preview 的 Windows Admin Center 和很大命中，实际上正在帮助解决我们配置的实验室，轻松访问我们的工程师的挑战和我们想要使我们的主管理一旦它达到目标的完整版本的控制台。是我们热爱与平方向上的集成和功能来呈现我们在一个位置的所有数据的潜力。"</cite>
>
> -David Acevedo 我 / S 专家 NuStar 能源 l.p.

平方的客户端管理数百个通常数千个，Windows 服务器和将 IT 团队的任务是由它们，并同时平方和 Microsoft 提供的项目组合不同的应用程序的最佳的快速、 现代、 web UI，以提供见解所需。 因此，平方向上的团队立即看到了令人兴奋的对齐方式与 Windows Admin Center 到下一代 Windows Server 管理将这些相同的值和主体。 具体而言，团队认为，长期性能数据、 实时服务器依赖项见解和应用程序上下文的平方值会显示将完全互为补充时尚、 实时数据和提供的服务器管理功能Windows Admin Center。

![平方向上扩展](../../media/extend-case-study-squared-up/squared-up-1.png)

> <cite>"为组织管理大规模的服务器空间，平方向上 / Windows Admin Center 集成是我们的本地化和集中式工具和诸如正在能够直接置于维护模式的服务器引发内完美结婚Windows Admin Center 是为我们的出色小 wins"</cite>
>
> --基普 Granson、 Purdue University 虚拟化系统管理员

有了清楚的想要在 Windows Admin Center 内无缝地显示该数据，平方向上从事 Windows Admin Center SDK 的早期专用预览版本，并发现它非常简单、 完善而灵活。

使用 Windows Admin Center SDK，平方向上就能够构建动态嵌入相关平方 Up Windows Admin Center 中的视图体验的扩展。 例如，在特定服务器或群集的上下文中，平方向上视图会自动嵌入到提供扩展的可见性。 视图包括关键性能和容量指标 （例如 CPU、 内存和磁盘) 的历史趋势托管堆栈 （云平台或数据中心虚拟化），例如 SQL 数据库和服务，应用程序组件，甚至基于云的日志分析和 ITSM 数据。

![平方向上扩展](../../media/extend-case-study-squared-up/squared-up-2.png)

平方和 Windows Admin Center 共享最新的 web 体系结构和设计思维，其中启用了简单的技术集成和无缝用户体验。 结合逐渐成为一种规范的基于 web 的管理，我们认为此方法的不同系统之间的集成是解锁现代、 统一管理体验的关键。

> <cite>"，我们看到 Windows Admin Center 最先进的新式 Windows 服务器管理，因此它已为我们处理如此紧密团队和这一事实，他们现在将此类的速度、 热情、 灵活性，并在此类工作从根本上更完美的体验新式开发范例使得它们非常适合的方式，作为精益、 快节奏的敏捷软件开发公司，工作自己。"</cite>
>
> -Richard Benwell，产品架构师，平方

为此自然对齐，平方向上的开发团队能够快速进行平方启动本机 Windows Admin Center 体验中显示的原型集成并获取的手中他们自己先行采纳者，技术预览客户端。 从客户的反应是立即清除情景已入选方。

> <cite>"维护服务的超过 3,500 服务器环境统一我们多样化的主要挑战之一横向的管理和监视工具，因此平方和 Windows Admin Center-带来了之间的集成组合在一起如此多的数据，从众多不同的源，到单个控制台 – 是巨大的我们。"</cite>
>
> -Martin Ehrnst，Azure 在 Intility a/S 的技术负责人

这种热情平方客户端已以及大量的出色的新功能仍以转到 Windows Admin Center 平方向上极其热衷于这种集成和令人惊叹的可能性开创为其客户端的未来及其true 单一窗格-控制台其 IT 操作管理之旅。

平方向上 / Windows Admin Center 集成目前处于 beta 版本;如果想要访问，请查阅[平方的专用的页](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-wac&utm_medium=public-relations&utm_campaign=honolulu)的更多详细信息。 如果你的组织使用 Microsoft System Center Operations Manager，你还没有平方向上 （这是必需的扩展才能运行），然后还可以手上的功能完备，30 天免费试用版位于同一位置的。 
