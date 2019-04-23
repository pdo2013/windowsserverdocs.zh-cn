---
title: 至少每月执行测试故障转移，以验证故障转移会成功且故障转移后虚拟机工作负荷将充当预期
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 57a8aa50-e59e-4a4b-8571-1099d5a8eee4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 879c860caede942393f0929faab9e4d567f225a6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856138"
---
# <a name="test-failovers-should-be-carried-out-at-least-monthly-to-verify-that-failover-will-succeed-and-that-virtual-machine-workloads-will-operate-as-expected-after-failover"></a>至少每月执行测试故障转移，以验证故障转移会成功且故障转移后虚拟机工作负荷将充当预期

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
*在至少一个月内已被任何测试故障转移。*  
  
## <a name="impact"></a>影响  
*没有计划内或计划外故障转移会成功，或在故障转移后将正确地继续工作负荷操作确认。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*使用 HYPER-V 管理器执行测试故障转移。*  
  


