---
title: 避免虚拟基块和动态虚拟硬盘或差异磁盘上的物理磁盘扇区之间的对齐方式不一致
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4973c199a5507d00e15da8f621a09f0c602a29fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833858"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>避免虚拟基块和动态虚拟硬盘或差异磁盘上的物理磁盘扇区之间的对齐方式不一致

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
*对齐方式不一致情况已检测到一个或多个虚拟硬盘。*  
  
### <a name="impact"></a>影响  
*如果虚拟硬盘的存储上的扇区大小为 4k、 虚拟机或应用程序使用虚拟硬盘的物理磁盘可能会遇到性能问题。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*创建虚拟硬盘向导用于创建新 VHD 格式或者 VHDX 格式虚拟硬盘，并指定为源磁盘的现有虚拟硬盘。使用虚拟基块和物理磁盘之间的对齐方式，将创建新的虚拟硬盘。*  
  


