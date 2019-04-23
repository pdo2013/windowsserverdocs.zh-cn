---
title: 运行配置的 SR-IOV 的虚拟机数不应超过可供虚拟机的虚拟函数的数目
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e63df9283927437f9cfc62c052d83b07fe599b34
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829748"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>运行配置的 SR-IOV 的虚拟机数不应超过可供虚拟机的虚拟函数的数目

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
*没有足够虚函数可用于运行针对单根 I/O 虚拟化 (SR-IOV) 配置的虚拟机的数目。*  
  
## <a name="impact"></a>影响  
*网络性能可能不是最佳的以下虚拟机上：*  
   
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*请考虑在不需要的 SR-IOV 虚拟功能的一个或多个虚拟机上禁用 SR-IOV。*  
  


