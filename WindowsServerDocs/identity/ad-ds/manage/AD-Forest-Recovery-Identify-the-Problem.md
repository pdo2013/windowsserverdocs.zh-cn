---
title: AD 林恢复 - 标识问题
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: cb3cf45f778ff305c8fe5aad5e23f0257650d6f5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865518"
---
# <a name="identify-the-problem"></a>确定此问题

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2
  
当出现全林性失败的症状时，如在事件日志或其他监视解决方案中，在 Microsoft 支持部门以确定失败的原因，并评估任何可能的补救措施。  

## <a name="examples-of-forest-wide-failures"></a>林范围内发生故障的示例

- 已以逻辑方式损坏或物理损坏到点业务连续性是不可能的; 所有域控制器例如，依赖于 AD DS 中的所有业务应用程序都都无法正常运行。  
- 心怀恶意的管理员已破坏的 Active Directory 环境。  
- 攻击者有意 — 或管理员意外 — 运行脚本，用于在林之间分布数据损坏。  
- 攻击者有意 — 或管理员意外，扩展了 Active Directory 架构的恶意或有冲突的更改。  
- 攻击者具有管理上域控制器，以安装恶意软件，您已被告知 Microsoft 支持从备份恢复林。  
  
   > [!IMPORTANT]
   >  本文不涉及有关如何恢复已受到黑客攻击或泄露的林中的安全建议。 一般情况下，它被建议按照传递的哈希缓解措施来强化环境。 有关详细信息，请参阅[缓解传递的哈希 (PtH) 攻击和其他凭据盗窃技术](https://www.microsoft.com/download/details.aspx?id=36036)。
  
- 不在域控制器可以复制其复制伙伴。  
- 不能对在任何域控制器上的 AD DS 进行更改。  
- 不能在任何域中安装新域控制器。  
  
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
