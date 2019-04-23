---
title: 不应在第 2 代虚拟机上配置的串行端口
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 87061193-dd3f-4398-aa5d-4cee83cadfa3
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 58c3fc5f975b85ce17ac5f7cca4930ec9e851e07
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877378"
---
# <a name="serial-ports-should-not-be-configured-on-generation-2-virtual-machines"></a>不应在第 2 代虚拟机上配置的串行端口

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
*一个或多个生成 2 个虚拟机已配置的串行端口。*  
  
## <a name="impact"></a>**影响**  
*对以下虚拟机可能会影响性能：*  
  
\<虚拟机的列表 >  
  
## <a name="resolution"></a>**解决方法**  
*如果这是有意为之，不不需要任何进一步的操作。否则，请考虑使用 HYPER-V 管理器或 Windows PowerShell 删除虚拟机上的串行端口的连接字符串。*  
  


