---
title: 对于包含不属于同一信任组的虚拟机的主服务器，授权条目应具有不同的信任组名称
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ab71e0e6562b73d81b871914fd0e9e76570c518e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366536"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>对于包含不属于同一信任组的虚拟机的主服务器，授权条目应具有不同的信任组名称

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
*服务器将从与虚拟机相同的复制标记相关联的授权列表中的任何服务器接受对副本虚拟机的复制请求。*  
  
## <a name="impact"></a>**对**  
对于从属于不同授权条目的主服务器进行复制的虚拟机，@no__t 0There 可能是隐私和安全问题。这会影响以下授权条目： \<list of authorization > *  
  
## <a name="resolution"></a>**解决方法**  
*Use 不属于同一安全组的虚拟机的主服务器的授权条目中的不同标记。修改 Hyper-v 设置以配置复制标记。*  
  


