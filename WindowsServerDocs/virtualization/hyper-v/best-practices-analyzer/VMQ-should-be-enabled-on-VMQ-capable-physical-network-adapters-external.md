---
title: 应在绑定到外部虚拟交换机的支持 VMQ 的物理网络适配器上启用 VMQ。
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e8607ee891ef693d4e4e7a868540237855aebd2e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393286"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>应在绑定到外部虚拟交换机的支持 VMQ 的物理网络适配器上启用 VMQ。

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>**问题**  
*以下网络适配器支持虚拟机队列（VMQ），但功能被禁用。*  
  
## <a name="impact"></a>**对**  
*Windows 无法充分利用以下网络适配器上的可用硬件卸载：*  
  
@no__t 网络适配器的 0list >  
  
## <a name="resolution"></a>**解决方法**  
*使用 Get-netadaptervmq Windows PowerShell cmdlet 或使用网络适配器的高级属性用户界面启用 VMQ。*  
  


