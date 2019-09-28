---
ms.assetid: 680e05ac-f9be-4b07-a9f4-cd6da5835952
title: Active Directory 林恢复指南
description: 本指南提供有关备份、还原和 active directory 灾难恢复的指导。
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ee31076669351a92caf79c6b8972ccdd68b347aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390436"
---
# <a name="active-directory-forest-recovery-guide"></a>Active Directory 林恢复指南

>适用于：Windows Server 2016，Windows Server 2012 和 2012 R2，Windows Server 2008 和 2008 R2，Windows Server 2003

本指南包含有关以下方面的最佳实践建议：在林范围内发生故障时，在林范围内® Active Directory 将所有域控制器（Dc）都呈现为无法正常工作的林中的所有域控制器（Dc）。 它包含的步骤用作林恢复计划的模板，你可以为你的特定环境自定义此模板。 这些步骤适用于运行 Microsoft® Windows Server 2016、2012 R2、2012、2008 R2、2008和2003操作系统的 Dc。  
  
> [!NOTE]
> 在[AD 林恢复 Windows server 2003](AD-Forest-Recovery-Windows-Server-2003.md)中合并了运行 Windows server 2003 的 dc 的唯一过程。  
  
## <a name="steps-outlined-in-this-guide"></a>本指南中概述的步骤
  
- [AD 林恢复 - 先决条件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 林恢复-设计自定义林恢复计划](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 林恢复 - 恢复步骤](AD-Forest-Recovery-Steps-For-Restoring.md)
- [AD 林恢复-识别问题](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)  
- [AD 林恢复-常见问题](AD-Forest-Recovery-FAQ.md)  
- [AD 林恢复-恢复多域林中的单个域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 林恢复 - 虚拟化](AD-Forest-Recovery-Virtualization.md)
- [AD 林恢复-通过 Windows Server 2003 域控制器恢复林](AD-Forest-Recovery-Windows-Server-2003.md)  
