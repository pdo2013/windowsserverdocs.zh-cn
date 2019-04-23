---
title: 绑定到虚拟交换机的团队接口应在默认模式
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 22e5ad0eed6e6ea07a83150762b76163442f2c5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872158"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>绑定到虚拟交换机的团队接口应在默认模式

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
*有些虚拟交换机绑定到组接口，但团队接口不到虚拟交换机上所有 Vlan 中传递流量。*  
  
## <a name="impact"></a>**影响**  
*以下虚拟交换机不能有权访问所有 Vlan: \n{0}*  
  
## <a name="resolution"></a>**解决方法**  
*使用服务器管理器或 Windows PowerShell cmdlet 集 NetLbfoTeamNic 将团队界面重置为默认模式。*  
  


