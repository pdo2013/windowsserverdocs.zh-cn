---
title: "广告森林恢复-步骤还原森林"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 29988c262897649173e039cc052bb64f38a1a0ca
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
#<a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>广告森林恢复-步骤还原森林 

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

此部分中恢复树林中提供推荐的路径的概述。 稍后将详细介绍森林恢复步骤。  
  
 下面总结了较高级别恢复步骤：  
  
1.  [标识该问题](AD-Forest-Recovery-Identify-the-Problem.md)  
  
     适用于 IT 和 Microsoft 支持，以确定该问题并可能的故障原因之外的范围并与所有业务负责人评估可能补救方法。 在许多情况下总森林恢复应该的最后一个选项。  
  
2.  [确定如何恢复森林](AD-Forest-Recovery-Determine-how-to-Recover.md)  
  
     你确定森林恢复有必要后，完整初步文档步骤其准备：确定当前森林结构、识别每个直流执行，确定哪些 DC 还原为每个域中，并确保所有可写 Dc 拍摄离线状态下的功能。  
  
3.  [执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
  
     在隔离，恢复为每个域一个直流、清洁它们，并重新连接域。 重置特权的帐户，并解决问题所致在此阶段安全漏洞。  
  
4.  [重新部署剩余 Dc](AD-Forest-Recovery-Restore-Additional-DCs.md)  
  
     重新部署森林以将其返回到之前故障其状态。 此步骤中将需要为你的特定设计和要求满足。 虚拟化的域控制器复制有助于加快此过程。  
  
5.  [清理](AD-Forest-Recovery-Cleanup.md)  
  
     在还原功能后，重新名称分辨率配置根据需要并获取 LOB 工作的应用程序。  

  
 本指南中的步骤旨在最大程度减少可能重新危险数据引入到恢复森林。 你可能需要修改以涵盖因素这些步骤：  
  
-   可扩展性  
-   远程可管理性  
-   恢复速度  

