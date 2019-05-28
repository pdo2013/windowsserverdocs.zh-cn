---
ms.assetid: 9aaca9c5-ce44-495c-aad6-61aede87a83f
title: 在帐户伙伴组织中部署 AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bf21860603b3055c2ef2c9e7b77bb106eb06e238
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191619"
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>在帐户伙伴组织中部署 AD FS

在 Active Directory 联合身份验证服务帐户伙伴\(AD FS\)表示组织中受支持的属性存储中以物理方式存储用户帐户的联合身份验证信任关系。 有关支持的属性存储的详细信息，请参阅[The Role of Attribute Stores](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)。  
  
帐户伙伴组织中的联合身份验证服务器进行身份验证本地用户，并创建由资源伙伴在做出授权决定时使用的安全令牌。 例如网站和 Web 服务的信赖方就能轻松地向联合身份验证服务器进行自行注册和使用颁发的令牌进行身份验证和访问控制。  
  
情况，需要为用户提供有权访问多个联合应用程序或服务中-当每个应用程序或服务由不同组织承载 — 你可以配置帐户伙伴联合身份验证服务器，以便可以部署多个信赖方。  
  
有关如何设置和配置帐户伙伴组织的详细信息，请参阅[核对清单：配置帐户伙伴组织](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md)。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [查看联合服务器在帐户伙伴中的角色](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [查看联合服务器代理在帐户伙伴中的角色](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [准备帐户伙伴中的客户端计算机](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
