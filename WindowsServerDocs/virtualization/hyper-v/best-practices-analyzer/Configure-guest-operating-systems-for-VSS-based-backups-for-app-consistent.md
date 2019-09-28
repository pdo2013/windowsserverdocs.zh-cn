---
title: 为基于 VSS 的备份配置来宾操作系统，以便为 Hyper-v 副本启用应用程序一致性快照
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 032ca585da1c556fff6f9e06b3bde0662a5d64db
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364959"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>为基于 VSS 的备份配置来宾操作系统，以便为 Hyper-v 副本启用应用程序一致性快照

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|Error|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>问题  
*与应用程序一致的快照要求在参与复制的虚拟机的来宾操作系统中启用并配置卷影复制服务（VSS）。*  
  
## <a name="impact"></a>影响  
*Even 如果在复制配置中指定了应用程序一致性快照，则 Hyper-v 将不会使用它们，除非已配置 VSS。这会影响以下虚拟机：*  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>分辨率  
*使用虚拟机连接在虚拟机中安装 integration services。*  
  


