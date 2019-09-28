---
title: Windows Server 2019 和 Microsoft 服务器应用程序兼容性
description: Windows Server 2019 和 Microsoft 服务器应用程序的兼容性表
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 40c90b179321062949af5eea6d92f6238fb9b460
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391996"
---
# <a name="windows-server-2019-and-microsoft-server-application-compatibility"></a>Windows Server 2019 和 Microsoft 服务器应用程序兼容性

>适用于：Windows Server Standard 2012 R2

该表罗列支持 Window Server 2019 上安装和功能的 Microsoft 服务器应用程序。 此信息用于快速参考，不用于替代有关单个产品的规格、要求、公告或每个服务器应用程序的常规通信的说明。 请参考每种产品的正式文档以充分了解兼容性和选项。

如果你是要了解有关 Windows Server 与非 Microsoft 应用程序兼容性的详细信息的软件供应商合作伙伴，请访问[商业应用认证门户](https://commercialappcertification.microsoft.com/)。

| 产品                                                   | 在服务器核心上受支持              |   | 在具有桌面体验的服务器上受支持  | 已发布？  |   | 产品 Web 链接                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|--------------------------------------------------------------|------------------------------------------|---|-------------------------------------------------|---------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exchange Server 2019                                         | 是                                      |   | 是                                             | 是           |   | [Exchange Server 系统要求](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019)                                                                        |
| Host Integration Server 2016，CU3                            | 是                                      |   | 是                                             | 是            |   | [Host Integration Server 系统要求](https://docs.microsoft.com/host-integration-server/install-and-config-guides/system-requirements)                                                            |
| Visual Studio Team Foundation Server 2017                    | 是\*                                    |   | 是                                             | 是           |   | [Team Foundation Server 2017](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                |
| Visual Studio Team Foundation Server 2018                    | 是\*                                    |   | 是                                             | 是           |   | [Team Foundation Server 2018](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                  |
| Microsoft SQL Server 2014                                    | 是\*                                    |   | 是                                             | 是           |   | [安装 SQL Server 2014 的硬件和软件要求](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2014)   |
| Microsoft SQL Server 2016                                    | 是\*                                    |   | 是                                             | 是           |   | [安装 SQL Server 2016 的硬件和软件要求](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2016)   |
| Microsoft SQL Server 2017                                    | 是\*                                    |   | 是                                             | 是           |   | [安装 SQL Server 2017 的硬件和软件要求](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017) |
| Microsoft System Center Configuration Manager（1806 版） | “是”表示作为托管客户端，“否”表示作为站点服务器 |   | “是”表示作为托管客户端，“否”表示作为站点服务器        | 是           |   | [System Center Configuration Manager 1806 版中的新增功能](https://docs.microsoft.com/sccm/core/plan-design/changes/whats-new-in-version-1806)                                                    |
| Microsoft System Center Operations Manager 2019              | 是\*                                    |   | 是                                             | 是           |   | [System Center Operations Manager 的系统要求](https://docs.microsoft.com/system-center/scom/plan-system-requirements)                                                                                                      |
| Microsoft System Center Virtual Machine Manager 2019         | 是\*                                    |   | 是                                             | 是           |   | [System Center Virtual Machine Manager 的系统要求](https://docs.microsoft.com/system-center/vmm/system-requirements)                                                                                                      |
| Microsoft System Center Data Protection Manager 2019         | 否                                       |   | 是                                             | 是           |   | [针对 System Center Data Protection Manager 准备环境](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-2019)                                                                                                      |
| SharePoint Server 2016                                       | 否                                       |   | 是                                             | 是           |   | [SharePoint Server 2016 的硬件和软件要求](https://docs.microsoft.com/SharePoint/install/hardware-and-software-requirements)                                                                |
| SharePoint Server 2019                                       | 否                                       |   | 是                                             | 是           |   | [SharePoint Server 2019 的硬件和软件要求](https://docs.microsoft.com/sharepoint/install/hardware-and-software-requirements-2019)                                                       |
| Project Server 2016                                          | 否                                       |   | 是                                             | 是           |   | [Project Server 2016 的软件要求](https://docs.microsoft.com/project/software-requirements-for-project-server-2016)                                                                                |
| Project Server 2019                                          | 否                                       |   | 是                                             | 是           |   | [Project Server 2019 的软件要求](https://docs.microsoft.com/project/software-requirements-for-project-server-2019)                                                                          |
| Skype for Business 2019                                      | 否                                       |   | 是                                             | 是           |   | [为 Skype for Business Server 安装系统必备](https://docs.microsoft.com/skypeforbusiness/deploy/install/install-prerequisites)                                                                          |

\*可能存在限制或可能需要[服务器核心应用兼容性按需功能 (FOD)](install-fod-19.md)。
请参阅特定产品或 FOD 文档。
