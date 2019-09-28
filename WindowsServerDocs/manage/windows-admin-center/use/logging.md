---
title: 事件日志记录
description: Windows 管理中心的事件日志记录（Project Honolulu）
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 012c2229fb29aa711d9887f28859e09bcf71c14a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356867"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>使用 Windows 管理中心中的事件日志记录了解管理活动和跟踪网关使用情况

>适用于：Windows Admin Center、Windows Admin Center 预览版

Windows 管理中心会写入事件日志，以使你可以查看在你的环境中的服务器上执行的管理活动，并帮助你解决任何 Windows 管理中心问题。

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>通过用户操作日志记录深入了解环境中的管理活动

Windows 管理中心通过使用 EventID 4000 将操作记录到托管服务器的事件日志中的**ServerManagementExperience**事件通道，来深入了解环境中的服务器上执行的管理活动。和源 SMEGateway。 Windows 管理中心只记录托管服务器上的操作，因此，如果用户出于只读目的访问服务器，则不会看到记录的事件。

记录的事件包含以下信息：

| Key           | ReplTest1                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | 如果操作运行了 PowerShell 脚本，则在服务器上运行的 PowerShell 脚本名称 |
| CIM           | 如果操作运行了 CIM 调用，则是在服务器上运行的 CIM 调用                        |
| 模块        | 运行操作的工具（或模块）                                                     |
| 网关       | 运行操作的 Windows 管理中心网关计算机的名称                     |
| UserOnGateway | 用于访问 Windows 管理中心网关并执行操作的用户名                    |
| UserOnTarget  | 用于访问目标托管服务器的用户名（如果不同于 userOnGateway）（即，使用 "管理方式" 凭据访问服务器的用户） |
| 委派    | 布尔值：如果目标托管服务器信任网关，并从用户的客户端计算机委派凭据             |
| LAPS          | 布尔值：如果用户使用[LAPS](https://technet.microsoft.com/mt227395.aspx)凭据访问服务器                          |
| 文件          | 已上传文件的名称（如果操作是文件上传）                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>了解包含事件日志记录的 Windows 管理中心活动

Windows 管理中心中心会将网关活动记录到网关计算机上的事件通道，以帮助你解决问题并查看有关使用情况的指标。 这些事件记录到**ServerManagementExperience**事件通道。

[了解有关 Windows 管理中心故障排除的详细信息。](troubleshooting.md)
