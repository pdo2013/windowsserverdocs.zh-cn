---
title: AD 林恢复 - 过程
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.technology: identity-adds
ms.openlocfilehash: da45e3b20c370a2a37b0eab31a78216434dd60be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827718"
---
# <a name="ad-forest-recovery---procedures"></a>AD 林恢复 - 过程

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

本部分包含与林恢复过程相关的过程。 过程是适用于 Windows Server 2016、 2012 R2，2012年，还适用于 Windows Server 2008 R2 和 2008 有一些细微区别。

包括对 Windows Server 2003 不同的步骤的过程中找到[与 Windows Server 2003 域控制器的林恢复](AD-Forest-Recovery-Windows-Server-2003.md)。  

下面是使用备份和还原域控制器和 Active Directory 中的过程的列表。

- [备份整个服务器](AD-Forest-Recovery-Backing-up-a-Full-Server.md)  
- [备份系统状态数据](AD-Forest-Recovery-Backing-up-System-State.md)  
- [执行整个服务器恢复](AD-Forest-Recovery-Perform-a-Full-Recovery.md)  
- [执行 DFSR 复制的 SYSVOL 的权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
- [执行 Active Directory 域服务非权威还原](AD-Forest-Recovery-Nonauthoritative-Restore.md)  

下列步骤说明了如何执行 SYSVOL 的权威还原，在同一时间。  

- [配置 DNS 服务器服务](AD-Forest-Recovery-Configure-DNS.md)  
- [删除全局编录](AD-Forest-Recovery-Remove-GC.md)  
- [引发可用 RID 池的值](AD-Forest-Recovery-Raise-RID-Pool.md)  
- [使当前 RID 池失效](AD-Forest-Recovery-Invaildate-RID-Pool.md)  
- [占用操作主机角色](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)  
- [还原后进行清理](AD-Forest-Recovery-Cleanup.md)
- [清理已删除的可写域控制器的元数据](AD-Forest-Recovery-Cleaning-Metadata.md)  
- [重置域控制器的计算机帐户的密码](AD-Forest-Recovery-Reset-Computer-Account-DC.md)  
- [重置 krbtgt 密码](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)  
- [重置信任密码一侧的信任关系](AD-Forest-Recovery-Reset-Trust.md)  
- [添加全局编录](AD-Forest-Recovery-Add-GC.md)  
- [资源，以验证复制正常工作](AD-Forest-Recovery-Verify-Replication.md)  

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
