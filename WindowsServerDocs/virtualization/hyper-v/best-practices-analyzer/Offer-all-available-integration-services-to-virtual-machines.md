---
title: 提供对虚拟机的所有可用的集成服务
description: 提供说明来解决此最佳实践分析程序规则所报告的问题。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2c4b2043-ad81-495e-aa7a-467f813bb3d2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c2b5137594f78980f87f6520ae4b4af8203aef32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883778"
---
# <a name="offer-all-available-integration-services-to-virtual-machines"></a>提供对虚拟机的所有可用的集成服务

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 最佳实践分析程序。
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*虚拟机上不启用一个或多个可用的集成服务。*  
  
## <a name="impact"></a>影响  
  
*某些功能将不可用到以下虚拟机：*  
  
\<虚拟机名称的列表 >  
  
## <a name="resolution"></a>分辨率  
  
*如果这是有意为之，不不需要任何进一步的操作。否则，请考虑提供的这些虚拟机设置中的所有集成服务。*  
  
可以通过虚拟机设置管理一些集成服务的可用性。   
  
#### <a name="to-manage-the-availability-of-integration-services-to-a-virtual-machine"></a>若要管理集成服务添加到虚拟机的可用性  
  
1.  打开 Hyper-V 管理器。 单击 **“开始”**，指向 **“管理工具”**，然后单击 **“Hyper-V 管理器”**。  
  
2.  在结果窗格中下,**虚拟机**，选择你想要配置的虚拟机。  
  
3.  在 **“操作”** 窗格中的虚拟机名称下，单击 **“设置”**。  
  
4.  下**管理**，单击**Integration Services**。  
  
5.  在 integration services 的列表中，选择你想要为虚拟机提供每个服务该复选框。 如果复选框，则不可用，在虚拟机中运行来宾操作系统不支持特定的集成服务。  
  


