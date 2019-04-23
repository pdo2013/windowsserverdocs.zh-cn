---
title: 避免在生产环境中运行的服务器工作负荷的虚拟机上使用检查点
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1be75890-d316-495a-b9b7-be75fc1aac10
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 166ef839a40452cc4156144e10e9c666e7ce3472
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856148"
---
# <a name="avoid-using-checkpoints-on-a-virtual-machine-that-runs-a-server-workload-in-a-production-environment"></a>避免在生产环境中运行的服务器工作负荷的虚拟机上使用检查点

>适用于：Windows Server 2016


  
*有关最佳做法和扫描的详细信息，请参阅*[最佳做法分析器](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|操作|  

在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。

> [!NOTE]  
> 在 Windows Server 2012 R2 中，虚拟机快照的 HYPER-V 管理器以匹配在 System Center 虚拟机管理中使用的术语中的虚拟机检查点已重命名。 有关详细信息，请参阅[检查点和快照概述](https://technet.microsoft.com/library/dn818483.aspx)。  
  
## <a name="issue"></a>问题  
  
*已找到具有一个或多个检查点的虚拟机。*  
  
## <a name="impact"></a>影响  
  
*存储检查点文件的物理磁盘上会耗尽可用空间。如果发生这种情况，可以在物理存储上不执行任何额外的磁盘操作。可能会影响任何依赖于物理存储的虚拟机。*  
  
如果物理磁盘空间耗尽，任何正在运行的虚拟机具有检查点或存储在该磁盘上的虚拟硬盘可能会自动暂停。 Hyper-v 管理器显示为"已暂停-关键"这些虚拟机的状态。  
  
## <a name="resolution"></a>分辨率  
  
*如果虚拟机在生产环境中运行的服务器工作负荷，使虚拟机脱机，然后使用 Hyper-v 管理器来应用或删除检查点。若要删除检查点，必须关闭虚拟机以完成该过程。*  
  
> [!NOTE]  
> 生产检查点现可作为标准检查点的替代方法。 有关详细信息，请参阅[标准或生产检查点之间进行选择](../manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)。  
  


