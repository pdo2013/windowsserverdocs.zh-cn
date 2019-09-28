---
title: 此服务器上的一个或多个虚拟机的复制已暂停
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 17d50f116c6cee488367c924bfbce3791a8d879f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393541"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>此服务器上的一个或多个虚拟机的复制已暂停

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|操作|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>问题  
对于一个或多个虚拟机，@no__t 已暂停0Replication。在主虚拟机暂停的情况下，发生的任何更改都将累积，并将在恢复复制后发送到副本虚拟机。 *  
  
## <a name="impact"></a>影响  
*As 长，因为复制已暂停，在主虚拟机中发生的累积更改将消耗主服务器上的可用磁盘空间。在恢复复制后，可能会向副本服务器突发大量的网络流量。这会影响以下虚拟机：*  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>分辨率  
正在暂停复制的 0Confirm @no__t。如果复制已暂停，无法处理磁盘空间不足或网络连接问题，请在解决这些问题后立即恢复复制。 *  
  


