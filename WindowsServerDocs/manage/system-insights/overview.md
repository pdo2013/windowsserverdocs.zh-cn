---
title: 系统 Insights 概述
description: 系统 Insights 是 Windows Server 2019 中的新预测分析功能。 将系统 Insights 预测功能的每个这些均由机器学习模型的本地分析 Windows Server 系统数据，例如性能计数器和事件，深入了解你的服务器的正常运行并能帮助您减少与被动管理你的部署中的问题相关联的运营费用。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: system-insights
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: ca471e5e747c7aaa369f5725290e16deab7a6660
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887238"
---
# <a name="system-insights-overview"></a>系统 Insights 概述

>适用于：Windows Server 2019

系统 Insights 是 Windows Server 2019 中的新预测分析功能。 将系统 Insights 预测功能的每个这些均由机器学习模型的本地分析 Windows Server 系统数据，例如性能计数器和事件，深入了解你的服务器的正常运行并能帮助您减少与被动管理你的部署中的问题相关联的运营费用。 

在 Windows Server 2019 系统 Insights 附带有四个侧重于容量预测、 预测未来的计算、 网络和存储基于以前的使用模式的资源的默认功能。 系统 Insights 还附带[可扩展的基础结构](adding-and-developing-capabilities.md)，因此，Microsoft 和第三方可以添加新预测功能到系统 Insights 而不更新操作系统。 

您可以通过直观方式管理系统见解[Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview)扩展插件或[直接通过 PowerShell](https://aka.ms/SystemInsightsPowerShell)，和系统 Insights，可以配置每个预测功能单独根据你的部署需求。 所有预测结果都发布到事件日志，以便您可以使用[Azure Monitor](https://azure.microsoft.com/services/monitor/)或[System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807)来轻松地聚合和跨计算机组中看到预测。

![在 Windows Admin Center，显示与关系图绘制预测预测功能的 CPU 容量的系统 Insights 扩展](media/cpu-forecast-2.png)

## <a name="local-functionality"></a>本地功能
系统运行的见解完全本地 Windows Server 上。 使用 Windows Server 2019 中引入的新功能，你所有的数据收集，保持不变，并直接在你的计算机，从而可以实现预测分析功能，无需任何云连接上进行分析。

您的系统数据存储在你的计算机，并由不需要在云中重新训练的预测功能对此数据进行分析。 使用系统的见解，可以将数据保留在计算机上，并仍受益于预测分析功能。 

## <a name="get-started"></a>立即开始行动

<iframe src="https://www.youtube-nocookie.com/embed/AJxQkx5WSaA" width="560" height="315" allowfullscreen></iframe>

>[!TIP]
>观看这些视频短片，了解需要开始并自信地管理系统 Insights 的信息：[在 10 分钟内开始使用系统见解](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

### <a name="requirements"></a>要求
系统 Insights 是可在任何 Windows Server 2019 实例上。 它在主机和来宾计算机，在任何虚拟机监控程序，并在任何云上运行。

### <a name="install-system-insights"></a>安装系统 Insights
>[!IMPORTANT]
>系统 Insights 收集并存储最多一年的数据保存在本地。 如果你想要将操作系统升级时保留数据**请确保使用就地升级**。

#### <a name="install-the-feature"></a>安装功能
可以安装在 Windows Admin Center 内使用的扩展系统见解：

![第 0 天体验系统 Insights 扩展。](media/day-0-2.png)

此外可以通过添加安装系统 Insights 直接通过服务器管理器中**系统 Insights**功能，或使用 PowerShell:

```PowerShell
Add-WindowsFeature System-Insights -IncludeManagementTools
```

## <a name="provide-feedback"></a>提供反馈
我们希望听到你的反馈，帮助我们改进此功能。 可以使用以下渠道来提交反馈：
- **反馈中心**:使用在 Windows 10 反馈中心工具文件 bug 或反馈。 这样做时，请指定：
    - **类别**:Server 
    - **子类别**:系统见解
- **UserVoice**:提交功能请求通过我们[UserVoice 页面](https://windowsserver.uservoice.com/forums/295071-management-tools)。 与同事共享对您重要的投票赞成项。
- **Email**：如果你想要提交您私人与功能团队的反馈，发送电子邮件至system-insights-feed@microsoft.com。 请记住，我们仍并可能会请求你的反馈中心或 UserVoice。

## <a name="see-also"></a>请参阅
若要了解有关系统 Insights 的详细信息，请参阅以下资源：

- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [添加和开发功能](adding-and-developing-capabilities.md)
- [系统 Insights 常见问题](faq.md)