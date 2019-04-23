---
title: AD 林恢复-设计出 AD 林恢复计划
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 17381f30-55f2-4e00-977a-b701675fa4ff
ms.technology: identity-adds
ms.openlocfilehash: edfc874fe030c6394bc8bda95c49e61951e78f43
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881778"
---
# <a name="ad-forest-recovery---devising-an-ad-forest-recovery-plan"></a>AD 林恢复-设计出 AD 林恢复计划

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

具体取决于您的环境和业务要求，可能会或可能不需要执行本指南，以执行成功林恢复中所述的所有步骤。 考虑到本指南仅充当林恢复模板，非常重要设计适合自己的环境并满足业务需求的自定义林恢复计划。  
  
例如，在林恢复计划，应具有在目录林的详细的拓扑图。 映射应列出有关域控制器，如其名称、 其角色和备份状态和它们之间的信任关系的所有信息。 一个工具，可用于创建拓扑图，请参阅[Microsoft Active Directory 拓扑图表](https://www.microsoft.com/download/details.aspx?id=13380)。  
  
应实施林恢复计划每年至少一次。 此外，它是 Enterprise Admins 或 Domain Admins 组成员身份更改时执行林恢复演练一个好办法。 这有助于确保您的信息技术 (IT) 人员完全了解林恢复计划。

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
