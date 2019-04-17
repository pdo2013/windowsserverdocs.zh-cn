---
title: "广告森林恢复-失效 RID 池"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adfs
ms.openlocfilehash: cb024356ae5f872e93448d73ea54b271fe3fae4d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>广告森林恢复-失效当前 RID 池  

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 使用以下步骤向我们的 Windows PowerShell 导致失效域控制器上的当前 RID 池。 默认情况下在 Windows Server 2012 和 Windows Server 2008 R2，但不是 Windows Server 2008 了必须安装它使用启用了 Windows PowerShell**添加功能**。 它可以是[下载](https://www.microsoft.com/download/details.aspx?id=20020)在 Windows Server 2003 上运行。  
  
 若要验证是否已成功完成命令，检查事件 ID 16654（源是目录-服务三千）中的 Windows Server 2012 事件查看器在系统日志中。 早期版本的 Windows 不会记录该事件。  
  
> [!NOTE]
>  使失效 RID 池后，你将收到错误，当你首次尝试创建安全主要（用户、的电脑或组）。 尝试创建一个触发请求 RID 新池。 该操作的重试成功，因为将分配 RID 新池。  
  
## <a name="to-invalidate-the-current-rid-pool"></a>若要使无效当前不用池  
  
1.  打开提升了权限的 Windows PowerShell 会话，运行以下命令，然后按 ENTER:  
  
    ```  
    $Domain = New-Object System.DirectoryServices.DirectoryEntry  
    $DomainSid = $Domain.objectSid  
    $RootDSE = New-Object System.DirectoryServices.DirectoryEntry("LDAP://RootDSE")  
    $RootDSE.UsePropertyCache = $false  
    $RootDSE.Put("invalidateRidPool", $DomainSid.Value)  
    $RootDSE.SetInfo()  
    ```  
  
## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)
