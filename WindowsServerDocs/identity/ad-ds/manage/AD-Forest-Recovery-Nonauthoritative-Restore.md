---
title: "广告森林恢复-非授权还原"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.technology: identity-adfs
ms.openlocfilehash: 684672491a0574b12e9b117a8e3c9c1f0936f357
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>执行权威的 Active Directory 域服务还原 

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2
 
 若要执行非授权还原，完成以下过程。  
  
 以下过程使用 Wbadmin.exe 执行的 Active Directory 或 Active Directory 域服务 (广告 DS) 的未授权还原。 如果你使用的另一种备份解决方案，或者你打算权威的完全还原 SYSVOL 更高版本中森林恢复过程，你可以执行 SYSVOL 授权还原通过这些替代的方法：  
  
-   如果你使用文件复制服务 (FRS) 复制 SYSVOL，请按照[文章 290762](https://go.microsoft.com/fwlink/?LinkId=148443)中 Microsoft 知识库、使用**BurFlags**注册表项，可以重新初始化 FRS 副本集，或在必要情况下文章 315457 [315457](https://support.microsoft.com/kb/315457)要重新生成 SYSVOL 树。 若要确定是否由 FRS 复制 SYSVOL，请参阅[确定是否域控制器的 SYSVOL 文件夹复制通过 DFSR 或 FRS](https://msdn.microsoft.com/en-us/library/windows/desktop/cc507518.aspx#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs)。  
  
-   如果你使用分布式文件系统 (DFS) 复制复制 SYSVOL，请参阅[执行 DFSR 复制 SYSVOL 权威同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。  
  
 
## <a name="performing-a-nonauthoritative-restore"></a>执行权威还原  
 使用下面的过程的广告 DS 未授权还原和授权还原 SYSVOL 的同时通过使用来执行 wbadmin.exe 在运行 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 DC。 备份必须显式包含系统状态数据;用于完全服务器恢复完整服务器备份不起作用。 有关创建系统状态备份的详细信息，请参阅[数据备份系统状态](AD-Forest-Recovery-Backing-up-System-State.md)。  
  
### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>可以执行的广告 DS 未授权还原和授权使用 wbadmin.exe SYSVOL 还原  
  
-   包含**-authsysvol**切换恢复您的命令，在以下示例所示：  
  
    ```  
    wbadmin start systemstaterecovery <otheroptions> -authsysvol  
    ```  
  
     例如：  
  
    ```  
    wbadmin start systemstaterecovery -version:11/20/2012-13:00 -authsysvol  
    ```  
  
 ![还原](media/AD-Forest-Recovery-Nonauthoritative-Restore/nonauth.png)

## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)
