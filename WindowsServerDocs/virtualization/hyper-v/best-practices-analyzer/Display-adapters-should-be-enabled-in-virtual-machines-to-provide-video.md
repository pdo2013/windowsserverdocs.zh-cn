---
title: 显示适配器应启用虚拟机，以提供视频的功能
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: ac5992e6-3c0b-46c2-a48e-6ef37b679228
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8d61461db471a876ddf46c1e5fec6992ffa80373
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870688"
---
# <a name="display-adapters-should-be-enabled-in-virtual-machines-to-provide-video-capabilities"></a>显示适配器应启用虚拟机，以提供视频的功能

>适用于：Windows Server 2016


  
*有关最佳做法和扫描的详细信息，请参阅*[最佳做法分析器](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*Microsoft 虚拟机总线视频设备可能会禁用虚拟机中。*  
  
Microsoft 虚拟机总线视频设备是虚拟的视频适配器的 HYPER-V 虚拟机用于优化。 如果虚拟机未配置为使用 Microsoft 虚拟机总线视频设备，则使用旧的视频适配器。 Microsoft 虚拟机总线视频设备的执行性能优于传统的视频适配器。  
  
## <a name="impact"></a>影响  
  
*以下虚拟机的视频性能将会下降：*  
  
\<虚拟机名称的列表 >  
  
## <a name="resolution"></a>分辨率  
  
*使用设备管理器在来宾操作系统中启用 Microsoft 虚拟机总线视频设备。*  
  
若要使用设备管理器所需的步骤因操作系统而异。 有关说明，请参阅在来宾操作系统中的帮助。  
  


