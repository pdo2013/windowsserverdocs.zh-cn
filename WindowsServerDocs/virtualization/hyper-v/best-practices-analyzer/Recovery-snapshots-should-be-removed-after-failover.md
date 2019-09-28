---
title: 故障转移后，应删除恢复快照
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4b8574956fb1b46ca0cf9678187fffcd68c2d261
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393526"
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
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>**问题**  
*故障转移虚拟机有一个或多个恢复快照。*  
  
## <a name="impact"></a>**对**  
@no__t 0Available 的空间可能在存储快照文件的物理磁盘上运行。如果发生这种情况，则不能在物理存储上执行其他磁盘操作。依赖于物理存储的任何虚拟机都可能会受到影响。这会影响以下虚拟机： *  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>**解决方法**  
*对于每个故障转移的虚拟机，请使用 Windows PowerShell 中的 Start-vmfailover cmdlet 来删除恢复快照并指示故障转移完成。*  
  


