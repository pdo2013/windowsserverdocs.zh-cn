---
title: 在使用中的逻辑处理器数不能超过受支持的最大值
description: 提供说明来解决此最佳实践分析程序规则所报告的问题。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 66df8b02-91d1-424b-8934-a39c214d530e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: d1275a17cc04494708f5ecfe9b708834b4233641
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847068"
---
# <a name="the-number-of-logical-processors-in-use-must-not-exceed-the-supported-maximum"></a>在使用中的逻辑处理器数不能超过受支持的最大值

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 最佳实践分析程序。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|错误|  
|**类别**|策略|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的文本。  
  
## <a name="issue"></a>问题  
  
*在服务器配置了太多的逻辑处理器。*  
  
## <a name="impact"></a>影响  
  
*Microsoft 不支持此计算机上运行的 HYPER-V。*  
  
## <a name="resolution"></a>分辨率  
  
*从此计算机中删除某些处理器或使用 msconfig 限制可用处理器的数量。*  
  
请参阅下面的说明来使用 Msconfig。 有关删除处理器详细信息，请参阅计算机随附的说明，或与硬件制造商联系。 有关 HYPER-V 的最大支持的配置详细信息，请参阅[规划 Windows Server 2016 中的 HYPER-V 可伸缩性](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)。  
  
### <a name="to-limit-the-number-of-available-processors"></a>若要限制可用处理器的数量  
  
1.  打开系统配置应用程序 (Msconfig.exe)。 若要执行此操作，请单击**启动**，类型**msconfig**，右键单击**系统配置**桌面应用程序并单击**以管理员身份运行**。  
  
2.  从**引导**选项卡上，单击**高级选项**。  
  
3.  选择**的处理器数**，然后在列表中选择一个数字。 单击 **“确定”**。  
  
4.  重新启动计算机以使用新的处理器数运行它。  
  


