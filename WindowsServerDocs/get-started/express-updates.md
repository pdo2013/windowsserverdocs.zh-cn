---
title: Windows Server 2016 为 2018 年 11 月重新启用快速更新更新
description: 提供有关在 Windows Server 2016 Express 的更新的信息
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: 1644a61c87953e465895e23c3c8454bae7f3a056
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443382"
---
# <a name="express-updates-for-windows-server-2016-re-enabled-for-november-2018-update"></a>Windows Server 2016 为 2018 年 11 月重新启用快速更新更新

> 通过 Joel Frauenheim
> 
> 适用于：Windows Server 2016

正在启动与： 2018 年 11 月 13 日更新 （星期二）、 Windows 将再次发布 Express 的更新 Windows Server 2016。 快速更新的 Windows Server 2016 在 2017 年中旬中停止后找到最重要的问题的正确安装保持更新。 虽然该问题已修复在 2017 年 11 月中，更新团队采用保守的做法所用的时间发布 Express 包，以确保大多数客户将具有 2017 年 11 月 14 日更新 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) 在其服务器上安装环境和不受此问题。

WSUS 和 System Center Configuration Manager (SCCM) 的系统管理员需要了解在 2018 年 11 月它们再一次将看到两个 Windows Server 2016 更新包： 完整的更新和快速更新。 系统管理员想要针对其服务器环境需要确认该设备已经完全更新自 2017 年 11 月 14 日以来使用 Express ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) 以确保正确安装快速更新。 自 2017 年 11 月 14，更新以来尚未更新的任何设备 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) 将看到如果试图执行快速更新时使用的带宽和 CPU 资源在无限循环中的重复的失败。  为该状态修补将为系统管理员联系，以停止推入 Express 更新并推送新的完整更新，停止失败循环。

与： 2018 年 11 月 13 日 Express 更新客户将看到其管理系统和 Windows Server 2016 终结点之间的包大小立即减少。  