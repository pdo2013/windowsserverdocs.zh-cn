---
title: "广告林恢复-找出问题"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 1e9ede12dade0b5f1149eae784620520a8866e30
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="identify-the-problem"></a>标识该问题

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2
  
 当出现树林失败的症状时，如在事件日志或监视的其他解决方案，适用于 Microsoft 支持，以确定失败的原因和评估任何解决办法。  
 
## <a name="examples-of-forest-wide-failures"></a>树林失败情况的示例 
  
-   所有 Dc 已逻辑已损坏或企业连续性是不可能; 的某个点到机身损坏例如，所有取决于广告 DS 的业务应用将无法正常工作。  
  
-   恶意管理员已威胁 Active Directory 环境。  
  
-   攻击有意-或管理员意外 — 运行林中传播数据损坏的脚本。  
  
-   攻击有意-或管理员意外 — 扩展 Active Directory 架构恶意或冲突变化。  
  
-   攻击者已管理上 Dc，安装恶意软件，并且你已被告知 Microsoft 支持部门恢复森林从备份。  
  
    > [!IMPORTANT]
    >  本文中不包括安全有关如何恢复已被攻击或威胁森林的建议。 一般情况下，建议到按照 Pass--哈希缓解技术来加强环境。 有关详细信息，请参阅[Mitigating Pass--哈希 (PtH) 攻击和其他凭据盗窃技术](https://www.microsoft.com/download/details.aspx?id=36036)。  
  
-   无 Dc 可以与他们复制的合作伙伴进行复制。  
  
-   在任何域控制器广告 DS 无法进行更改。  
  
-   任何域中，无法安装新的域控制器。  
  
## <a name="next-steps"></a>后续步骤
-   [广告森林恢复-先决条件](AD-Forest-Recovery-Prerequisties.md)  
-   [广告森林恢复-在寻找自定义森林恢复套餐](AD-Forest-Recovery-Devising-a-Plan.md)  
- [广告森林恢复-找出问题](AD-Forest-Recovery-Identify-the-Problem.md)
-   [广告森林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [广告森林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)  
-   [广告森林恢复-常见问题](AD-Forest-Recovery-FAQ.md)  
-   [广告森林恢复-恢复单个域中多域森林](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [广告森林恢复-森林恢复与 Windows Server 2003 该域控制器](AD-Forest-Recovery-Windows-Server-2003.md) 
