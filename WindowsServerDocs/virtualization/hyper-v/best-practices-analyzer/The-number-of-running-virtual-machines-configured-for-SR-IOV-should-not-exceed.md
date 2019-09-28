---
title: 为 SR-IOV 配置的正在运行的虚拟机的数量不应超过可供虚拟机使用的虚拟功能的数目
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0887035c84ebc4b7d93163533387f2f8ab20fb87
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364615"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>为 SR-IOV 配置的正在运行的虚拟机的数量不应超过可供虚拟机使用的虚拟功能的数目

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>问题  
*对于为单根 i/o 虚拟化（SR-IOV）配置的正在运行的虚拟机数，没有足够的可用虚拟函数。*  
  
## <a name="impact"></a>影响  
*在以下虚拟机上，网络性能可能不是最佳的：*  
   
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>分辨率  
*请考虑在不需要 SR-IOV 虚拟功能的一个或多个虚拟机上禁用 SR-IOV。*  
  


