---
title: 请确保虚拟机使用差异虚拟硬盘时提供足够的物理磁盘空间
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 71f99aab-f994-4022-9da0-d661965b95ac
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6d4d219402cdc321a75cc27d75ea7749eb6127e0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855568"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-differencing-virtual-hard-disks"></a>请确保虚拟机使用差异虚拟硬盘时提供足够的物理磁盘空间

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
*一个或多个虚拟机使用差异虚拟硬盘。*  
  
## <a name="impact"></a>影响  
*差异虚拟硬盘需要托管的卷上的可用空间，以便为虚拟硬盘的写入操作发生时，可以分配空间。如果可用空间已用尽，可能会影响任何依赖于物理存储的虚拟机。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*监视可用磁盘空间，以确保有足够的空间是适用于虚拟硬盘扩展。请考虑合并到其父差异虚拟硬盘。在 HYPER-V 管理器中检查来确定父虚拟硬盘，差异磁盘。如果你合并到共享的其他差异磁盘的父磁盘的差异磁盘，该操作将损坏之间其他差异磁盘和父磁盘，使其不可用的关系。在验证不共享父虚拟硬盘后，可以使用编辑磁盘向导要合并到父虚拟硬盘，差异磁盘。*  
  


