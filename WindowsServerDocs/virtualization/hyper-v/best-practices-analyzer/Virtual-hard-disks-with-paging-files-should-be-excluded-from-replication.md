---
title: 应从复制中排除包含页面文件的虚拟硬盘
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 215e265d69af1b384d3461c627558ff0a59c8e91
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364551"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>应从复制中排除包含页面文件的虚拟硬盘

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|Information|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>问题  
*应排除分页文件以参与复制，但不包括任何磁盘。*  
  
## <a name="impact"></a>影响  
@no__t 0Paging 文件遇到大量的输入/输出活动，这将不必要地需要更多的资源来参与复制。这会影响以下虚拟机： *  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>分辨率  
*If 你尚未执行此操作，请为 Windows 页面文件创建一个单独的虚拟硬盘。如果初始复制已完成，请使用 Hyper-v 管理器删除复制。然后，再次配置复制，并从复制中排除包含页面文件的虚拟硬盘。*  
  


