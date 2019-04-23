---
title: 应为非高峰时间计划的复制的重新同步
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2d6c18b7e37c5d17f56f41c7ff03ed8796457de0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840018"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>应为非高峰时间计划的复制的重新同步

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|操作|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
*不会重新同步主虚拟机的复制的非高峰时间安排。*  
  
## <a name="impact"></a>影响  
*处于需要重新同步状态的虚拟机的时间越长，越长复制日志文件增长，并且主虚拟机上发生的更多未复制的更改。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*使用 Hyper-v 管理器来修改虚拟机的复制设置，以执行在非高峰时间内自动重新同步。*  
  


