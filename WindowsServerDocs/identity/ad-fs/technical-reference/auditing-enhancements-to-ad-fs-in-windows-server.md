---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: 在 Windows Server 2016 中审核 AD FS 的增强功能
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3d622686a3cc34316f0cf5187839785195c2f104
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880228"
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>在 Windows Server 2016 中审核 AD FS 的增强功能

>适用于：Windows Server 2016

目前，AD FS 的 Windows Server 2012 R2 有是生成用于单个请求与日志中的相关信息的大量审核事件或令牌颁发活动是不存在 （在某些版本的 AD FS） 或跨多个审核事件。 默认情况下，AD FS 审核事件处于关闭状态由于其详细的特性。  
    Windows Server 2016 中的 AD FS 的版本中，审核变得更简单和不太详细。  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>AD FS 的 Windows Server 2016 中的审核级别  
默认情况下，Windows Server 2016 中的 AD FS 具有基本审核已启用。  使用基本审核，管理员将看到针对单个请求的 5 个事件。  这会标记显著缩短管理员具有查看，若要查看单个请求的事件数。   审核级别可以引发或降低使用 PowerShell 脚本：Set-AdfsProperties -AuditLevel.  下表介绍了可用的审核级别。  
  
||||  
|-|-|-|  
|审核级别|PowerShell 语法|描述|  
|无|Set-adfsproperties-AuditLevel None|禁用审核并将记录任何事件。|  
|基本 （默认值）|Set-AdfsProperties - AuditLevel Basic|不能超过 5 个事件将记录的单个请求|  
|Verbose|Set-adfsproperties-AuditLevel 详细|将记录所有事件。  这将记录大量的每个请求的信息。|  
  
若要查看当前的审核级别，可以使用 PowerShell 脚本：Get-AdfsProperties.  
  
![审核增强功能](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
审核级别可以引发或降低使用 PowerShell 脚本：Set-AdfsProperties -AuditLevel.  
  
![审核增强功能](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>类型的审核事件  
AD FS 审核事件可以是基于处理的 AD FS 请求不同类型的不同类型。 每种类型的审核事件具有与之关联的特定数据。  与系统的请求 （包括提取配置信息的服务器调用），可以登录请求 （即令牌请求） 之间区别的审核事件的类型。    
  下表描述了基本类型的审核事件。  
  
||||  
|-|-|-|  
|审核事件类型|事件 ID|描述|  
|新凭据验证成功|1202|其中最新的凭据通过联合身份验证服务已成功验证请求。 这包括 Ws-trust、 WS 联合身份验证 SAML-P （若要生成 SSO 第一个阶段） 和 OAuth 授权终结点。|  
|新凭据验证错误|1203|新凭据验证联合身份验证服务的失败的请求。 这包括 Ws-trust、 WS 联合身份验证、 SAML-P （若要生成 SSO 第一个阶段） 和 OAuth 授权终结点。|  
|应用程序令牌成功|1200|其中的安全令牌的联合身份验证服务已成功发出的请求。 对于 WS 联合身份验证，SAML-P SSO 项目与处理请求时，记录此。 （例如 SSO cookie)。|  
|应用程序令牌失败|1201|安全令牌颁发联合身份验证服务的失败的请求。 对于 WS 联合身份验证，SAML-P SSO 项目与处理请求时，记录此。 （例如 SSO cookie)。|  
|密码更改请求成功|1204|联合身份验证服务已成功处理密码更改请求的其中一个事务。|  
|密码更改请求错误|1205|密码更改请求的其中一个事务失败由联合身份验证服务来处理。| 
|注销成功|1206|描述成功注销请求。|  
|注销失败|1207|描述失败的注销请求。|  

  


