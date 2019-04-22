---
title: 配置策略以限制在网络上的复制流量
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2c1f1865fa1d611c0b5baaf981140f9807b51458
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818688"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>配置策略以限制在网络上的复制流量

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
*可能不会有限制的网络带宽量复制则允许使用。*  
  
## <a name="impact"></a>影响  
*网络带宽可能会变得完全取决于复制流量，会影响其他关键网络活动。这会影响以下端口：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*如果你使用另一种方法来限制网络流量，则可以忽略此。否则，使用组策略编辑器配置策略会限制到副本服务器的相关端口的网络流量。*  
  
  


