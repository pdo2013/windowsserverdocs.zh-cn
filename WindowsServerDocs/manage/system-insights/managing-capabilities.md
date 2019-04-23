---
title: 管理功能
description: 系统见解会公开各种可以为每个功能，配置的设置，并可以以解决特定于部署的需求优化这些设置。 本主题介绍如何管理通过 Windows Admin Center 或 PowerShell，提供基本的 PowerShell 示例和 Windows Admin Center 屏幕截图演示如何调整这些设置的每个功能的各种设置。
ms.custom: na
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 6/05/2018
ms.openlocfilehash: 9081a0b576ab9871b47df38255047b6cbe889419
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868018"
---
# <a name="managing-capabilities"></a>管理功能

>适用于：Windows Server 2019

在 Windows Server 2019 系统见解会公开各种可以为每个功能，配置的设置，这些设置可以进行调整以解决特定于部署的需求。 本主题介绍如何管理通过 Windows Admin Center 或 PowerShell，提供基本的 PowerShell 示例和 Windows Admin Center 屏幕截图演示如何调整这些设置的每个功能的各种设置。 

>[!TIP]
>这些视频短片还可用于帮助你入门和更有信心地管理系统见解：[在 10 分钟内开始使用系统见解](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

尽管本部分提供了 PowerShell 示例，但可以使用[系统 Insights PowerShell 文档](https://aka.ms/systeminsightspowershell)若要查看所有系统 Insights 中的 cmdlet、 参数和参数集。 

## <a name="viewing-capabilities"></a>查看功能

若要开始，可以列出可用的功能使用的所有**Get InsightsCapability** cmdlet: 

```PowerShell
Get-InsightsCapability
``` 
这些功能也是在系统 Insights 扩展中可见：

![列出可用的功能的系统 Insights 的概述页](media/overview-page-contoso.png)

## <a name="enabling-and-disabling-a-capability"></a>启用和禁用功能
可以启用或禁用每个功能。 禁用一项功能可防止该功能被调用，并为非默认功能禁用一项功能停止该功能的所有数据收集。 默认情况下已启用所有功能，并且可以检查功能使用的状态**Get InsightsCapability** cmdlet。 

若要启用或禁用一项功能，请使用**启用 InsightsCapability**并**禁用 InsightsCapability** cmdlet:

```PowerShell
Enable-InsightsCapability -Name "CPU capacity forecasting"
Disable-InsightsCapability -Name "Networking capacity forecasting"
``` 
这些设置还可以通过选择一项功能在 Windows Admin Center 单击**启用**或**禁用**按钮。

### <a name="invoking-a-capability"></a>调用一项功能
立即调用一项功能运行可以检索预测，并且管理员可以调用一种功能随时通过单击**Invoke**按钮在 Windows Admin Center 或通过使用**调用 InsightsCapability** cmdlet:

```PowerShell
Invoke-InsightsCapability -Name "CPU capacity forecasting"
```

>[!TIP]
>若要确保调用一种功能与在计算机上的关键操作不会发生冲突，请考虑在关闭营业时间内计划的预测。

## <a name="retrieving-capability-results"></a>检索功能结果
最新结果调用一项功能后, 是可见的使用**Get InsightsCapability**或**Get InsightsCapabilityResult**。 这些 cmdlet 输出的最新**状态**并**状态说明**的每项功能，描述每个预测的结果。 **状态**并**状态说明**字段将进一步描述中[了解功能文档](understanding-capabilities.md)。 

此外，还可以使用**Get InsightsCapabilityResult** cmdlet 查看上一次 30 预测结果以及如何检索与预测关联的数据： 

```PowerShell
# Specify the History parameter to see the last 30 prediction results.
Get-InsightsCapabilityResult -Name "CPU capacity forecasting" -History

# Use the Output field to locate and then show the results of "CPU capacity forecasting."
# Specify the encoding as UTF8, so that Get-Content correctly parses non-English characters.
$Output = Get-Content (Get-InsightsCapabilityResult -Name "CPU capacity forecasting").Output -Encoding UTF8 | ConvertFrom-Json
$Output.ForecastingResults
```
系统 Insights 扩展会自动显示预测历史记录和分析 JSON 结果，为您提供每个预测的直观的、 高保真图形的结果：

![单一功能页，其中显示的预测关系图和预测历史记录](media/cpu-forecast-2.png)

### <a name="using-the-event-log-to-retrieve-capability-results"></a>使用事件日志检索功能结果
系统 Insights 记录的事件的一项功能完成预测每次。 这些事件是在可见**Microsoft-Windows-系统-Insights/Admin**通道和系统 Insights 发布每个状态的不同的事件 ID:   

| 预测状态 | 事件 ID |
| --------------- | --------------- |
| 确定 | 151 |
| 警告 | 148 |
| 严重 | 150 |
| 错误 | 149 |
| 无 | 132 |

>[!TIP]
>使用[Azure Monitor](https://azure.microsoft.com/services/monitor/)或[System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807)聚合这些事件和跨计算机组中看到的预测结果。


## <a name="setting-a-capability-schedule"></a>设置功能计划
除了按需预测，还可以配置定期预测每项功能，以便指定的功能将自动调用预定义的计划。 使用**Get InsightsCapabilitySchedule** cmdlet 查看功能计划： 

>[!TIP]
>在 PowerShell 中使用管道运算符以查看返回的所有功能的信息**Get InsightsCapability** cmdlet。

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilitySchedule
```

尽管可以随时使用禁用它们但默认情况下启用定期的预测**启用 InsightsCapabilitySchedule**并**禁用 InsightsCapabilitySchedule** cmdlet:

```PowerShell
Enable-InsightsCapabilitySchedule -Name "Total storage consumption forecasting"
Disable-InsightsCapabilitySchedule -Name "Volume consumption forecasting"
```

每个默认功能计划每天运行一次在凌晨 3 点。 但是，可以创建自定义计划为每个功能，并且系统 Insights 支持不同的计划类型，可以使用配置**集 InsightsCapabilitySchedule** cmdlet: 

```PowerShell
Set-InsightsCapabilitySchedule -Name "CPU capacity forecasting" -Daily -DaysInterval 2 -At 4:00PM
Set-InsightsCapabilitySchedule -Name "Networking capacity forecasting" -Daily -DaysOfWeek Saturday, Sunday -At 2:30AM
Set-InsightsCapabilitySchedule -Name "Total storage consumption forecasting" -Hourly -HoursInterval 2 -DaysOfWeek Monday, Wednesday, Friday
Set-InsightsCapabilitySchedule -Name "Volume consumption forecasting" -Minute -MinutesInterval 30 
```
>[!NOTE]
>因为默认功能分析每日数据，建议使用这些功能的每日计划。 要详细了解默认功能[此处](understanding-capabilities.md)。

此外可以使用 Windows Admin Center 可以查看和设置每个功能的计划，通过单击**设置**。 上显示的当前计划**计划**选项卡上，并且您可以使用 GUI 工具来创建新计划：

![设置页上显示当前计划](media/schedule-page-contoso.png)

## <a name="creating-remediation-actions"></a>创建修正操作
系统 Insights 可以启动自定义修正脚本基于结果的一项功能。 为每个功能，你可以配置自定义 PowerShell 脚本，用于每个预测状态，从而使管理员可以自动采取纠正措施，而不是需要手动干预。 

示例更正操作包括运行磁盘清理，扩展卷，实时迁移的 Vm，并设置 Azure 文件同步运行重复数据删除。

您可以看到每个功能使用的操作**Get InsightsCapabilityAction** cmdlet:

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilityAction
```

可以创建新的操作，也可以删除使用的现有操作**集 InsightsCapabilityAction**并**删除 InsightsCapabilityAction** cmdlet。 使用中指定的凭据运行每个操作**ActionCredential**参数。

>[!NOTE]
>在初始系统 Insights 版本中，必须指定用户的目录外的修正脚本。 这将在即将发布的版本中修复。

```PowerShell
$Cred = Get-Credential
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning -Action "C:\Users\Public\WarningScript.ps1" -ActionCredential $Cred
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Critical -Action "C:\Users\Public\CriticalScript.ps1" -ActionCredential $Cred

Remove-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning
```

此外可以使用 Windows Admin Center 使用设置的补救措施**操作**选项卡内**设置**页：

![用户可以在其中指定的更正操作的设置页](media/actions-page-contoso.png)


## <a name="see-also"></a>请参阅
若要了解有关系统 Insights 的详细信息，请参阅以下资源：

- [系统 Insights 概述](overview.md)
- [了解功能](understanding-capabilities.md)
- [添加和开发功能](adding-and-developing-capabilities.md)
- [系统 Insights 常见问题](faq.md)