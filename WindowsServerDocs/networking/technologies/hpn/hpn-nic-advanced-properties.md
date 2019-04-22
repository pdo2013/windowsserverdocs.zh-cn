---
title: NIC 高级属性
description: 你可以管理 Nic 和通过 Windows PowerShell 或在网络控件面板的所有功能。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: d1a5fb57bf71fd981e001cfd9ac595ab5bc3cfc5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819898"
---
# <a name="nic-advanced-properties"></a>NIC 高级属性

你可以管理 Nic 和通过 Windows PowerShell 中使用的所有功能[NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps&viewFallbackFrom=winserverr2-ps) cmdlet。  此外可以管理 Nic 和使用网络控制面板 (ncpa.cpl) 的所有功能。 

1. 在中**Windows PowerShell**，请运行`Get‑NetAdapterAdvancedProperties`cmdlet 针对两个不同制造商/型号的 Nic。

   ![Get-NetAdapterAdvancedProperty m1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-m1.png)

   ![Get-NetAdapterAdvancedProperty c1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-c1.png)

   有相似之处和这些两个 NIC 高级属性列出了差异。

2. 在中**网络控制面板**(ncpa.cpl)，执行以下操作：

   a. 右键单击 nic。

   ![网络连接对话框](../../media/network-offload-and-optimization/network-connections-dialog.png)

   b. 在属性对话框中，单击**配置**。

    ![C1 属性](../../media/network-offload-and-optimization/c1-properties.png)

   c. 单击**高级**选项卡以查看高级的属性。<p>此列表中的项将中的项相关联`Get-NetAdapterAdvancedProperties`输出。

   ![Chelsio 网络适配器属性](../../media/network-offload-and-optimization/chelsio-network-adapter-properties.png)

---