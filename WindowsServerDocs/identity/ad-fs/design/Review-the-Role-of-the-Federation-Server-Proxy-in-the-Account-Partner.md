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
ms.openlocfilehash: eb0547ac1d84fffed31360f0ee510504510c24ce
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867666"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>查看联合服务器代理在帐户伙伴中的角色

Active Directory 联合身份验证服务\(ADFS\)中帐户伙伴组织的外围网络中联合服务器代理的主要角色是从登录的客户端计算机收集身份验证凭据通过 Internet 并将这些凭据传递给联合服务器（位于帐户伙伴组织的企业网络内）。 客户端计算机的帐户存储在帐户伙伴的属性存储中。  
  
联合服务器代理还可以在以下一个或多个角色中工作，具体取决于你如何配置它以满足帐户伙伴组织的需求：  
  
-   中继安全令牌—联合服务器向联合服务器代理颁发安全令牌，后者随后将令牌中继到客户端计算机。 安全令牌用于为该客户端计算机提供对特定信赖方的访问。  
  
-   收集凭据—联合服务器代理使用默认的客户端登录 Web 表单\(clientlogon.aspx\) ，通过基于窗体\-\-的身份验证来收集基于密码的凭据。 但是，你可以自定义此表单以接受其他受支持的身份验证类型， \(如\)安全套接字层 SSL 客户端身份验证。 有关如何自定义此页面的详细信息，请参阅自定义客户端登录和主页\(领域发现页面[http\/：\/\/go.microsoft.com\/fwlink？LinkId\=104275](https://go.microsoft.com/fwlink/?LinkId=104275)。\) 联合服务器代理不会通过 Windows 集成身份验证接受凭据。  
  
总之，帐户伙伴中的联合服务器代理作为客户端登录到位于公司网络中的联合服务器的代理。 联合服务器代理还有助于向发往信赖方的 Internet 客户端分发安全令牌。  
  
> [!CAUTION]  
> 若要在帐户伙伴 extranet 上公开联合服务器代理，可以通过 Internet 访问的任何人都可以访问客户端登录 Web 窗体。 这可能会使组织容易遭受一些基于密码\-的攻击，如字典式攻击或暴力攻击，这些攻击可能会对存储在企业 Active Directory 域中的用户帐户触发帐户锁定。服务\(AD DS\)。  
  

## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
