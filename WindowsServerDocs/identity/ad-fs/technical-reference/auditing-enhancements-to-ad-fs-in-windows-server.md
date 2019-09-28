---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: 在 Windows Server 2016 中审核 AD FS 的增强功能
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4eb93513d12b2bba2620ff16be24f62ace5dee85
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407253"
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>在 Windows Server 2016 中审核 AD FS 的增强功能


目前，在 Windows Server 2012 R2 AD FS 中，为单个请求生成了很多审核事件，有关登录或令牌颁发活动的相关信息可能不存在（在某些版本的 AD FS 中）或分布于多个审核事件。 默认情况下，AD FS 审核事件处于关闭状态，原因是它们的详细特性。  
    随着 Windows Server 2016 中的 AD FS 的发布，审核变得越来越简单，更不详细。  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>Windows Server 2016 的 AD FS 审核级别  
默认情况下，Windows Server 2016 中的 AD FS 启用了基本审核。  使用基本审核时, 管理员将看到单个请求的5个或更少事件。  这会使管理员必须查看的事件数明显减少, 以便查看单个请求。   使用 PowerShell cmdlt 可以提高或降低审核级别:Set-adfsproperties-AuditLevel。  下表说明了可用的审核级别。  
  
||||  
|-|-|-|  
|审核级别|PowerShell 语法|描述|  
|无|Set-adfsproperties-AuditLevel None|审核处于禁用状态, 并且不会记录任何事件。|  
|Basic (默认值)|Set-adfsproperties-AuditLevel Basic|对于单个请求, 将只记录5个以上的事件|  
|Verbose|Set-adfsproperties-AuditLevel Verbose|将记录所有事件。  这会为每个请求记录大量的信息。|  
  
若要查看当前审核级别, 可以使用 PowerShell cmdlt:Set-adfsproperties。  
  
![审核增强功能](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
使用 PowerShell cmdlt 可以提高或降低审核级别:Set-adfsproperties-AuditLevel。  
  
![审核增强功能](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>审核事件的类型  
AD FS 审核事件的类型可以不同，具体取决于 AD FS 处理的不同类型的请求。 每种类型的审核事件都具有与之关联的特定数据。  审核事件的类型可以在登录请求（即令牌请求）与系统请求（包括提取配置信息的服务器调用）之间进行区分。    
  下表描述了审核事件的基本类型。  
  
||||  
|-|-|-|  
|审核事件类型|事件 ID|描述|  
|全新凭据验证成功|1202|联合身份验证服务成功验证新凭据的请求。 这包括 WS-TRUST、WS 联合身份验证、SAML-P (生成 SSO 的第一个阶段) 和 OAuth 授权终结点。|  
|全新凭据验证错误|1203|联合身份验证服务上的新凭据验证失败的请求。 这包括 WS-TRUST、WS-FEDERATION、SAML-P (生成 SSO 的第一个阶段) 和 OAuth 授权终结点。|  
|应用程序令牌成功|1200|联合身份验证服务成功颁发安全令牌的请求。 对于 WS-FEDERATION, 在通过 SSO 项目处理请求时, 将记录 SAML P。 (如 SSO cookie)。|  
|应用程序令牌失败|1201|联合身份验证服务上的安全令牌颁发失败的请求。 对于 WS-FEDERATION, 在通过 SSO 项目处理请求时, 将记录 SAML P。 (如 SSO cookie)。|  
|密码更改请求成功|1204|联合身份验证服务已成功处理密码更改请求的事务。|  
|密码更改请求错误|1205|联合身份验证服务无法处理密码更改请求的事务。| 
|注销成功|1206|描述成功的注销请求。|  
|注销失败|1207|描述失败的注销请求。|  

  


