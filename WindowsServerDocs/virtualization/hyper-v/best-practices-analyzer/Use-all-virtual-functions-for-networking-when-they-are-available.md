---
title: 在可用时使用所有虚拟函数进行网络连接
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8798a7021b3df0113b8d957340d6d688acead5c7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393347"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>在可用时使用所有虚拟函数进行网络连接

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
*某些硬件加速功能没有被使用*  
  
## <a name="impact"></a>影响  
@no__t 0This 配置可能会导致总体 CPU 利用率高于所需的使用率。在以下虚拟机上，网络性能可能不是最佳的： *  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>分辨率  
*如果物理硬件支持 SR-IOV 并且此配置不与虚拟机所需的网络功能发生冲突，请考虑为 SR-IOV 配置虚拟网络适配器。*  
  


