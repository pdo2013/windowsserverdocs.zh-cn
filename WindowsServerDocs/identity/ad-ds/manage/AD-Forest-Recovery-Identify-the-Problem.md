---
title: AD 林恢复 - 标识问题
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: dddbd187fb100b94b505a74595e040cec580a797
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369116"
---
# <a name="identify-the-problem"></a>确定问题

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2
  
当出现林范围的故障时，例如在事件日志或其他监视解决方案中时，请使用 Microsoft 支持部门来确定失败的原因，并评估任何可能的补救措施。  

## <a name="examples-of-forest-wide-failures"></a>林范围的故障的示例

- 所有 Dc 都已逻辑损坏或物理损坏，无法实现业务连续性;例如，依赖于 AD DS 的所有业务应用程序都不能正常工作。  
- 恶意管理员已损坏 Active Directory 环境。  
- 攻击者有意或管理员意外地运行了跨林传播数据损坏的脚本。  
- 攻击者有意或管理员意外地扩展了 Active Directory 架构，并发生了恶意或发生冲突的更改。  
- 攻击者已设法在 Dc 上安装恶意软件，并已 Microsoft 支持部门从备份中恢复林。  
  
   > [!IMPORTANT]
   >  本文不介绍有关如何恢复受到黑客攻击的林的安全建议。 通常，建议遵循传递哈希缓解技术来强化环境。 有关详细信息，请参阅[缓解哈希传递（PtH）攻击和其他凭据盗窃技术](https://www.microsoft.com/download/details.aspx?id=36036)。
  
- 不能将任何 Dc 与其复制伙伴进行复制。  
- 不能对任何域控制器 AD DS 更改。  
- 新 Dc 不能安装在任何域中。  
  
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
