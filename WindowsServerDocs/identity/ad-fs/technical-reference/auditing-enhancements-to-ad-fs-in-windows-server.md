---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: "在 Windows Server 2016 的广告 FS 审核增强功能"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3d622686a3cc34316f0cf5187839785195c2f104
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>在 Windows Server 2016 的广告 FS 审核增强功能

>适用于：Windows Server 2016

当前，广告有 Windows Server 2012 R2 的 FS 将生成一个请求和相关信息日志中的许多审核事件或者令牌发放活动缺少 （在某些版本的广告 FS） 或分布跨多个审核事件。 默认情况下广告 FS 审核事件均处于关闭状态由于其详述自然。  
    在 Windows Server 2016 的广告 FS 的发布，与审核已成为更流畅，并且不太详述。  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>在适用于 Windows Server 2016 的广告 FS 审核级别  
默认情况下，在 Windows Server 2016 的广告 FS 已启用基本审核。  在基本审核、 管理员将看到一个请求 5 一些或少一些的事件。  这将标记显著的事件管理员已查看、 以查看单个请求数降低。   可以提出审核级别，或降低使用 PowerShell cmdlt： 集 AdfsProperties AuditLevel。  下表说明了可审核级别。  
  
||||  
|-|-|-|  
|审核级别|PowerShell 语法|描述|  
|无|设置 AdfsProperties-AuditLevel 无|禁用审核，并且不将记录事件。|  
|基本 （默认）|设置 AdfsProperties-AuditLevel 基本|将一个请求登录最多 5 事件|  
|详细|设置 AdfsProperties-AuditLevel 详述|将记录的所有事件。  这将记录大量的每个请求的信息。|  
  
若要查看当前审核级别，你可以使用 PowerShell cmdlt： 获取 AdfsProperties。  
  
![审核增强功能](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
可以提出审核级别，或降低使用 PowerShell cmdlt： 集 AdfsProperties AuditLevel。  
  
![审核增强功能](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>类型的审核事件  
根据不同类型的处理的广告 FS 请求的不同类型的可以广告 FS 审核事件。 每种类型的审核事件具有特定与之关联的数据。  与系统请求 （包括取配置信息服务器来电），可以登录请求 （即令牌请求） 之间区分审核事件的类型。    
  下表介绍了基本的审核活动类型。  
  
||||  
|-|-|-|  
|审核事件类型|事件 ID|描述|  
|全新的凭据验证成功|1202|在新的凭据成功验证的联合身份验证服务请求。 这包括 WS 信任 WS 联合身份验证的 SAML P （生成 SSO 第一个链路） 和 OAuth 授权端点。|  
|全新的凭据验证错误|1203|请求联合身份验证服务的全新凭据验证失败了。 这包括 WS 信任 WS 进、 SAML P （生成 SSO 第一个链路） 和 OAuth 授权端点。|  
|标记成功的应用程序|1200|在安全令牌联合身份验证服务由成功发出请求。 对于 WS 联合身份验证，这会记录与 SSO 项目处理请求时 SAML-P。 （如 SSO cookie)。|  
|应用程序令牌失败|1201|在安全令牌发放失败联合身份验证服务请求。 对于 WS-联盟，SAML-P 与 SSO 项目处理请求时，这会记录。 （如 SSO cookie)。|  
|更改请求成功密码|1204|成功地处理的事务请求更改密码了联合身份验证服务。|  
|密码更改请求错误|1205|请求更改密码某笔交易未处理的联合身份验证服务。| 
|出成功登录|1206|描述成功注销请求。|  
|登录出失败|1207|描述失败的注销请求。|  

  


