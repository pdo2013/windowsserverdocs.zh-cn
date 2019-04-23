---
title: 启用虚拟网络适配器上 vRSS
description: 在本主题中，您将学习如何使用设备管理器或 Windows PowerShell 启用 vRSS 在 Windows Server 中。
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882678"
---
# <a name="enable-vrss-on-a-virtual-network-adapter"></a>启用虚拟网络适配器上 vRSS

>适用于：Windows 服务器 （半年频道），Windows Server 2016

虚拟 RSS \(vRSS\)需要虚拟机队列\(VMQ\)支持从物理适配器。 如果 VMQ 已禁用或不受支持则虚拟接收方缩放会禁用。 

有关详细信息，请参阅[规划 vRSS 使用](vrss-plan.md)。

## <a name="enable-vrss-on-a-vm"></a>启用 vRSS 在 VM 上
 
使用以下过程使用 Windows PowerShell 或设备管理器启用 vRSS。

-   设备管理器
-   Windows PowerShell
  
### <a name="device-manager"></a>设备管理器

此过程可用于通过使用设备管理器启用 vRSS。

>[!NOTE]
>此过程的第一步是特定于运行 Windows 10 或 Windows Server 2016 的 Vm。 如果 VM 正在运行其他操作系统，你可以通过第一个打开控制面板，然后查找并打开设备管理器中打开设备管理器。
  
1.  在虚拟机任务栏上，在**此处键入内容以搜索**，类型**设备**。 

2.  在搜索结果中，单击**设备管理器**。

3.  在设备管理器中，单击以展开**网络适配器**。 

4.  右键单击你想要配置，然后单击网络适配器**属性**。<p>网络适配器**属性**对话框随即打开。

5.  在网络适配器**属性**，单击**高级**选项卡。 

6.  在中**属性**，向下滚动并单击**接收方缩放**。 

7.  确保中的选定**值**是**已启用**。 

8.  单击 **“确定”**。
  
> [!NOTE]
> 上**高级**选项卡上，某些网络适配器还显示适配器支持的 RSS 队列数目。

---

### <a name="windows-powershell"></a>Windows PowerShell

使用以下过程使用 Windows PowerShell 启用 vRSS。

1. 在虚拟机中，打开**Windows PowerShell**。

2. 键入以下命令，确保您替换*AdapterName*值 **-名称**参数与你想要配置，然后按 enter 键的网络适配器的名称。 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >或者，可以使用以下命令以启用 vRSS。
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

有关详细信息，请参阅[Windows PowerShell 命令适用于 RSS 和 vRSS](vrss-wps.md)。

---