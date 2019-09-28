---
title: 在虚拟网络适配器上启用 vRSS
description: 本主题介绍如何使用设备管理器或 Windows PowerShell 在 Windows Server 中启用 vRSS。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: cb48315c-0204-4927-aa24-64f6789c2e20
manager: dougkim
ms.localizationpriority: medium
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8f2886f01e4835cf2edb86fcae0a1fe77bc03d25
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405257"
---
# <a name="enable-vrss-on-a-virtual-network-adapter"></a>在虚拟网络适配器上启用 vRSS

>适用于：Windows Server（半年频道）、Windows Server 2016

虚拟 RSS \(vRSS @ no__t-1 需要物理适配器的 \(VMQ @ no__t 支持虚拟机队列。 如果 VMQ 已禁用或不受支持，则会禁用虚拟接收方缩放。 

有关详细信息，请参阅[计划使用 vRSS](vrss-plan.md)。

## <a name="enable-vrss-on-a-vm"></a>在 VM 上启用 vRSS
 
使用以下过程来通过使用 Windows PowerShell 或设备管理器来启用 vRSS。

-   设备管理器
-   Windows PowerShell
  
### <a name="device-manager"></a>设备管理器

可以使用此过程通过设备管理器来启用 vRSS。

>[!NOTE]
>此过程的第一个步骤特定于运行 Windows 10 或 Windows Server 2016 的 Vm。 如果 VM 运行的是不同的操作系统，则可以通过先打开 "控制面板"，然后查找并打开设备管理器来打开设备管理器。
  
1.  在 VM 任务栏上，在 "**要搜索的位置**" 中键入 "**设备**"。 

2.  在搜索结果中，单击 "**设备管理器**"。

3.  在设备管理器中，单击以展开 "**网络适配器**"。 

4.  右键单击要配置的网络适配器，然后单击 "**属性**"。<p>"网络适配器**属性**" 对话框将打开。

5.  在 "网络适配器**属性**" 中，单击 "**高级**" 选项卡。 

6.  在 "**属性**" 中，向下滚动到并单击 "**接收方缩放**"。 

7.  确保**已启用**"**值**" 中的选择。 

8.  单击 **“确定”** 。
  
> [!NOTE]
> 在 "**高级**" 选项卡上，某些网络适配器还显示适配器所支持的 RSS 队列的数量。

---

### <a name="windows-powershell"></a>Windows PowerShell

使用以下过程通过 Windows PowerShell 启用 vRSS。

1. 在虚拟机上，打开**Windows PowerShell**。

2. 键入以下命令，确保将 **-Name**参数的*AdapterName*值替换为要配置的网络适配器的名称，然后按 enter。 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >或者，可以使用以下命令启用 vRSS。
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

有关详细信息，请参阅[RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

---