---
title: 2018 年 11 月更新重新启用了适用于 Windows Server 2016 的快速更新
description: 提供有关 Windows Server 2016 快速更新的信息
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: 1644a61c87953e465895e23c3c8454bae7f3a056
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66443382"
---
# <a name="express-updates-for-windows-server-2016-re-enabled-for-november-2018-update"></a>2018 年 11 月更新重新启用了适用于 Windows Server 2016 的快速更新

> 作者：Joel Frauenheim
> 
> 适用于：Windows Server 2016

从 2018 年 11 月 13 日的星期二更新开始，Windows 将再次发布适用于 Windows Server 2016 的快速更新。 在发现导致更新无法正确安装的重大问题后，适用于 Windows Server 2016 的快速更新已于 2017 年年中停止。 虽然该问题已于 2017 年 11 月修复，但更新团队采用了保守方法发布快速包，以确保大部分客户在其服务器环境中安装 2017 年 11 月 14 日更新 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953))，且不受到该问题的影响。

WSUS 和 System Center Configuration Manager (SCCM) 的系统管理员需要注意，在 2018 年 11 月，他们将再次看到两个适用于 Windows Server 2016 的更新包：完整更新和快速更新。 想要在其服务器环境中使用快速更新的系统管理员需要确认设备自 2017 年 11 月 14 日 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) 以来已进行过完整更新，从而确保正确安装快速更新。 自 2017 年 11 月 14 日更新 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) 以来未进行过更新的任何设备在尝试进行快速更新时，都将看到以无限循占用带宽和 CPU 资源的重复失败。  系统管理员可通过停止推送快速更新并推送最新的完整更新来停止失败循环，从而修正该状态。

通过 2018 年 11 月 13 日快速更新，客户将看到其管理系统和 Windows Server 2016 终结点之间的包大小立即减小。  