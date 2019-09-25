---
title: 欢迎使用 Windows Server 2016 中的远程桌面服务
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
ms.openlocfilehash: 1fcb3fb7e2989399d908a1b5bed7bd21240efeab
ms.sourcegitcommit: 2ec5e779c00481b13f186e2c56d207007897cfd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2019
ms.locfileid: "71021644"
---
# <a name="welcome-to-remote-desktop-services"></a>欢迎使用远程桌面服务 

远程桌面服务 (RDS) 是一个卓越的平台，可以生成虚拟化解决方案来满足每个最终客户的需求，包括交付独立的虚拟化应用程序、提供安全的移动和远程桌面访问，使最终用户能够从云运行其应用程序和桌面。

![远程桌面服务概述](./media/rds-overview.png)

RDS 提供部署灵活性、高成本效益和可扩展性 - 各种部署选项都能提供所有这些优势，包括使用 Windows Server 2016 进行本地部署、使用 Microsoft Azure 进行云部署，以及可靠的合作伙伴解决方案阵容。

根据你的环境和偏好，可为基于会话的虚拟化设置 RDS 解决方案、将 RDS 解决方案设置为虚拟桌面基础结构 (VDI)，或者结合这两种设置：

- **基于会话的虚拟化**：利用 Windows Server 的计算能力提供经济高效的多会话环境，以驱动用户的日常工作负荷。
- **VDI**：利用 Windows 客户端为用户提供他们在 Windows 桌面体验中所预期的高性能、应用兼容性和顺手性。

在这些虚拟化环境中，可以更灵活地自定义向用户发布的内容：

- **桌面**：包含你安装和管理的各种应用程序，为用户提供完整的桌面体验。 对于依赖于使用这些计算机作为其主要工作站，或者当前正在使用瘦客户端（例如 MultiPoint 服务）的用户而言，此解决方案非常理想。
- **RemoteApp**：指定在虚拟化计算机上托管/运行的，但看上去如同本地应用程序一样在用户桌面上运行的各个应用程序。 应用有其自身的任务栏条目，并可调整大小以及在监视器之间移动。 非常适合用于在安全的远程环境中部署和管理关键应用程序，同时可让用户在其自己的桌面中工作以及自定义其桌面。

对于成本效益至关重要的环境，以及当你希望在基于会话的虚拟化环境中部署完整桌面后能够扩大收益时，可以使用 [MultiPoint 服务](../multipoint-services/multipoint-services.md)来实现最大价值。 

使用这些选项和配置可以在远端以安全且经济高效的方式灵活部署用户所需的桌面和应用程序。

## <a name="next-steps"></a>后续步骤

以下后续步骤可帮助你更好地了解 RDS，甚至可帮助你开始部署自己的环境：
-   了解各种 Windows 和 Windows Server 版本中的 RDS [支持的配置](rds-supported-config.md)
-   [规划和设计](rds-plan-and-design.md)一个 RDS 环境来满足各种要求，例如高可用性和多重身份验证。
-   查看最适合所需环境的[远程桌面服务体系结构模型](desktop-hosting-logical-architecture.md)。
-   开始[使用 ARM 和 Azure 市场部署 RDS 环境](rds-in-azure.md)。
