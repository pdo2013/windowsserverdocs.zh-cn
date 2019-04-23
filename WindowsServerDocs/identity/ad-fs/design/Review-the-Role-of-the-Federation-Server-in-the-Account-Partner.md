---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: 查看联合服务器在帐户伙伴中的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0914d32e8f24d5e7db0a25c733342c1bde3e0329
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835128"
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>查看联合服务器在帐户伙伴中的角色

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Active Directory 联合身份验证服务中的联合身份验证服务器\(AD FS\)充当安全令牌颁发者。 联合身份验证服务器生成基于帐户驻留在本地属性的值存储声明并将它们打包为安全令牌，以便用户可以无缝地访问 Web\-浏览器\-基于应用程序\(使用单一登录\-上\(SSO\) \)资源伙伴组织中承载。  
  
> [!NOTE]  
> 联合身份验证服务器时你的用户使用 Web 浏览器访问联合应用程序，向用户为该 Web 维护其登录状态的 cookie 会自动颁发\-浏览器\-基于应用程序。 这些 cookie 包括用于用户的声明。 Cookie 可实现 SSO 功能，以便用户无需凭据每次都输入他们访问另一个 Web\-浏览器\-基于资源伙伴中的应用程序。  
  
在 Web SSO 设计中，外围网络并且希望 Internet 用户才能访问应用程序的组织必须安装在外围网络中的联合身份验证服务器代理。 在联合 Web SSO 设计中，必须安装在帐户伙伴组织的企业网络中的至少一台联合服务器和安装在资源伙伴组织的企业网络中的至少一台联合服务器。  
  
> [!NOTE]  
> 可以将联合身份验证服务器计算机帐户伙伴组织中设置之前，您必须首先将计算机加入到其中的联合身份验证服务器将使用来自该林的用户进行身份验证在 Active Directory 林中任何域中。 有关详细信息，请参阅[核对清单：设置联合身份验证服务器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
