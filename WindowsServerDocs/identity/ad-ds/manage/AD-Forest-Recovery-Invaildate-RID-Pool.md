---
title: AD 林恢复-使 RID 池失效
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adds
ms.openlocfilehash: c3c477e21a455e5e5777da00b064ca7a02672571
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390408"
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>AD 林恢复-使当前 RID 池失效  

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

使用以下过程向我们的 Windows PowerShell 使域控制器上的当前 RID 池失效。 默认情况下，windows PowerShell 在 Windows Server 2012 和 Windows Server 2008 R2 上处于启用状态，但 windows Server 2008 却不能通过使用 "**添加功能**" 安装。 可以将其[下载](https://www.microsoft.com/download/details.aspx?id=20020)到在 Windows Server 2003 上运行。  

若要验证命令是否已成功完成，请在 Windows Server 2012 事件查看器中检查系统日志中的事件 ID 16654 （源为目录服务-SAM）。 Windows 的早期版本不会记录此事件。  
  
> [!NOTE]
> 使 RID 池无效后，在第一次尝试创建安全主体（用户、计算机或组）时，你将收到一条错误消息。 尝试创建对象会触发对新 RID 池的请求。 重试操作成功，因为将分配新的 RID 池。  
  
## <a name="to-invalidate-the-current-rid-pool"></a>使当前 RID 池无效  
  
- 打开提升的 Windows PowerShell 会话，运行以下命令并按 ENTER：  

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
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)
