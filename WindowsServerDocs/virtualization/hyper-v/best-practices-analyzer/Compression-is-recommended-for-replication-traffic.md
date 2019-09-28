---
title: 建议对复制通信使用压缩
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: cf8be6e9-2909-4e4a-bb63-d1e1ebbc6930
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 77a314e816c36f626ea3edb10b80f65e3897e7c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365096"
---
# <a name="compression-is-recommended-for-replication-traffic"></a>建议对复制通信使用压缩

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
*通过网络从主服务器发送到副本服务器的复制流量未压缩。*  
  
## <a name="impact"></a>影响  
@no__t 0Replication 流量将使用比所需的更多的带宽。这会影响以下虚拟机： *  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>分辨率  
@no__t 0Configure Hyper-v 副本来压缩 hyper-v 管理器中虚拟机设置中通过网络传输的数据。你还可以使用 Hyper-v 之外的工具来执行压缩。 *  
  


