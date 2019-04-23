---
title: 添加、删除和更新功能
description: 系统见解，可创建新的功能，利用现有的数据收集和管理功能。 非常重要，您还可以添加、 删除和更新这些功能的管理的平台支持。 本主题介绍用于添加、 删除和更新系统 Insights 中的功能的高级功能。
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
ms.openlocfilehash: 07fb036d1c4aa4a63107594ec1f81cb5be1c7724
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880158"
---
# <a name="adding-removing-and-updating-capabilities"></a>添加、删除和更新功能

>适用于：Windows Server 2019

系统见解，可创建新的功能，利用现有的数据收集和管理功能。 但是，一旦创建了这些功能，是同样重要的是，您还可以添加、 删除和更新这些功能的管理的平台支持。 

本主题介绍用于添加、 删除和更新系统 Insights 中的功能的高级功能。 

## <a name="adding-a-capability"></a>添加一项功能
系统 Insights 允许您将随时使用的新功能添加**添加 InsightsCapability** cmdlet。 **添加 InsightsCapability**要求指定功能名称和功能库。 功能库包含功能说明、 数据源和预测逻辑。

```PowerShell
Add-InsightsCapability -Name "Sample capability" -Library "C:\SampleCapability.dll"
```

功能已添加到系统 Insights 后，您可以立即调用和管理使用 PowerShell 或 Windows Admin Center 的功能。 

## <a name="updating-a-capability"></a>更新功能
系统 Insights 还使您可以更新功能使用**更新 InsightsCapability** cmdlet。

```PowerShell
Update-InsightsCapability -Name "Sample capability" -Library "C:\SampleCapabilityv2.dll"
```

更新功能，可指定新的功能库，可以更改的功能说明、 数据源和与该功能相关联的预测逻辑。 重要的是，更新功能将保留所有配置和有关该功能，包括自定义计划、 操作和历史预测结果的历史信息。 

## <a name="removing-a-capability"></a>删除一项功能
您还可以在系统 Insights 使用删除功能**删除 InsightsCapability** cmdlet。 

```PowerShell
Remove-InsightsCapability -Name "Sample capability" 
```
>[!NOTE]
>不能删除默认值预测功能。

永久删除一项功能删除的功能和所有关联的信息，包括计划、 任何修正操作和过去的预测结果。 

>[!TIP]
>请考虑禁用一项功能，而不是如果您担心永久删除与功能相关联的所有信息中将其删除。 

## <a name="see-also"></a>请参阅
若要了解有关系统 Insights 的详细信息，请参阅以下资源：

- [系统 Insights 概述](overview.md)
- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [添加和开发功能](adding-and-developing-capabilities.md)
- [系统 Insights 常见问题](faq.md)