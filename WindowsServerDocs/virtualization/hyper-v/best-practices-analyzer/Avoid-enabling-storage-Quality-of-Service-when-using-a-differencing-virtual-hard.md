---
title: 避免当使用差异虚拟硬盘时父和子虚拟硬盘位于不同的卷上启用存储服务质量
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2bdc8462c4d9dc50dbb69792f2f294add0ca3a74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856198"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>避免当使用差异虚拟硬盘时父和子虚拟硬盘位于不同的卷上启用存储服务质量

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
*包含父和子虚拟硬盘在不同卷上的差异虚拟硬盘已存储启用服务的质量。*  
  
## <a name="impact"></a>**影响**  
*此配置可能会导致意外的存储服务质量行为差异的虚拟硬盘，以及其他父级和子级卷上的虚拟硬盘。这会影响以下虚拟硬盘：*  
  
\<虚拟硬盘的列表 >  
  
## <a name="resolution"></a>**解决方法**  
*禁用存储服务质量上引用的虚拟硬盘，或执行存储迁移，以便将父和子虚拟硬盘移到同一个卷。*  
  


