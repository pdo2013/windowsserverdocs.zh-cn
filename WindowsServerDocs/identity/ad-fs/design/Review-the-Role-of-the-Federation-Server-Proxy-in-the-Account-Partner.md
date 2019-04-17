---
ms.assetid: 1b3a03c0-5558-4177-9b2f-e9d6ce3271cd
title: "查看联合服务器代理伙伴帐户中的角色"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d2b60ce593c2ca7eb902595ee6a42850cb7605d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>查看联合服务器代理伙伴帐户中的角色

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

联合 server 代理外围中 Active Directory 联合身份验证服务 \(AD FS\) 帐户合作伙伴公司的网络中的主要角色是用于收集来自客户端计算机登录 Internet 上的身份验证凭据并将这些凭据传递到联盟服务器，则位于公司帐户合作伙伴公司的网络。 客户端计算机的帐户将存储在帐户合作伙伴特性应用商店中。  
  
在一个或多个以下角色，具体取决于你如何配置以满足帐户合作伙伴公司的需求，还可正常联合身份验证的服务器代理：  
  
-   中继安全标记，联合服务器到联盟服务器代理服务器，然后将继向客户端计算机令牌问题安全标记。 安全令牌用于为特定信赖方提供的客户端计算机的访问权限。  
  
-   收集的凭据，联合身份验证的服务器代理使用默认客户端登录 Web 表单 \(clientlogon.aspx\) 收集 password\ 基于通过 forms\ 基于身份验证的凭据。 但是，你可以自定义此窗体接受其他受支持的身份验证，如安全套接字层 \(SSL\) 客户端身份验证类型。 有关如何自定义此页面的详细信息，请参阅自定义的客户端登录和家庭领域发现页面 \ ([http:///\/go.microsoft.com\/fwlink\/？LinkId\ = 104275](https://go.microsoft.com/fwlink/?LinkId=104275)\)。 联合身份验证的服务器代理不接受通过 Windows 的集成身份验证的凭据。  
  
总结，在帐户合作伙伴联合服务器代理充当的代理客户端登录到位于公司的网络的联合身份验证的服务器。 联合身份验证的服务器代理还有助于到 Internet 的客户端信赖方发给安全令牌 distribution。  
  
> [!CAUTION]  
> 公开联盟服务器代理服务器上的帐户合作伙伴外部网络将客户端登录 Web 表单可以访问 Internet 的任何人访问。 这可能会使你的组织易受某些基于 password\ 攻击，如字典攻击或可以触发存储在公司的 Active Directory 域服务 \(AD DS\) 的用户帐户的帐户锁定暴力攻击。  
  

## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
