---
title: Windows Server 2019 和 Microsoft 服务器应用程序兼容性
description: Windows Server 2019 和 Microsoft 服务器应用程序的兼容性表
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2afe7c32-1fda-4441-989b-4115dabdcd34
author: coreyp
ms.author: coreyp-at-msft
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 7b62f0951ab8d7ed54448eb86f457f9a689da9cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858508"
---
# <a name="windows-server-2019-and-microsoft-server-application-compatibility"></a>Windows Server 2019 和 Microsoft 服务器应用程序兼容性

>适用于：Windows Server 2019

此表列出了支持窗口服务器 2019年安装和功能的 Microsoft 服务器应用程序。 此信息用于快速参考，不用于替代有关单个产品的规格、要求、公告或每个服务器应用程序的常规通信的说明。 请参考每种产品的正式文档以充分了解兼容性和选项。

如果你是软件供应商合作伙伴查找 Windows Server 与非 Microsoft 应用程序兼容性的详细信息，请访问[商业应用程序认证门户](https://commercialappcertification.microsoft.com/)。
| **Product**                                                  | **支持在 Server Core 上**             |   | **在带桌面体验的服务器上受支持** | **发布了吗？** |   | **产品 Web 链接**                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------------------------------|------------------------------------------|---|-------------------------------------------------|---------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exchange Server 2019                                         | 是                                      |   | 是                                             | 是            |   | [Exchange Server 系统要求](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019)                                                                        |
| 在 host Integration Server 2016 CU3                            | 是                                      |   | 是                                             | 否            |   | [主机集成服务器系统要求](https://docs.microsoft.com/host-integration-server/install-and-config-guides/system-requirements)                                                            |
| Visual Studio Team Foundation Server 2017                    | 是的\*                                    |   | 是                                             | 是           |   | [Team Foundation Server 2017](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                |
| Visual Studio Team Foundation Server 2018                    | 是的\*                                    |   | 是                                             | 是           |   | [Team Foundation Server 2018](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                  |
| Microsoft SQL Server 2014                                    | 是的\*                                    |   | 是                                             | 是           |   | [硬件和软件要求安装 SQL Server 2014 的](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2014)   |
| Microsoft SQL Server 2016                                    | 是的\*                                    |   | 是                                             | 是           |   | [硬件和软件要求安装 SQL Server 2016 的](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2016)   |
| Microsoft SQL Server 2017                                    | 是的\*                                    |   | 是                                             | 是           |   | [硬件和软件要求安装 SQL Server 2017 的](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017) |
| Microsoft System Center Configuration Manager （版本 1806年） | 是作为托管客户端，不作为站点服务器 |   | 是作为托管客户端，不作为站点服务器        | 是           |   | [什么是版本 1806年的 System Center Configuration Manager 中的新增功能](https://docs.microsoft.com/sccm/core/plan-design/changes/whats-new-in-version-1806)                                                    |
| Microsoft System Center Operations Manager 2019              | 是的\*                                    |   | 是                                             | 否            |   | [System Center Operations Manager 的系统要求](https://docs.microsoft.com/system-center/scom/plan-system-requirements)                                                                                                      |
| Microsoft System Center Virtual Machine Manager 2019         | 是的\*                                    |   | 是                                             | 否            |   | [为 System Center Virtual Machine Manager 的系统要求](https://docs.microsoft.com/system-center/vmm/system-requirements)                                                                                                      |
| Microsoft System Center Data Protection Manager 2019         | 否                                       |   | 是                                             | 否            |   | [System Center 版本选项概述](https://docs.microsoft.com/system-center/ltsc-and-sac-overview)                                                                                                      |
| SharePoint Server 2016                                       | 否                                       |   | 是                                             | 是           |   | [SharePoint Server 2016 的硬件和软件要求](https://docs.microsoft.com/SharePoint/install/hardware-and-software-requirements)                                                                |
| SharePoint Server 2019                                       | 否                                       |   | 是                                             | 是            |   | [SharePoint Server 2019 的硬件和软件要求](https://docs.microsoft.com/sharepoint/install/hardware-and-software-requirements-2019)                                                       |
| Project Server 2016                                          | 否                                       |   | 是                                             | 是           |   | [Project Server 2016 的软件要求](https://docs.microsoft.com/project/software-requirements-for-project-server-2016)                                                                                |
| Project Server 2019                                          | 否                                       |   | 是                                             | 是           |   | [项目服务器 2019年的软件要求](https://docs.microsoft.com/project/software-requirements-for-project-server-2019)                                                                          |
| 有关业务 2019 Skype                                      | 否                                       |   | 是                                             | 是           |   | [安装先决条件 Skype for Business Server](https://docs.microsoft.com/skypeforbusiness/deploy/install/install-prerequisites)                                                                          |

\*可能有限制或可能需要[服务器核心应用程序兼容性功能上需 (FOD)。 ](install-fod-19.md)
请参阅特定产品或 FOD 文档。
