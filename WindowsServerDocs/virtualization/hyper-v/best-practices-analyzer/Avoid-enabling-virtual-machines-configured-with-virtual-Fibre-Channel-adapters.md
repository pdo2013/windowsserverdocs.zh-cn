---
title: 避免在使用虚拟光纤通道适配器，以便实时迁移时有更少的路径比源上在目标上的光纤通道逻辑单元 (Lun) 配置虚拟机
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6ff69d5cb09133a806c2a2df3446713264a4e892
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849548"
---
# <a name="avoid-enabling-virtual-machines-configured-with-virtual-fibre-channel-adapters-to-allow-live-migrations-when-there-are-fewer-paths-to-fibre-channel-logical-units-luns-on-the-destination-than-on-the-source"></a>避免在使用虚拟光纤通道适配器，以便实时迁移时有更少的路径比源上在目标上的光纤通道逻辑单元 (Lun) 配置虚拟机

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|

在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。
  
## <a name="issue"></a>**问题**  
*一个或多个虚拟机有虚拟化 WMI 提供程序中设置的 AllowReducedFcRedunancy 属性。*  
  
## <a name="impact"></a>**影响**  
*以下虚拟机的实时迁移可能会导致数据丢失或 I/O 中断到存储：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>**解决方法**  
*请考虑清除受影响的虚拟机上的 AllowReducedFcRedundancy WMI 属性。清除此属性后，您可以仅当在目标上的光纤通道的路径数是同一个或多个源的路径数量时，使用虚拟光纤通道适配器配置虚拟机上执行实时迁移。这些检查有助于防止数据丢失或 I/O 的中断到存储。* 