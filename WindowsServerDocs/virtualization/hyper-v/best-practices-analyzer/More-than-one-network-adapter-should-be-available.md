---
title: 多个网络适配器应可用
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 678c0161e97b8dd022bbf0037d9add5de0281f77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884598"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>多个网络适配器应可用

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 [最佳实践分析程序](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|错误|  
|**类别**|配置|  

在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。

## <a name="issue"></a>问题  
  
*此服务器配置使用一个网络适配器，必须由管理操作系统共享和需要访问物理网络的所有虚拟机。*  
  
## <a name="impact"></a>影响  
  
*管理操作系统中，网络性能可能会下降。*  
  
## <a name="resolution"></a>分辨率  
  
*将更多的网络适配器添加到此计算机。若要保留由管理操作系统独自使用的一个网络适配器，不对其进行配置以用于外部虚拟网络。*  
  
有关将网络适配器添加到计算机的信息，请查阅计算机或网络适配器的文档。 然后，若要保留它专用于管理操作系统，不将其连接到虚拟交换机。   
  


