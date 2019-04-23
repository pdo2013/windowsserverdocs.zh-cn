---
title: 授权条目应具有不同的信任组名，以便与不是相同的信任组的一部分的虚拟机的主服务器
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e5c5b1a6bf8ef0bbceb5dde6b28cd951f399fc5e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882958"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>授权条目应具有不同的信任组名，以便与不是相同的信任组的一部分的虚拟机的主服务器

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
*服务器将接受来自任何与同一个复制标记为虚拟机关联的授权列表中的服务器在副本虚拟机的复制请求。*  
  
## <a name="impact"></a>**影响**  
*可能存在隐私和安全问题与接受从属于不同的授权条目的主服务器复制的虚拟机。这会影响以下授权条目：\<的授权条目的列表 >*  
  
## <a name="resolution"></a>**解决方法**  
*对于不属于同一安全组的虚拟机的主服务器的授权条目中使用不同的标记。修改 HYPER-V 设置，配置复制标记。*  
  


