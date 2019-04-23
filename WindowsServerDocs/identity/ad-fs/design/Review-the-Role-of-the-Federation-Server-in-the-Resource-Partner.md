---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: 查看联合服务器在资源伙伴中的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d4c7763d7204dd2340d10a234e48f47c96788dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841838"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>查看联合服务器在资源伙伴中的角色

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

资源伙伴组织中的联合身份验证服务器截获帐户联合身份验证服务器发送的传入安全令牌、 验证和对它们进行签名，然后颁发其自己安全令牌与发往 Web\-基于应用程序。  
  
> [!NOTE]  
> 当联合的用户使用其 Web 浏览器访问 Web\-基于应用程序，资源伙伴组织中的联合身份验证服务器生成一个新的身份验证 cookie 并将其写入到浏览器。 此 cookie 将启用单一\-符号\-上\(SSO\)功能，以便用户无需再次登录在帐户伙伴中联合身份验证服务器时在用户尝试访问另一个 Web\-基于资源伙伴中的应用程序。  
  
在 Web SSO 设计中，必须在外围网络中安装至少一台联合服务器。 在联合 Web SSO 设计中，必须安装在帐户伙伴组织的企业网络中的至少一台联合服务器和安装在资源伙伴组织的企业网络中的至少一台联合服务器。  
  
> [!NOTE]  
> 可以设置资源伙伴组织中的联合身份验证服务器计算机之前，您必须首先将计算机加入到资源伙伴组织中任何 Active Directory 域中。 有关详细信息，请参阅[核对清单：设置联合身份验证服务器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

