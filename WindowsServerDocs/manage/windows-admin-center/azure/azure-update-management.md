---
title: 使用 Windows Admin Center 管理使用 Azure 更新管理操作系统更新
description: 使用 Windows Admin Center (Project Honolulu) 来设置 Azure 更新管理来管理操作系统更新。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 5ba81968f8baa81176ad646fb2a97961ddc49fda
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296877"
---
# 使用 Windows Admin Center 管理使用 Azure 更新管理操作系统更新

[了解有关 Azure 与 Windows Admin Center 集成的详细信息。](../plan/azure-integration-options.md)

Azure 更新管理是 Azure 自动化中允许你管理更新和修补程序的多个计算机从一个位置，而不是在每台服务器上的解决方案。 使用 Azure 更新管理，您可以快速评估可用更新的状态、 计划安装所需的更新，并查看部署结果，以验证已成功应用更新。 这是可能计算机是否托管由其他云提供程序，或上部署的 Azure 虚拟机。 [了解有关 Azure 更新管理的详细信息。](https://docs.microsoft.com/azure/automation/automation-update-management)

使用 Windows Admin Center，可以轻松地设置，并使用 Azure 更新管理使你的托管的服务器保持最新。 如果还没有 Azure 订阅中 Log Analytics 工作区，Windows Admin Center 将自动配置你的服务器，并创建所需的 Azure 资源中的订阅和你指定的位置。 如果你有现有的 Log Analytics 工作区，Windows Admin Center 可以自动配置你的服务器使用 Azure 更新管理更新。  

若要开始，转到服务器连接中的更新工具并选择"设置现在"，并提供相关的 Azure 资源的首选项。 

一旦你已配置你要受 Azure 更新管理的服务器，你可以通过使用提供更新工具中的超链接访问 Azure 更新管理。 

[了解如何以停止使用 Azure 更新管理更新你的服务器。](azure-monitor.md#disabling-monitoring)

请注意，你必须[注册 Windows Admin Center 网关使用 Azure](..\configure\azure-integration.md)前设置 Azure 更新管理。

