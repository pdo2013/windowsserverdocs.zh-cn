---
title: AD 林恢复虚拟化
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: c055445c2d3aecd8c6d92e94799f556962c977bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390132"
---
# <a name="active-directory-forest-recovery-virtualization"></a>Active Directory 林恢复虚拟化

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

本主题介绍 Windows Server 2016、2012 R2 和2012中的虚拟化域控制器克隆功能。  

## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>使用虚拟化域控制器克隆来加快林恢复

虚拟化域控制器（DC）克隆简化并加速了在域中安装额外的虚拟化 Dc 的过程，尤其是在集中的位置（例如，在虚拟机监控程序上运行多个 Dc 的数据中心）。 从备份中还原每个域中的一个虚拟 DC 后，每个域中的其他 Dc 可以通过使用虚拟化 DC 克隆过程快速联机。 可以准备要恢复的第一个虚拟化 DC，然后将其关闭，然后根据需要复制该虚拟硬盘，以便创建克隆的虚拟化 Dc 来构建域。  
  
虚拟化 DC 克隆的要求如下：  
  
- 虚拟机监控程序必须支持 VM 生成 id。 Windows Server 2016、2012和 Windows 8 中的 hyper-v 是支持 VM 生成 id 的虚拟机监控程序的示例。 如果支持 VM-生成 id，请与虚拟机监控程序供应商核实。  
- 用作克隆源的虚拟化 DC 必须运行 Windows Server 2016 或2012，并且必须是可克隆域控制器组的成员。 
- PDC 仿真器必须运行 Windows Server 2016 或2012。 如果是虚拟化的，则可以克隆 PDC 模拟器。  
  
有关如何执行虚拟化 DC 克隆的分步说明，请参阅[Active Directory 域服务（AD DS）虚拟化简介（等级100）](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)。 有关虚拟化 DC 克隆工作原理的详细信息，请参阅[虚拟化域控制器技术参考（级别300）](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md)。 

## <a name="next-steps"></a>后续步骤

- [AD 林恢复 - 先决条件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 林恢复-设计自定义林恢复计划](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 林恢复-识别问题](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)  
- [AD 林恢复-常见问题](AD-Forest-Recovery-FAQ.md)  
- [AD 林恢复-恢复多域林中的单个域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 林恢复-通过 Windows Server 2003 域控制器恢复林](AD-Forest-Recovery-Windows-Server-2003.md) 
