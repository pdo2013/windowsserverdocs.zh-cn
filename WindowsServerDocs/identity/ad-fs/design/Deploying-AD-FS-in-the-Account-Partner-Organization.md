---
ms.assetid: 8c3536b7-d091-4ee6-ad04-24713f070862
title: 在帐户伙伴组织中部署 AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 63c080904482814f9f62451e8e7cfa4862d19927
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359252"
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>在帐户伙伴组织中部署 AD FS

Active Directory 联合身份验证服务 \(AD FS @ no__t 中的帐户伙伴表示联合身份验证信任关系中以物理方式将用户帐户存储在受支持的属性存储中的组织。 有关支持的属性存储的详细信息，请参阅[属性存储的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)。  
  
帐户伙伴组织中的联合服务器对本地用户进行身份验证，并创建由资源伙伴在进行授权决策时使用的安全令牌。 然后，信赖方（如网站和 Web 服务）可以轻松地向联合服务器注册自己，并使用颁发的令牌进行身份验证和访问控制。  
  
在需要向用户提供对多个联合应用程序或服务的访问权限的情况下（当每个应用程序或服务由不同组织承载时），可以配置帐户伙伴联合服务器，以便可以部署多个信赖方。  
  
有关如何设置和配置帐户伙伴组织的详细信息，请参阅 [Checklist：配置帐户伙伴组织 @ no__t。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [查看联合服务器在帐户伙伴中的角色](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [查看联合服务器代理在帐户伙伴中的角色](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [在帐户伙伴中准备客户端计算机](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
