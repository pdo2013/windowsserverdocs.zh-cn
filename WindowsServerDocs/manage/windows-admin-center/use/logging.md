---
title: 事件日志记录
description: 从 Windows Admin Center (项目 Honolulu) 的事件日志记录
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d91b92cb3bba99ae4aa96a96650a251a6df4cea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849298"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>使用 Windows Admin Center 中的日志记录的事件来深入了解管理活动和跟踪网关使用情况

>适用于：Windows Admin Center，Windows Admin Center 预览版

Windows Admin Center 写入事件日志，以便你可以看到你的环境中的服务器上执行管理活动，以及可帮助你解决任何 Windows Admin Center 问题。

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>深入了解通过用户操作日志记录在环境中的管理活动

Windows Admin Center 提供日志记录操作你的环境中的服务器上执行的管理活动的深入**Microsoft ServerManagementExperience**托管的事件日志中的事件通道具有 EventID 4000 和源 SMEGateway 的服务器。 Windows Admin Center 仅记录在托管服务器上的操作，因此不会记录在用户用于只读目的访问服务器的事件。

记录的事件包括以下信息：

| 键           | ReplTest1                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | 如果该操作运行 PowerShell 脚本在服务器运行的 PowerShell 脚本名称 |
| CIM           | 如果该操作运行的 CIM 调用的服务器，运行的 CIM 调用                        |
| 模块        | 工具 （或模块） 运行该操作                                                     |
| 网关       | 操作已运行的 Windows Admin Center 网关计算机的名称                     |
| UserOnGateway | 用来访问 Windows Admin Center 网关和执行的操作的用户名称                    |
| UserOnTarget  | 用来访问目标托管的服务器，如果不同于 userOnGateway （即使用使用"管理方式"凭据的服务器访问的用户） 的用户名称 |
| 委派    | 布尔： 如果目标管理服务器信任网关和从用户的客户端计算机的凭据将委派             |
| LAPS          | 布尔： 如果服务器使用的用户访问[LAPS](https://technet.microsoft.com/mt227395.aspx)凭据                          |
| 文件          | 上传时，如果操作是文件上传的文件的名称                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>了解如何使用事件日志记录的 Windows Admin Center 活动

Windows Admin Center 网关活动记录到以帮助您解决问题并查看使用情况指标的网关计算机上的事件通道。 这些事件记录到**Microsoft ServerManagementExperience**事件通道。

[了解有关故障排除 Windows Admin Center 的详细信息。](troubleshooting.md)
