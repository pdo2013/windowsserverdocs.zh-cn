---
title: "广告森林恢复虚拟化"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adfs
ms.openlocfilehash: 08b0c4a4c5ed230f91241fcfb67db1749935c5e2
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# <a name="active-directory-forest-recovery-virtualization"></a>Active Directory 森林恢复虚拟化

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

本主题介绍 Windows Server 2016、2012 R2、2012 年中的虚拟化的域控制器克隆功能。  
 
## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>使用复制虚拟化的域控制器加快森林恢复  
 虚拟化的域控制器 (DC) 复制简化了和加快在域中，尤其是在中心位置数据中心如在管理程序运行多个域控制器安装额外的虚拟化的 Dc 的过程。 从备份中还原每域中的一个虚拟 DC 后，每个域中的其他 Dc 可以快速联机通过虚拟化的 DC 复制过程。 您可以准备好恢复、关机它，，然后将复制的第一个虚拟化的 DC 该虚拟硬盘在许多情况下，如有必要，以便创建复制虚拟化 Dc 为构建域。  
  
 虚拟化 DC 复制要求如下：  
  
-   虚拟机监控程序必须支持 VM-GenerationID。 Windows Server 2016，2012 Hyper-V 虚拟机监控程序支持 VM-GenerationID 它的一个示例 Windows 8，并且。 如果 VM-GenerationID 受支持，请咨询你虚拟机监控程序供应商。  
  
-   复制作为源用于虚拟化的 DC 必须运行 Windows Server 2016 或 2012 年和是克隆域控制器组中的成员。  
  
-   PDC 模拟器必须运行 Windows Server 2016 或 2012 年。 如果虚拟化时，您可以复制 PDC 仿真器。  
  
 有关如何执行的分步说明虚拟化 DC 复制，请参阅[简介 Active Directory 域服务 (广告 DS) 虚拟化 (级别 100)](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)。 有关详细信息虚拟化 DC 复制有效，请参阅[虚拟化域控制器技术参考 (级别 300)](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md)。  

-   [广告森林恢复-先决条件](AD-Forest-Recovery-Prerequisties.md)  
-   [广告森林恢复-在寻找自定义森林恢复套餐](AD-Forest-Recovery-Devising-a-Plan.md)  
- [广告森林恢复-找出问题](AD-Forest-Recovery-Identify-the-Problem.md)
-   [广告森林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [广告森林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)  
-   [广告森林恢复-常见问题](AD-Forest-Recovery-FAQ.md)  
-   [广告森林恢复-恢复单个域中多域森林](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [广告森林恢复-森林恢复与 Windows Server 2003 该域控制器](AD-Forest-Recovery-Windows-Server-2003.md) 

  
