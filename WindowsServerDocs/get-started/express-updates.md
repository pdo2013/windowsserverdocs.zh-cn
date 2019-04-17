---
title: Express 更新重新启用 2018 年 11 月针对 Windows Server 2016 更新
description: 提供有关 Windows Server 2016 中 Express 更新的信息
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: c48979440ab7c5cfa86aa1287b354a1e43692f48
ms.sourcegitcommit: 23e0a68e21985d709e029e7771d3c52d6815bcb4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6507846"
---
# Express 更新重新启用 2018 年 11 月针对 Windows Server 2016 更新

>通过 Joel Frauenheim

>适用于：Windows Server 2016

开始使用 2018 年 11 月 13 日更新星期二，Windows 将再次发布 Express 更新适用于 Windows Server 2016。 Express 更新适用于 Windows Server 2016 停止中等 2017 年后发现防止正确安装更新的重大问题。 2017 年 11 月修复了此问题，而更新团队保守的方法所用的时间发布 Express 包，以确保已安装在其服务器环境的 2017 年 11 月 14 日更新 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) 并不是大多数客户受影响的问题。

对于 WSUS 和 System Center Configuration Manager (SCCM) 的系统管理员需要知道在 2018 年 11 月他们再次将看到两个 Windows Server 2016 更新软件包： 完全更新和 Express 更新。 想要为其服务器环境中使用 Express 的系统管理员需要确认设备已完全更新自 2017 年 11 月 14，([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) 以确保正确安装 Express 更新。 由于在 2017 年 11 月 14 日更新 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) 将看到尚未更新的任何设备重复使用带宽和 CPU 资源无限循环中的，如果尝试 Express 更新失败。  修正该状态是系统管理员停止 Express 更新推送和推送最近的完全更新以停止故障循环。

与 2018 年 11 月 13 日 Express 更新的客户将看到其管理系统和 Windows Server 2016 终结点之间的包大小即时减少。  