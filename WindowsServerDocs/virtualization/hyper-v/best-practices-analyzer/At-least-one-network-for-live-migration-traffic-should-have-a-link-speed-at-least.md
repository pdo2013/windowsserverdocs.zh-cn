---
title: 至少一个网络用于实时迁移流量应具有至少 1 Gbps 的链接速度
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5714df3f-f810-4618-8c93-e24881651100
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ef75f73bd934b863b146e93f4cdc7323e5d4c6fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837728"
---
# <a name="at-least-one-network-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>至少一个网络用于实时迁移流量应具有至少 1 Gbps 的链接速度

>适用于：Windows Server 2016


  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|错误|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
*无实时迁移通信的网络具有至少 1 Gbps 的链接速度。*  
  
## <a name="impact"></a>影响  
*这可能会由于 TCP 连接超时的网络连接中断，实时迁移可能速度慢，会出现。*  
  
## <a name="resolution"></a>分辨率  
*至少一个实时迁移网络配置为 1 Gbps 或更快的速度。*  
  
请参阅网络硬件供应商，以找出的任何现有的网络适配器是否可以支持至少 1 Gbps 的链接速度的文档。  
  


