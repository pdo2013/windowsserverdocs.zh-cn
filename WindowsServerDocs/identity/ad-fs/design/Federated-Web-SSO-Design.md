---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: 联合 Web SSO 设计
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6a3e7eb6c42c8190da799c88c1e947e6aef1c29f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408101"
---
# <a name="federated-web-sso-design"></a>联合 Web SSO 设计

联合 Web Single @ no__t-0Sign @ no__t-1On \(SSO @ no__t Active Directory 联合身份验证服务 \(AD FS @ no__t-5 包含跨多个防火墙、外围网络和名称 @ no__t-6resolution 的安全通信服务器-除了整个 Internet 路由基础结构。  
  
通常情况下，如果两个组织同意创建联合信任关系，以允许一个组织中的用户 @no__t 0the 帐户伙伴组织 @ no__t 来访问受保护的 Web @ no__t 2based 应用程序或服务，则通常使用此设计AD FS，请在其他组织 \(the 资源伙伴组织 @ no__t。  
  
换句话说，联合身份验证信任关系是在两个组织之间建立企业 @ no__t-0level 协议或合作关系的体现。 如下图所示，你可以在两个企业之间建立联合身份验证信任关系，这将导致结束 @ no__t-0to @ no__t-1end federation 方案。  
  
![联合的 web sso](media/adfs2_FederatedWebSSODesign.gif)  
  
图中的一个 @ no__t-0way 箭头表示联合身份验证信任的方向（例如 Windows 信任的方向）始终指向林的帐户端。 这意味着，身份验证将从帐户伙伴组织流向资源伙伴组织。  
  
在此联合 Web SSO 设计中，两个联合服务器在 Fabrikam 中 @no__t 0one，另一个在 Contoso @ no__t 中从 Fabrikam 中的用户帐户到 Web @ no__t 2based 应用程序或 Contoso 中的服务的路由身份验证请求。  
  
> [!NOTE]  
> 为了进一步提高安全性，你可以使用联合服务器代理将请求中继到无法从 Internet 直接访问的联合服务器。  
  
在此示例中，Fabrikam 是标识、帐户或提供程序。 联合 Web SSO 设计的 Fabrikam 部分使用以下 AD FS 部署目标：  
  
-   [为其他组织的应用程序和服务提供 Active Directory 用户访问权限](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso 是资源提供单元。 联合 Web SSO 设计的 Contoso 部分实现了以下 AD FS 部署目标：  
  
-   [为另一个组织中的用户提供对声明感知应用程序和服务的访问权限](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [为声明感知应用程序和服务提供 Active Directory 用户访问权限](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
有关可用于计划和部署联合 Web SSO 设计的详细任务的列表，请参阅 [Checklist：实现联合 Web SSO 设计 @ no__t-0。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
