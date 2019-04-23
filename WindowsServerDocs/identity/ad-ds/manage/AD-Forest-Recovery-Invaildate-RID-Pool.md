---
title: AD 林恢复-RID 池设为无效
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adds
ms.openlocfilehash: 46115991e48da301a8a739009bac27415ebe73df
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842508"
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>AD 林恢复-当前 RID 池设为无效  

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

使用以下过程对我们的 Windows PowerShell 以使域控制器上的当前 RID 池失效。 默认情况下，Windows Server 2012 和 Windows Server 2008 R2，但它必须安装使用的不 Windows Server 2008 上启用 Windows PowerShell**添加功能**。 它可以是[下载](https://www.microsoft.com/download/details.aspx?id=20020)若要在 Windows Server 2003 上运行。  

若要验证已成功完成该命令，检查事件 ID 16654 （来源为目录服务 SAM） 在 Windows Server 2012 中的事件查看器系统日志中。 早期版本的 Windows 不记录此事件。  
  
> [!NOTE]
> 您使 RID 池失效后，将收到错误，当首次尝试创建安全主体 （用户、 计算机或组）。 尝试创建一个对象将触发新的 RID 池的请求。 重试该操作的成功，因为将分配给新的 RID 池。  
  
## <a name="to-invalidate-the-current-rid-pool"></a>若要使当前 RID 池失效  
  
- 打开提升的 Windows PowerShell 会话，运行以下命令并按 ENTER:  

   ```powershell
   $Domain = New-Object System.DirectoryServices.DirectoryEntry  
   $DomainSid = $Domain.objectSid  
   $RootDSE = New-Object System.DirectoryServices.DirectoryEntry("LDAP://RootDSE")  
   $RootDSE.UsePropertyCache = $false  
   $RootDSE.Put("invalidateRidPool", $DomainSid.Value)  
   $RootDSE.SetInfo()  
   ```  

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)
