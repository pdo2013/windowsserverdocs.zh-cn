---
title: 应至少每周备份一次虚拟机
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ec11b067de2c9f8cbb3a17731caa0dc526bf54a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393234"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>应至少每周备份一次虚拟机

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|Error|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>问题  
*过去一周内没有备份一个或多个虚拟机。*  
  
## <a name="impact"></a>影响  
如果虚拟机遇到问题且最近的备份不存在，则可能会发生 @no__t 0Significant 数据丢失。这会影响以下虚拟机： *  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>分辨率  
*Schedule 备份虚拟机，使其每周至少运行一次。如果此虚拟机是一个副本，并且正在备份其主虚拟机，或者如果这是主虚拟机并且其副本正在进行备份，则可以忽略此规则。*  
  


