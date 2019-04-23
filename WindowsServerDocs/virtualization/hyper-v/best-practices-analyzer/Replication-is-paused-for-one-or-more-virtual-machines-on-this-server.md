---
title: 此服务器上，复制已暂停的一个或多个虚拟机
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 248b5fbdbfb54380e441d14cde6beaa9146ce800
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827768"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>此服务器上，复制已暂停的一个或多个虚拟机

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|操作|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
*复制已暂停为一个或多个虚拟机。在主虚拟机暂停时，会发生任何更改将累积和后恢复复制发送到副本虚拟机。*  
  
## <a name="impact"></a>影响  
*只要复制已暂停，累积的更改发生在主虚拟机中将使用主服务器上的可用磁盘空间。恢复复制后，可能有大型突然增加为副本服务器的网络流量。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*确认暂停复制的初衷。如果复制已暂停到地址低磁盘空间或网络连接，这些问题都得到解决时，就立即恢复复制。*  
  


