---
title: 应启用所有虚拟网络适配器
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0a769c3203f6c6946f01cd91b66fbec38af83bbd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837128"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>应启用所有虚拟网络适配器

>适用于：Windows Server 2016


  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*管理操作系统中禁用了与物理网络适配器关联的一个或多个虚拟网络适配器。*  
  
## <a name="impact"></a>影响  
  
*此服务器配置并非最佳选择。*  
  
管理操作系统无法连接到使用一个物理网络适配器在此计算机上，因为它具有与禁用的虚拟网络适配器关联的物理 （外部） 网络。  
  
## <a name="resolution"></a>分辨率  
  
*使用网络和 Internet 设置来启用虚拟网络适配器。或者，使用虚拟交换机管理器重新配置的外部虚拟交换机，使它不会与管理操作系统共享。*  
  


