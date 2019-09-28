---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: 查看联合服务器在资源伙伴中的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fd9f20eb7559f5862ee50bdd8364fa1604d3c1b6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358971"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>查看联合服务器在资源伙伴中的角色

资源伙伴组织中的联合服务器截获由帐户联合服务器发送的传入安全令牌，对其进行验证和签名，然后颁发其自己的安全令牌，该令牌将发送给 Web @ no__t-0based 应用程序.  
  
> [!NOTE]  
> 当联合用户使用他们的 Web 浏览器访问 Web @ no__t-0based 应用程序时，资源伙伴组织中的联合服务器将生成新的身份验证 cookie，并将其写入浏览器。 此 cookie 启用单个 @ no__t-0sign @ no__t-1on \(SSO @ no__t-3 功能，使用户在尝试访问资源中的不同 Web @ no__t 应用程序时无需再次登录帐户伙伴中的联合服务器partner.  
  
在 Web SSO 设计中，必须在外围网络中安装至少一台联合服务器。 在联合 Web SSO 设计中，至少必须在帐户伙伴组织的企业网络中安装至少一台联合服务器，并在资源伙伴组织的企业网络中安装至少一台联合服务器。  
  
> [!NOTE]  
> 必须先将计算机加入资源伙伴组织中的任何 Active Directory 域，然后才能在资源伙伴组织中设置联合服务器计算机。 有关详细信息，请参阅 [Checklist：设置联合服务器 @ no__t。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

