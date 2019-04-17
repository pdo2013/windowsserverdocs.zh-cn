---
title: 事件日志记录
description: 从 Windows 管理员中心 (Project Honolulu) 的事件日志记录
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d91b92cb3bba99ae4aa96a96650a251a6df4cea5
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "2074338"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>使用 Windows 管理中心中的日志记录的事件深入管理活动和跟踪网关使用率

>应用于： Windows 管理中心中，Windows Admin Center 预览

Windows Admin Center 写入事件日志，以让您看到您的环境中的服务器上执行的管理活动，以及可帮助您解决任何 Windows Admin Center 问题。

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>深入了解通过用户操作日志记录环境中管理活动

Windows Admin Center 提供深入的日志记录操作到**Microsoft ServerManagementExperience**事件通道事件日志中托管服务器与 EventID 4000 的执行环境中的服务器上的管理活动和源 SMEGateway。 Windows Admin Center 仅日志操作在托管服务器上，因此您将看记录如果用户的只读访问服务器的事件。

记录的事件包括以下信息：

| 注册表项           | 值                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | 如果操作运行 PowerShell 脚本的服务器，运行的 PowerShell 脚本名称 |
| CIM           | 如果操作运行 CIM 呼叫的服务器，运行的 CIM 呼叫                        |
| 模块        | 工具 （或模块） 运行操作                                                     |
| 网关       | 运行该操作的 Windows Admin Center 网关计算机名称                     |
| UserOnGateway | 用于访问 Windows Admin Center 网关并执行操作的用户名                    |
| UserOnTarget  | 用于访问目标托管的服务器，如果不同于 userOnGateway （即用户使用的服务器使用"管理"凭据访问） 的用户名 |
| 委派    | 布尔值： 如果目标托管服务器信任该网关和凭据将委派从用户的客户端计算机             |
| LAPS          | 布尔值： 如果用户访问的服务器使用[LAPS](https://technet.microsoft.com/mt227395.aspx)凭据                          |
| File          | 上载文件的名称，如果该动作是文件上载                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>了解 Windows Admin Center 活动事件日志记录

Windows Admin Center 将网关活动记录到可帮助您解决问题和查看使用情况指标的网关计算机上的事件通道。 这些事件记录到**Microsoft ServerManagementExperience**事件通道。

[了解有关疑难解答 Windows Admin Center。](troubleshooting.md)
