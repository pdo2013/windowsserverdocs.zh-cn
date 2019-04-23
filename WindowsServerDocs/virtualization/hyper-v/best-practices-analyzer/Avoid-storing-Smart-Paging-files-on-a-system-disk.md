---
title: 避免将存储在系统磁盘上的智能分页文件
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6abc84b406de7e7c33628ccee4e3af706efe5c70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886168"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>避免将存储在系统磁盘上的智能分页文件

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|操作|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的文本。  
  
## <a name="issue"></a>问题  
*如果虚拟机重新启动时，和智能分页文件的指定的位置是运行 HYPER-V 的服务器的系统磁盘，一个或多个虚拟机的内存配置可能需要使用智能分页。*  
  
## <a name="impact"></a>影响  
*智能分页系统磁盘的使用可能会导致运行 HYPER-V 时遇到问题的服务器。这会影响以下虚拟机：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>分辨率  
*重新配置虚拟机存储在非系统磁盘上的智能分页文件。*  
  


