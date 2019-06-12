---
title: Windows Server 2016 中的欢迎使用远程桌面服务
description: 提供远程桌面服务的概述
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 02/22/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 52b9e09f-39e0-41a9-9d3b-4d5f4eacf3e0
author: christianmontoya
manager: scottman
ms.localizationpriority: medium
ms.openlocfilehash: 3d148c99911be0cebfc29429d93241f24c2b9606
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453014"
---
# <a name="welcome-to-remote-desktop-services"></a>欢迎使用远程桌面服务 

远程桌面服务 (RDS) 是用于构建每个最终客户需求，包括提供单个虚拟化应用程序、 提供移动和远程桌面访问的安全性，并提供最终用户的虚拟化解决方案选择的平台若要从在云中运行其应用程序和桌面的功能。

![远程桌面服务概述](./media/rds-overview.png)

RDS 提供了部署灵活性，成本效率和可扩展性-全部通过各种部署选项，包括 Windows Server 2016 的本地部署，Microsoft Azure 的云部署，以及合作伙伴的可靠数组解决方案。

具体取决于您的环境和首选项，您可以设置基于会话的虚拟化的 RDS 解决方案作为虚拟桌面基础结构 (VDI) 或两者的组合：

- **基于会话的虚拟化**:利用 Windows Server 的计算能力，以提供经济高效的多会话环境来驱动用户的日常工作负荷
- **VDI**:利用 Windows 客户端提供的高性能、 应用程序兼容性和用户都希望能其 Windows 桌面体验的熟悉程度。

在这些虚拟化环境中，你有更灵活地向用户发布的内容：

- **桌面**:为用户提供与各种安装和管理应用程序的完整的桌面体验。 适用于依赖于这些计算机作为其主工作站或，即将从瘦客户端，如与 MultiPoint 服务的用户。
- **RemoteApps**:指定的虚拟机上托管/运行单个应用程序，但出现像它们类似于本地应用程序的用户的桌面上运行。 应用有其自己的任务栏条目和可调整大小和移动的监视器。 用于部署和管理，从而允许用户从工作和自定义他们自己的台式机安全的远程环境中的关键应用程序的理想选择。

对于环境是至关重要的成本效益，以及你想要扩展部署在基于会话的虚拟化环境中的完整桌面的优势，可以使用[MultiPoint 服务](../multipoint-services/multipoint-services.md)以提供最佳价值。 

使用这些选项和配置，可以灵活地部署桌面和您的用户需要远程，安全和经济高效的方式中的应用程序。

## <a name="next-steps"></a>后续步骤

下面是一些后续步骤可帮助您更好地了解 RDS 和甚至开始部署您自己的环境：
-   了解[支持的配置](rds-supported-config.md)rds 与各种 Windows 和 Windows Server 版本
-   [规划和设计](rds-plan-and-design.md)RDS 环境，以适应各种要求，如高可用性和多重身份验证。
-   审阅[远程桌面服务体系结构模型](desktop-hosting-logical-architecture.md)适合于您所需的环境。
-   启动到[部署 ARM 和 Azure Marketplace 中的对 RDS 环境](rds-in-azure.md)。
