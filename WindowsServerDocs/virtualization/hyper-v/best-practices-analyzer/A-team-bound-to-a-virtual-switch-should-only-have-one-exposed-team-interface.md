---
title: 绑定到虚拟交换机的团队应只有一个公开的团队界面
description: 此最佳做法分析器规则文本的联机版本。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6baa9e4ae900c9b671003872b4eb4589efb2f085
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365400"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>绑定到虚拟交换机的团队应只有一个公开的团队界面

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
*一个或多个虚拟交换机绑定到具有多个团队界面的团队。*  
  
## <a name="impact"></a>**对**  
*以下虚拟交换机可能无权访问其他团队界面使用的 Vlan 和带宽：*  
  
@no__t-虚拟交换机的 0list >  
  
## <a name="resolution"></a>**解决方法**  
*使用 Windows PowerShell cmdlet NetLbfoTeamNic 从团队中删除除默认团队界面之外的所有团队界面。*  
  


