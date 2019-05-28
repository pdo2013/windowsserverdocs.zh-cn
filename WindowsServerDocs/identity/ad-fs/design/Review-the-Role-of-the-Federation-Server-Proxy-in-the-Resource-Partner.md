---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: 查看联合服务器代理在资源伙伴中的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 377baa8f282f3886284a53b686944fe145b1b15e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190890"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>查看联合服务器代理在资源伙伴中的角色

Active Directory 联合身份验证服务中的联合身份验证服务器代理\(AD FS\)可以在一个或多个以下角色，具体取决于如何配置服务器以满足资源伙伴组织中的函数：  
  
-   **帐户伙伴发现**：Internet 客户端计算机必须确定将对其进行身份验证的帐户伙伴。 客户端查找帐户伙伴使用帐户伙伴发现 Web 窗体\(discoverclientrealm.aspx\)，这存储在资源伙伴联合身份验证服务器代理上。 如果配置多个帐户伙伴中 AD FS 管理管理单元\-中，放置\-菜单下显示给客户端的对访问帐户伙伴的 Internet 客户端计算机可见的所有可用帐户伙伴发现 Web 窗体。 你可以通过自定义 discoverclientrealm.aspx 文件来更改帐户伙伴发现 Web 窗体对客户端计算机的呈现方式。  
  
-   **安全令牌重定向**：帐户伙伴中的联合身份验证服务器代理将安全令牌发送给资源伙伴。 资源联合服务器代理接受这些令牌，并将它们传递到资源伙伴中的联合身份验证服务器。 然后，资源联合服务器颁发被绑定到特定资源 Web 服务器的安全令牌。 资源联合服务器代理将重定向到客户端的令牌。  
  
总之，资源联合服务器代理通过将客户端计算机重定向到联合身份验证服务器可以进行身份验证客户端简化了联合身份验证的登录过程。 资源联合服务器代理还充当资源联合身份验证服务的客户端安全令牌的代理。  
  
> [!NOTE]  
> 需要以帮助减少硬件和所需证书的数目时，联合服务器代理可以位于与 Web 服务器在同一台计算机上。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

