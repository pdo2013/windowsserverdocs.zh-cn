---
title: 添加和开发功能
description: 系统见解，可将新预测功能添加到系统见解，而无需任何 OS 更新。 这使开发人员，包括 Microsoft 和第三方，用于创建和传送新功能中间版本，若要解决你关注的方案。 新功能可以指定自定义数据收集和分析，并且它们还集成与现有系统 Insights 管理平面。
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
ms.date: 7/31/2018
ms.openlocfilehash: 8caddead774ac69a38906f3c0a0d2eaf005c1d28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817478"
---
# <a name="adding-and-developing-new-capabilities"></a>添加和开发新功能

>适用于：Windows Server 2019

系统见解，可将新预测功能添加到系统见解，而无需任何 OS 更新。 这使开发人员，包括 Microsoft 和第三方，用于创建和传送新功能中间版本，若要解决你关注的方案。 

任何新功能可以与集成并扩展现有系统 Insights 基础结构：

- 新功能可以**指定的任何性能计数器或系统事件**，这将收集、 保留在本地，并调用功能时，返回到以进行分析的功能。  
- 新功能可以**利用现有 Windows Admin Center 和 PowerShell 管理平面**。 不仅将新功能可发现系统 Insights 中，它们还受益于自定义计划和更正操作。 

## <a name="manage-new-capabilities"></a>管理新功能
- [了解](add-remove-update-capabilities.md)如何添加、 删除和更新使用 PowerShell 的功能。 

## <a name="develop-a-capability"></a>开发功能，
使用以下资源来帮助你开始编写你自己的自定义功能：
- [了解](data-sources.md)有关你可以收集的数据源。
- [下载](https://www.nuget.org/packages/Microsoft.WindowsServer.SystemInsights/)系统 Insights NuGet 包，其中包含的类和接口需要编写一项功能。
- [请访问](https://aka.ms/systeminsights-api)API 文档，以了解系统 Insights 类和接口。 
- [使用](https://aka.ms/systeminsights-samplecapability)系统 Insights 示例功能，以帮助你入门。 这将演示如何注册功能，指定要收集的数据源并开始分析系统的数据。

>[!NOTE]
>这是预发行功能。 它会发生变化，我们添加的新功能和将反馈意见。

## <a name="see-also"></a>请参阅
若要了解有关系统 Insights 的详细信息，请参阅以下资源：

- [系统 Insights 概述](overview.md)
- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [添加、 删除和更新功能](add-remove-update-capabilities.md)
- [系统 Insights 常见问题](faq.md)