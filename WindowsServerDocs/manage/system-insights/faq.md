---
title: 系统 Insights 常见问题
description: 系统 Insights 常见问题
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
ms.openlocfilehash: 13767e1336d1ff729d1fbbe6cae3ed57d68cefc4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851058"
---
# <a name="system-insights-faq"></a>系统 Insights 常见问题

>适用于：Windows Server 2019

## <a name="how-can-you-use-system-insights-with-azure-monitor-or-system-center-operations-manager"></a>如何使用 Azure Monitor 或 System Center Operations Manager 使用系统 Insights？

[Azure Monitor](https://azure.microsoft.com/services/monitor/)并[System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807)对你的部署可帮助您管理您的基础结构提供的操作信息。 系统的见解，与此相反，是 Windows Server 功能，引入了本地的预测分析功能。 在一起，系统 Insights 和 Azure Monitor 或 SCOM 可帮助跨设备的总体呈现预测：

 Azure 监视器或 SCOM 可以键关闭创建的系统的见解，事件系统 Insights 输出到事件日志的每个预测的结果。 对一系列 Windows 服务器，您可以在服务器实例组中具有这些预测的统一的视图，它们可能会出现这些特定于计算机的预测。 
 
 请参阅针对每个预测的通道和事件 Id[此处](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results)。

## <a name="how-does-system-insights-relate-to-windows-ml"></a>系统 Insights 关联 Windows 机器学习？

[Windows 机器学习](https://docs.microsoft.com/windows/uwp/machine-learning/)是一个平台，使开发人员能够导入和预先定型的机器学习模型评分 Windows 设备上。 这些模型利用硬件加速，并可以本地评分。 

系统 Insights 是一项功能提供了完整的管理体验，包括 PowerShell 和 Windows Admin Center 集成以及本地预测功能的 Windows Server 2019。 

## <a name="can-i-use-system-insights-for-my-cluster"></a>可以为群集使用系统 Insights？ 

是。 系统 Insights 可以独立运行每个单独的故障转移群集节点，和系统 Insights 预测使用情况的默认行为中，跨本地存储、 卷、 CPU 和网络。 **此外可以将群集存储预测**，因此存储预测功能预测为群集的卷和存储使用情况。 

你可以在 Windows Admin Center 或 PowerShell 中，这些设置和管理提供了有关此功能的更多详细的信息[此处](https://blogs.technet.microsoft.com/filecab/2018/10/03/using-system-insights-to-forecast-clustered-storage-usage/)。
 

## <a name="how-expensive-is-it-to-run-the-default-capabilities"></a>代价是其运行的默认功能？

每个默认功能是成本较低来运行。 每个功能需要较长时间运行收集更多数据，但它们通常应在几秒内完成。 

## <a name="see-also"></a>请参阅
若要了解有关系统 Insights 的详细信息，请参阅以下资源：

- [系统 Insights 概述](overview.md)
- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [添加和开发功能](adding-and-developing-capabilities.md)
