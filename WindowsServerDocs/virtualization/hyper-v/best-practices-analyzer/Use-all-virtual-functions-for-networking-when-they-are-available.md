---
title: 所有虚拟函数用于在网络可用时
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3ad120ffa689f1f7dcae832432e216ebda57e62f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877788"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>所有虚拟函数用于在网络可用时

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
*某些硬件加速功能都不能使用*  
  
## <a name="impact"></a>影响  
*此配置可能导致总体 CPU 使用率达到高于必要的水平。网络性能可能不是最佳的以下虚拟机上：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*请考虑如果物理硬件支持 SR-IOV 和与虚拟机所需的网络功能不会不冲突此配置的 SR-IOV 配置虚拟网络适配器。*  
  


