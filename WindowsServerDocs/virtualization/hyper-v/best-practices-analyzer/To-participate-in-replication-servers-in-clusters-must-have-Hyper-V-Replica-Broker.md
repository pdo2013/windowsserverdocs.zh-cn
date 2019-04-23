---
title: 若要参与复制，故障转移群集中的服务器必须具有配置的 HYPER-V 副本代理
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: d4966396af955f9c8bad34b5b2892115e93c3b85
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887968"
---
# <a name="to-participate-in-replication-servers-in-failover-clusters-must-have-a-hyper-v-replica-broker-configured"></a>若要参与复制，故障转移群集中的服务器必须具有配置的 HYPER-V 副本代理

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|错误|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
*用于故障转移群集的 HYPER-V 副本需要使用 HYPER-V 副本代理名称而不是单独的服务器名称。*  
  
## <a name="impact"></a>影响  
*如果虚拟机移到不同的故障转移群集节点，无法继续复制。*  
  
## <a name="resolution"></a>分辨率  
*使用故障转移群集管理器配置 HYPER-V 副本代理。在 HYPER-V 管理器中，请确保复制配置与服务器的名称使用 HYPER-V 副本代理名称。*  
  


