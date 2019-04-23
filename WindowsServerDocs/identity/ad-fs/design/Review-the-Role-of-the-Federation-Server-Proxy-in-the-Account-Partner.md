---
ms.assetid: 1b3a03c0-5558-4177-9b2f-e9d6ce3271cd
title: 查看联合服务器代理在帐户伙伴中的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d2b60ce593c2ca7eb902595ee6a42850cb7605d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870838"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>查看联合服务器代理在帐户伙伴中的角色

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在 Active Directory 联合身份验证服务在帐户伙伴组织的外围网络中的联合身份验证服务器代理的主要作用\(AD FS\)是从登录的客户端计算机收集身份验证凭据通过 Internet 以及将这些凭据传递给联合身份验证服务器，该按钮位于帐户伙伴组织在企业网络内部。 客户端计算机的帐户存储在帐户伙伴的属性存储中。  
  
联合服务器代理还可以既在一个或多个以下角色，具体取决于如何配置它以满足帐户伙伴组织的需求：  
  
-   中继安全令牌-联合身份验证服务器到联合身份验证服务器代理，然后将令牌中继到客户端计算机颁发安全令牌。 安全令牌用于为该客户端计算机提供对特定信赖方的访问。  
  
-   收集凭据 — 联合服务器代理使用默认客户端登录 Web 表单\(clientlogon.aspx\)收集密码\-基于窗体通过凭据\-基于身份验证。 但是，自定义此表单以接受其他受支持的类型的身份验证，如安全套接字层\(SSL\)客户端身份验证。 有关如何自定义此页面的详细信息，请参阅自定义客户端登录和主页领域发现页面\( [http:\/\/go.microsoft.com\/fwlink\/？LinkId\=104275](https://go.microsoft.com/fwlink/?LinkId=104275)\)。 联合服务器代理不接受通过 Windows 集成身份验证的凭据。  
  
总之，帐户伙伴中的联合身份验证服务器代理充当客户端登录到位于公司网络中的联合身份验证服务器的代理。 联合身份验证服务器代理还帮助向目标为信赖方的 Internet 客户端的安全令牌的分布。  
  
> [!CAUTION]  
> 公开帐户伙伴 extranet 上的联合身份验证服务器代理将客户端登录 Web 窗体可访问与 Internet 的任何人访问。 这可能会使你的组织受到一些密码\-基于攻击，如字典式攻击或暴力破解攻击，可以触发对存储在公司的 Active Directory 域中的用户帐户的帐户锁定服务\(AD DS\)。  
  

## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
