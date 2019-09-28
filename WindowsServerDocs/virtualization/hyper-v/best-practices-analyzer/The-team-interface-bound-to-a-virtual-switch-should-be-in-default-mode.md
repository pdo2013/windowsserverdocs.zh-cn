---
title: 绑定到虚拟交换机的团队接口应处于默认模式
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9bfd0c98e865a0faae8dd70e97696e2c2682531b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393426"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>绑定到虚拟交换机的团队接口应处于默认模式

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
*某些虚拟交换机绑定到团队界面，但团队界面不会将所有 Vlan 上的流量传递给虚拟交换机。*  
  
## <a name="impact"></a>**对**  
*以下虚拟交换机无法访问所有 Vlan： \n @ no__t-1*  
  
## <a name="resolution"></a>**解决方法**  
*使用服务器管理器或 Windows PowerShell cmdlet NetLbfoTeamNic 将团队界面重置为默认模式。*  
  


