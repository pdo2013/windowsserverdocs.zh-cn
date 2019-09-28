---
title: AD 林恢复 - 过程
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.technology: identity-adds
ms.openlocfilehash: 0d427448c8d2a6616b87a524bcc941fc8555cbd4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390309"
---
# <a name="ad-forest-recovery---procedures"></a>AD 林恢复 - 过程

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

本部分包含与林恢复过程相关的过程。 这些过程适用于 Windows Server 2016、2012 R2、2012，还适用于 Windows Server 2008 R2 和2008，但有一些细微的例外。

在[带有 windows server 2003 域控制器的林恢复](AD-Forest-Recovery-Windows-Server-2003.md)中发现包含不同于 windows server 2003 的步骤的过程。  

下面是用于备份和还原域控制器和 Active Directory 的过程列表。

- [备份整个服务器](AD-Forest-Recovery-Backing-up-a-Full-Server.md)  
- [备份系统状态数据](AD-Forest-Recovery-Backing-up-System-State.md)  
- [执行完整服务器恢复](AD-Forest-Recovery-Perform-a-Full-Recovery.md)  
- [执行 DFSR 复制的 SYSVOL 的权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
- [执行 Active Directory 域服务的非权威还原](AD-Forest-Recovery-Nonauthoritative-Restore.md)  

以下步骤说明了如何同时执行 SYSVOL 的权威还原。  

- [配置 DNS 服务器服务](AD-Forest-Recovery-Configure-DNS.md)  
- [删除全局编录](AD-Forest-Recovery-Remove-GC.md)  
- [提高可用 RID 池的值](AD-Forest-Recovery-Raise-RID-Pool.md)  
- [使当前 RID 池失效](AD-Forest-Recovery-Invaildate-RID-Pool.md)  
- [占用操作主机角色](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)  
- [还原后清理](AD-Forest-Recovery-Cleanup.md)
- [清除已删除的可写域控制器的元数据](AD-Forest-Recovery-Cleaning-Metadata.md)  
- [正在重置域控制器的计算机帐户密码](AD-Forest-Recovery-Reset-Computer-Account-DC.md)  
- [正在重置 krbtgt 密码](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)  
- [重置信任一方的信任密码](AD-Forest-Recovery-Reset-Trust.md)  
- [添加全局编录](AD-Forest-Recovery-Add-GC.md)  
- [用于验证复制的资源正在运行](AD-Forest-Recovery-Verify-Replication.md)  

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
