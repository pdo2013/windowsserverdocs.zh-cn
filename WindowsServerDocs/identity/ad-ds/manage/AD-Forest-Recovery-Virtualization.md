---
title: AD 林恢复虚拟化
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: 23317f55fdce18e78ac3e7e1490f6fc4937fd062
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858448"
---
# <a name="active-directory-forest-recovery-virtualization"></a>Active Directory 林恢复虚拟化

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

本主题介绍 Windows Server 2016、 2012 R2 和 2012年中的虚拟化的域控制器克隆功能。  

## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>使用虚拟化的域控制器克隆以加快林恢复

虚拟化的域控制器 (DC) 克隆可简化并加快了安装在域中，尤其是在其中的虚拟机监控程序运行多个 Dc 的集中位置例如数据中心的其他虚拟化的 Dc 的过程。 从备份还原每个域中的一个虚拟 DC 后，每个域中的其他 Dc 可以快速处于联机状态通过使用虚拟化的 DC 克隆过程。 可以准备的第一个虚拟化的 DC 的恢复，其关闭，并复制该虚拟硬盘无数次，以便为必须创建克隆虚拟化 Dc 建立域。  
  
虚拟化 DC 克隆的要求如下：  
  
- 虚拟机监控程序必须支持 VM 生成 Id。 Windows Server 2016、 2012年中的 HYPER-V 和 Windows 8 是虚拟机监控程序支持 VM 生成 Id 的示例。 如果支持 VM 生成 Id，请与你的虚拟机监控程序的供应商。  
- 用作源来克隆虚拟化的 DC 必须运行 Windows Server 2016 或 2012年，并且是可克隆的域控制器组的成员。 
- PDC 仿真器必须运行 Windows Server 2016 或 2012年。 如果它虚拟化，则可以克隆 PDC 仿真器。  
  
有关分步说明有关如何执行虚拟化 DC 克隆，请参阅[Active Directory 域服务 (AD DS) 虚拟化 (级别 100) 简介](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)。 详细了解如何虚拟化 DC 克隆的工作原理，请参阅[虚拟化域控制器技术参考 (级别 300)](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md)。 

## <a name="next-steps"></a>后续步骤

- [AD 林恢复的系统必备组件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 林恢复-设计出一个自定义林恢复计划](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 林恢复-识别问题](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)  
- [AD 林恢复-方面的常见问题](AD-Forest-Recovery-FAQ.md)  
- [AD 林恢复-恢复单个的多域林中域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 林恢复-与 Windows Server 2003 域控制器的林恢复](AD-Forest-Recovery-Windows-Server-2003.md) 
