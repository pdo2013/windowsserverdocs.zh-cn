---
title: AD 林恢复-用于还原林的步骤
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 1712d3a636160f82495539afdd42ab2ee85ffae2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861468"
---
# <a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>AD 林恢复-用于还原林的步骤

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

本部分概述了用于恢复林的推荐路径。 更高版本中详细介绍林恢复步骤。  
  
以下列表总结了在高级别的恢复步骤：  
  
1. [确定此问题](AD-Forest-Recovery-Identify-the-Problem.md)  

   使用 IT 和 Microsoft 支持部门以确定问题和可能的原因的范围，并与所有业务利益相关者评估可能的补救措施。 在许多情况下恢复总林应最后一个选项。  
  
2. [决定如何恢复林](AD-Forest-Recovery-Determine-how-to-Recover.md)  

   你确定必要的林恢复后，完整的初步文档步骤做好准备： 确定当前的林结构，标识的函数的每个 DC 执行时，决定哪些 DC 若要还原的每个域，并确保所有可写域控制器是脱机状态。  

3. [执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  

   在隔离中，恢复为每个域的一个 DC、 其，清除和重新连接域。 重置特权的帐户，并纠正在此阶段中引起安全漏洞的问题。  
  
4. [重新部署剩余 Dc](AD-Forest-Recovery-Restore-Additional-DCs.md)  

   重新部署林恢复到在发生故障之前其状态。 此步骤将需要适应特定的设计和要求。 虚拟化的域控制器克隆可帮助加速此过程。  

5. [清理](AD-Forest-Recovery-Cleanup.md)  

   还原功能之后，根据需要重新配置名称解析，并获取使用 LOB 应用程序。  

本指南中的步骤旨在最大程度减少重新危险数据引入到已恢复目录林中的可能性。 您可能必须修改等因素考虑以下步骤：  
  
- 可伸缩性  
- 远程可管理性  
- 恢复速度快  
