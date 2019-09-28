---
title: 使用中的逻辑处理器数不得超过支持的最大值
description: 提供有关如何解决此最佳做法分析器规则报告的问题的说明。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 66df8b02-91d1-424b-8934-a39c214d530e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 380daf333c041c8702228a60c26ab6e76e4cf3e1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393403"
---
# <a name="the-number-of-logical-processors-in-use-must-not-exceed-the-supported-maximum"></a>使用中的逻辑处理器数不得超过支持的最大值

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 [最佳实践分析程序](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|Error|  
|**类别**|策略|  
  
在以下部分中，斜体表示在此问题的最佳做法分析器工具中出现的文本。  
  
## <a name="issue"></a>问题  
  
*为服务器配置的逻辑处理器太多。*  
  
## <a name="impact"></a>影响  
  
*Microsoft 不支持在此计算机上运行 Hyper-v。*  
  
## <a name="resolution"></a>分辨率  
  
*从此计算机中删除一些处理器，或使用 msconfig 限制可用处理器的数量。*  
  
请参阅以下说明以使用 Msconfig。 有关删除处理器的详细信息，请参阅计算机附带的说明或联系硬件制造商。 有关 Hyper-v 支持的最高配置的详细信息，请参阅[在 Windows Server 2016 中规划 hyper-v 可伸缩性](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)。  
  
### <a name="to-limit-the-number-of-available-processors"></a>限制可用处理器的数量  
  
1.  打开系统配置应用（Msconfig.exe）。 为此，请单击 "**开始**"，键入**msconfig**，右键单击 "**系统配置**" 桌面应用程序，然后单击 "以**管理员身份运行**"。  
  
2.  从 "**启动**" 选项卡中，单击 "**高级选项**"。  
  
3.  选择 "**处理器数量**"，然后在列表中选择一个编号。 单击 **“确定”** 。  
  
4.  重新启动计算机以使用新数目的处理器运行它。  
  


