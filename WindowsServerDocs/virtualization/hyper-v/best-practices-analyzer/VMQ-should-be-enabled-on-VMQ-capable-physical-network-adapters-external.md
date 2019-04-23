---
title: 应支持 VMQ 的绑定到外部虚拟交换机的物理网络适配器上启用 VMQ
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9a552c15675e6ca7a7310c8c9eaec883653987be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889468"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>应支持 VMQ 的绑定到外部虚拟交换机的物理网络适配器上启用 VMQ

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>**问题**  
*以下网络适配器都能使用的虚拟机队列 (VMQ)，但功能已禁用。*  
  
## <a name="impact"></a>**影响**  
*Windows 不能在以下的网络适配器上充分利用可用硬件卸载功能：*  
  
\<网络适配器列表 >  
  
## <a name="resolution"></a>**解决方法**  
*启用 VMQ 使用 Enable-netadaptervmq Windows PowerShell cmdlet 或使用网络适配器的高级属性用户界面。*  
  


