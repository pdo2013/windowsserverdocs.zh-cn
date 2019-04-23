---
title: 绑定到虚拟交换机的团队应只有一个公开的团队接口
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 108bbec1439959bb7ab4475b59c7231653952ea8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838458"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>绑定到虚拟交换机的团队应只有一个公开的团队接口

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
*一个或多个虚拟交换机绑定到具有多个团队接口团队。*  
  
## <a name="impact"></a>**影响**  
*以下虚拟交换机不可能有权访问 Vlan 和带宽使用的其他团队接口：*  
  
\<虚拟交换机的列表 >  
  
## <a name="resolution"></a>**解决方法**  
*使用 Windows PowerShell cmdlet 删除 NetLbfoTeamNic 从非默认团队接口团队删除团队的所有接口。*  
  


