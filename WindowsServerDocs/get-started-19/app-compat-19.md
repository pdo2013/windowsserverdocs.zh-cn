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
ms.openlocfilehash: e8bb8cda003ca151216e3b86d5884dfd05e73e6d
ms.sourcegitcommit: 5d55c3ebd1ceee7229ea9a9989c69cf93757ed88
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "9256940"
---
# Windows Server 2019 和 Microsoft 服务器应用程序兼容性

>适用于：Windows Server 2019

此表列出了 Microsoft 服务器应用程序支持 Window Server 2019 上安装和功能。 此信息用于快速参考，不用于替代有关单个产品的规格、要求、公告或每个服务器应用程序的常规通信的说明。 请参考每种产品的正式文档以充分了解兼容性和选项。

如果你是软件供应商合作伙伴查找 Windows Server 与非 Microsoft 应用程序兼容性的详细信息，请访问[商业应用认证门户](https://commercialappcertification.microsoft.com/)。
| **产品**                                                  | **在服务器核心上受支持**             |   | **在带桌面体验的服务器上受支持** | **已发布？** |   | **产品 Web 链接**                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------------------------------|------------------------------------------|---|-------------------------------------------------|---------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exchange Server 2019                                         | 是                                      |   | 是                                             | 是           |   | [Exchange Server 系统要求](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019)                                                                        |
| Host Integration Server 2016，CU3                            | 是                                      |   | 是                                             | 否            |   | [主机集成服务器系统要求](https://docs.microsoft.com/host-integration-server/install-and-config-guides/system-requirements)                                                            |
| Visual Studio Team Foundation Server 2017                    | Yes\ *                                    |   | 是                                             | 是           |   | [Team Foundation Server 2017](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                |
| Visual Studio Team Foundation Server 2018                    | Yes\ *                                    |   | 是                                             | 是           |   | [Team Foundation Server 2018](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                  |
| Microsoft SQL Server 2014                                    | Yes\ *                                    |   | 是                                             | 是           |   | [安装 SQL Server 2014 的硬件和软件要求](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2014)   |
| Microsoft SQL Server 2016                                    | Yes\ *                                    |   | 是                                             | 是           |   | [硬件和软件要求安装 SQL Server 2016](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2016)   |
| Microsoft SQL Server 2017                                    | Yes\ *                                    |   | 是                                             | 是           |   | [硬件和软件要求安装 SQL Server 2017](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017) |
| Microsoft System Center Configuration Manager （版本 1806年） | 是作为托管客户端，不会作为站点服务器 |   | 是作为托管客户端，不会作为站点服务器        | 是           |   | [什么是版本 1806 System Center Configuration Manager 中的新增功能](https://docs.microsoft.com/sccm/core/plan-design/changes/whats-new-in-version-1806)                                                    |
| Microsoft System Center Operations Manager 2019              | Yes\ *                                    |   | 是                                             | 是           |   | [System Center Operations Manager 的系统要求](https://docs.microsoft.com/system-center/scom/plan-system-requirements)                                                                                                      |
| Microsoft System Center Virtual Machine Manager 2019         | Yes\ *                                    |   | 是                                             | 是           |   | [系统要求的系统中心虚拟机管理器](https://docs.microsoft.com/system-center/vmm/system-requirements)                                                                                                      |
| Microsoft System Center Data Protection Manager 2019         | 否                                       |   | 是                                             | 是           |   | [System Center Data Protection Manager 准备环境](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-2019)                                                                                                      |
| SharePoint Server 2016                                       | 否                                       |   | 是                                             | 是           |   | [SharePoint Server 2016 的硬件和软件要求](https://docs.microsoft.com/SharePoint/install/hardware-and-software-requirements)                                                                |
| SharePoint Server 2019                                       | 否                                       |   | 是                                             | 是           |   | [SharePoint Server 2019 的硬件和软件要求](https://docs.microsoft.com/sharepoint/install/hardware-and-software-requirements-2019)                                                       |
| Project Server 2016                                          | 否                                       |   | 是                                             | 是           |   | [Project Server 2016 的软件要求](https://docs.microsoft.com/project/software-requirements-for-project-server-2016)                                                                                |
| Project Server 2019                                          | 否                                       |   | 是                                             | 是           |   | [项目 Server 2019 的软件要求](https://docs.microsoft.com/project/software-requirements-for-project-server-2019)                                                                          |
| 针对业务 2019 Skype                                      | 否                                       |   | 是                                             | 是           |   | [安装适用于企业服务器 Skype 的先决条件](https://docs.microsoft.com/skypeforbusiness/deploy/install/install-prerequisites)                                                                          |

\*May 有限制，或者可能需要[服务器核心应用兼容性功能上按需 (FOD)。 ](install-fod-19.md)
请参阅特定产品或 FOD 文档。
