---
title: 应从复制中排除页面文件使用的虚拟硬盘
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8b6289a82c83f3dcfc0de299250ce19ee3782678
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850118"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>应从复制中排除页面文件使用的虚拟硬盘

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|信息|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
*分页文件应排除参与复制，但没有磁盘已被排除。*  
  
## <a name="impact"></a>影响  
*分页文件更大的输入/输出活动，将不必要地需要多更高版本的资源，若要参与复制。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*如果尚未这样做，则创建单独的虚拟硬盘，Windows 页面文件。如果已完成初始复制，使用 HYPER-V 管理器删除复制。然后，再次配置复制，并使用此页面文件从复制中排除磁盘的虚拟硬盘。*  
  


