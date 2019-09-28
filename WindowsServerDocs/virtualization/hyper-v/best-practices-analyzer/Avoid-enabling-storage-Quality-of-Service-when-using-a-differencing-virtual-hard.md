---
title: 当父虚拟硬盘和子虚拟硬盘位于不同的卷上时，请避免启用存储服务质量
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 716a32de2f9327e5eca38c470fa1b7c44150e9cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366437"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>当父虚拟硬盘和子虚拟硬盘位于不同的卷上时，请避免启用存储服务质量

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。
  
## <a name="issue"></a>**问题**  
*不同卷上具有父虚拟硬盘和子虚拟硬盘的差异虚拟硬盘已启用存储服务质量。*  
  
## <a name="impact"></a>**对**  
@no__t 0This 配置可能会导致差异虚拟硬盘的意外存储质量服务行为以及父和子卷上的其他虚拟硬盘。这会影响以下虚拟硬盘： *  
  
@no__t 虚拟硬盘 >  
  
## <a name="resolution"></a>**解决方法**  
*在被引用的虚拟硬盘上禁用存储服务质量，或执行存储迁移，将父级和子虚拟硬盘移到相同的卷。*  
  


