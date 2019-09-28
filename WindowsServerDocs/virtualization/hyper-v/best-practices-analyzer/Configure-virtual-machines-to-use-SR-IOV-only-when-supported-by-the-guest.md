---
title: 将虚拟机配置为仅在来宾操作系统支持时使用 SR-IOV
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 33cf5b68-e43e-47ef-adbc-6b266c1d4dce
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8c43e06806f66ce0faae255f0f34d80a653fbe10
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366265"
---
# <a name="configure-virtual-machines-to-use-sr-iov-only-when-supported-by-the-guest-operating-system"></a>将虚拟机配置为仅在来宾操作系统支持时使用 SR-IOV

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
*一台或多台虚拟机配置为使用单根 i/o 虚拟化（SR-IOV），但来宾操作系统不支持 SR-IOV*  
  
## <a name="impact"></a>影响  
*SR-IOV 虚拟函数将不会分配到以下虚拟机：*  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>分辨率  
*在运行不支持 SR-IOV 的来宾操作系统的所有虚拟机上禁用 SR-IOV。*  
  
只有有些64位 Windows 来宾支持 SR-IOV。 有关详细信息，请参阅[hyper-v 功能与生成和来宾的兼容性](../Hyper-V-feature-compatibility-by-generation-and-guest.md)。  
  


