---
title: 使用 Windows Admin Center 来管理与 Azure 更新管理的操作系统更新
description: 使用 Windows Admin Center (项目 Honolulu) 若要设置 Azure 更新管理来管理 OS 更新。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 79b18e9963fba0993a7f34b1409edba6abfd48f0
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452539"
---
# <a name="use-windows-admin-center-to-manage-operating-system-updates-with-azure-update-management"></a>使用 Windows Admin Center 来管理与 Azure 更新管理的操作系统更新

[了解有关 Windows Admin Center 与 Azure 集成的详细信息。](../plan/azure-integration-options.md)

Azure 更新管理是一个解决方案，可从单个位置，而不是在每台服务器上管理更新和修补程序的多台计算机的 Azure 自动化中。 借助 Azure 更新管理，可以快速评估可用更新的状态、 计划所需更新的安装和查看部署结果，验证已成功应用的更新。 这可能是您计算机是否被其他云提供程序，或本地托管的 Azure Vm。 [了解有关 Azure 更新管理的详细信息。](https://docs.microsoft.com/azure/automation/automation-update-management)

与 Windows Admin Center，可以轻松地设置，并使用 Azure 更新管理来保持最新的管理的服务器。 如果还没有 Azure 订阅中的 Log Analytics 工作区，Windows Admin Center 将自动配置你的服务器和订阅和你指定的位置中创建所需的 Azure 资源。 如果有现有的 Log Analytics 工作区，Windows Admin Center 可自动配置你的服务器使用来自 Azure 更新管理的更新。  

若要开始，转到服务器连接中的更新工具和选择"立即设置"，并提供相关的 Azure 资源的首选项。 

配置你的服务器由 Azure 更新管理后，你可以使用更新工具中提供的超链接来访问 Azure 更新管理。 

[了解如何停止使用 Azure 更新管理来更新你的服务器。](azure-monitor.md#disabling-monitoring)

请注意，必须[Windows Admin Center 网关注册到 Azure](../configure/azure-integration.md)之前设置了 Azure 更新管理。

