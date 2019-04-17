---
title: 启用虚拟网络适配器上 vRSS
description: 在本主题中，你将了解如何通过使用设备管理器或 Windows PowerShell 启用 Windows Server 中的 vRSS。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cb48315c-0204-4927-aa24-64f6789c2e20
manager: dougkim
ms.localizationpriority: medium
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 19e8011fb98b84c20e8237792664551d2362d589
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133413"
---
# 启用虚拟网络适配器上 vRSS

>适用于：Windows Server（半年频道）、Windows Server 2016

虚拟 RSS \(vRSS\) 要求从物理适配器的虚拟机队列 \(VMQ\) 支持。 如果 VMQ 处于禁用状态或不支持则虚拟接收方缩放会禁用。 

有关详细信息，请参阅[计划 vRSS 的使用](vrss-plan.md)。

## 启用 vRSS VM 上
 
使用以下过程使用 Windows PowerShell 或设备管理器启用 vRSS。

-   设备管理器
-   Windows PowerShell
  
### 设备管理器

你可以使用此过程使用设备管理器启用 vRSS。

>[!NOTE]
>此过程的第一步是特定于运行 Windows 10 或 Windows Server 2016 的 Vm。 如果你的虚拟机运行其他操作系统，你可以通过首次打开控制面板，然后找到并打开设备管理器中打开设备管理器。
  
1.  在虚拟机任务栏上，在**这里要搜索类型**中，键入**设备**。 

2.  在搜索结果中，单击**设备管理器**。

3.  在设备管理器中，单击以展开**网络适配器**。 

4.  右键单击你想要配置的网络适配器，然后单击**属性**。<p>打开网络适配器的**属性**对话框。

5.  在网络适配器**属性**，依次单击**高级**选项卡。 

6.  在**属性**中，向下滚动到，然后单击**接收方缩放**。 

7.  确保**值**中的选择**已启用**。 

8.  单击**确定**。
  
> [!NOTE]
> 在**高级**选项卡上，某些网络适配器还会显示适配器支持的 RSS 队列数。

---

### Windows PowerShell

使用以下过程使用 Windows PowerShell 启用 vRSS。

1. 在虚拟机，打开**Windows PowerShell**。

2. 键入以下命令，确保你替换的*适配器名称*值 **-名称**参数与你想要配置，然后按 enter 键的网络适配器的名称。 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >或者，你可以使用以下命令来启用 vRSS。
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

有关详细信息，请参阅[RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

---