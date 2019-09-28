---
title: 使用 Windows 管理中心使用 Azure 更新管理管理操作系统更新
description: 使用 Windows 管理中心（Project Honolulu）设置 Azure 更新管理以管理操作系统更新。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server
ms.openlocfilehash: 7b38dae62232c61afafd65eabaf239ad7e747d69
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407069"
---
# <a name="use-windows-admin-center-to-manage-operating-system-updates-with-azure-update-management"></a>使用 Windows 管理中心使用 Azure 更新管理管理操作系统更新

[了解有关 Azure 与 Windows 管理中心的集成的详细信息。](../plan/azure-integration-options.md)

Azure 更新管理是 Azure 自动化中的一种解决方案，可用于从单个位置（而不是每个服务器）管理多台计算机的更新和修补程序。 借助 Azure 更新管理，可以快速评估可用更新的状态、计划所需更新的安装，以及查看部署结果来验证更新是否已成功应用。 无论你的计算机是 Azure Vm、由其他云提供商托管还是在本地，都可以这样做。 [详细了解 Azure 更新管理。](https://docs.microsoft.com/azure/automation/automation-update-management)

使用 Windows 管理中心，你可以轻松地设置和使用 Azure 更新管理使托管服务器保持最新。 如果 Azure 订阅中还没有 Log Analytics 工作区，Windows 管理中心会自动配置服务器，并在订阅和指定位置创建必要的 Azure 资源。 如果现有 Log Analytics 工作区，Windows 管理中心可以自动将服务器配置为使用 Azure 更新管理中的更新。  

若要开始，请转到服务器连接中的更新工具，然后选择 "立即设置"，并提供相关 Azure 资源的首选项。 

将服务器配置为由 Azure 更新管理管理后，可以使用 "更新" 工具中提供的超链接访问 Azure 更新管理。 

[了解如何停止使用 Azure 更新管理更新服务器。](azure-monitor.md#disabling-monitoring)

请注意，在设置 Azure 更新管理之前，必须先将[Windows 管理中心网关注册到 azure](../configure/azure-integration.md) 。

