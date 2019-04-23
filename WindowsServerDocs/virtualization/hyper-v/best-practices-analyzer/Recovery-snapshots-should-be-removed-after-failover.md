---
title: 故障转移后，应删除恢复快照
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4663320df91019fc7dc1d8ca7ffdb2fcc3e0de42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837678"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>故障转移后，应删除恢复快照

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016| 
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|操作|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>**问题**  
*一个故障转移虚拟机具有一个或多个恢复快照。*  
  
## <a name="impact"></a>**影响**  
*可用空间可能在存储快照文件的物理磁盘上运行。如果发生这种情况，可以在物理存储上不执行任何额外的磁盘操作。可能会影响任何依赖于物理存储的虚拟机。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>**解决方法**  
*对于每个故障转移虚拟机，使用 Complete-vmfailover cmdlet 在 Windows PowerShell 中可删除恢复快照，并指示故障转移完成。*  
  


