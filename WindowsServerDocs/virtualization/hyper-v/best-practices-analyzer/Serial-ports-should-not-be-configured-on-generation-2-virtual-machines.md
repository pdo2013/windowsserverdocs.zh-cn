---
title: 不应在第2代虚拟机上配置串行端口
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 87061193-dd3f-4398-aa5d-4cee83cadfa3
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8a8c15076921efa0e1e791a18c6a45ea1bf27b0e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364735"
---
# <a name="serial-ports-should-not-be-configured-on-generation-2-virtual-machines"></a>不应在第2代虚拟机上配置串行端口

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
*一个或多个第2代虚拟机配置了串行端口。*  
  
## <a name="impact"></a>**对**  
*对于下列虚拟机，性能可能会受到影响：*  
  
@no__t-虚拟机的 0list >  
  
## <a name="resolution"></a>**解决方法**  
@no__t 0If，这是有意的，无需执行其他操作。否则，请考虑使用 Hyper-v 管理器或 Windows PowerShell 从虚拟机上的串行端口中删除连接字符串。 *  
  


