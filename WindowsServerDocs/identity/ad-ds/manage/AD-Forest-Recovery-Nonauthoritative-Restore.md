---
title: AD 林恢复-未经授权还原
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.technology: identity-adds
ms.openlocfilehash: eae4cab2bd709097fe0efd0745baeb0ec685abc7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829608"
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>执行 Active Directory 域服务非权威还原 

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

若要执行非权威还原，请完成以下过程。  
  
以下过程使用 Wbadmin.exe 来执行非权威还原的 Active Directory 或 Active Directory 域服务 (AD DS)。 如果使用不同的备份解决方案，或如果你想要完成权威 SYSVOL 还原林恢复过程中更高版本，您可以通过使用这些备选方法执行 SYSVOL 的权威还原：  
  
- 如果使用文件复制服务 (FRS) 来复制 SYSVOL，请按照中的步骤[一文 290762](https://go.microsoft.com/fwlink/?LinkId=148443)在 Microsoft 知识库文章中，使用**BurFlags**注册表项以重新初始化 FRS 副本设置，或如有必要，文章 315457 [315457](https://support.microsoft.com/kb/315457)重新生成 SYSVOL 树。 若要确定是否由 FRS 复制 SYSVOL，请参阅[确定是否在域控制器的 SYSVOL 文件夹复制 DFSR 或 FRS](https://msdn.microsoft.com/en-us/library/windows/desktop/cc507518.aspx#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs)。  
- 如果使用分布式文件系统 (DFS) 复制来复制 SYSVOL，请参阅[执行 DFSR 复制的 SYSVOL 的权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。  

## <a name="performing-a-nonauthoritative-restore"></a>执行非权威还原

使用以下过程在运行 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 DC 上使用 wbadmin.exe 执行 AD DS 的非权威还原和 SYSVOL 的权威还原，在同一时间。 备份必须显式包括系统状态数据;用于整个服务器恢复的完整服务器备份将无法工作。 有关创建系统状态备份的详细信息，请参阅[备份系统状态数据](AD-Forest-Recovery-Backing-up-System-State.md)。  
  
### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>若要执行使用 wbadmin.exe 的 SYSVOL 的非权威还原的 AD DS 和权威还原  
  
- 包括**的方法是将 authsysvol**切换在恢复命令中，在下面的示例所示：  

   ```  
   wbadmin start systemstaterecovery <otheroptions> -authsysvol  
   ```  

   例如：  

   ```  
   wbadmin start systemstaterecovery -version:11/20/2012-13:00 -authsysvol  
   ```  
  
![还原](media/AD-Forest-Recovery-Nonauthoritative-Restore/nonauth.png)

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)
