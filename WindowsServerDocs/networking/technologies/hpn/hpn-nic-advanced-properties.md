---
title: NIC 高级属性
description: 可以通过 Windows PowerShell 或网络控制面板管理 Nic 和所有功能。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 1395cefca5d9ef696eed3f2735334954b9ee02a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405714"
---
# <a name="nic-advanced-properties"></a>NIC 高级属性

你可以使用[get-netadapter](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps&viewFallbackFrom=winserverr2-ps) Cmdlet 通过 Windows PowerShell 管理 nic 和所有功能。  你还可以使用网络控制面板（ncpa）管理 Nic 和所有功能。 

1. 在**Windows PowerShell**中，对两个不同品牌/型号的 nic 运行 @no__t 的 cmdlet。

   ![NetAdapterAdvancedProperty m1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-m1.png)

   ![NetAdapterAdvancedProperty c1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-c1.png)

   这两个 NIC 高级属性列表中存在相似性和不同之处。

2. 在 "**网络" 控制面板**中，执行以下操作：

   a. 右键单击 NIC。

   !["网络连接" 对话框](../../media/network-offload-and-optimization/network-connections-dialog.png)

   b. 在 "属性" 对话框中，单击 "**配置**"。

    ![C1 属性](../../media/network-offload-and-optimization/c1-properties.png)

   c. 单击 "**高级**" 选项卡以查看高级属性。<p>此列表中的项与 @no__t 的输出中的项相关联。

   ![Chelsio 网络适配器属性](../../media/network-offload-and-optimization/chelsio-network-adapter-properties.png)

---
