---
title: 确保所有必需的虚拟交换机扩展可用
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 53ceeb9aab6ca7196454fbcd7f0fdae8b34d05d2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825938"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>确保所有必需的虚拟交换机扩展可用

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
*一个或多个虚拟网络适配器连接到已禁用或未安装的必需扩展的虚拟交换机。*  
  
## <a name="impact"></a>影响  
*在以下虚拟机上的一个或多个虚拟网络适配器上阻止网络流量：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*首先，请确保在主机上已安装必需的扩展程序并安装扩展，如有必要。然后，如果禁用了必需的扩展，使用虚拟交换机管理器或 Windows PowerShell cmdlet Enable-vmswitchextension 启用扩展。*  
  


