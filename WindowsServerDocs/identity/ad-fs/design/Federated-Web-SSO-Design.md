---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: 联合 Web SSO 设计
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f7454279f234f65136b9fe6649a6e96ea53e5d51
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191506"
---
# <a name="federated-web-sso-design"></a>联合 Web SSO 设计

联合 Web 单一\-符号\-上\(SSO\) Active Directory 联合身份验证服务中的设计\(AD FS\)涉及的安全通信跨越多个防火墙的外围网络和名称\-解析服务器 — 除了整个 Internet 路由基础结构。  
  
当两个组织都同意创建联合身份验证信任关系，以便允许一个组织中的用户通常情况下，使用此设计\(帐户伙伴组织\)来访问 Web\-基于应用程序或服务这是 AD FS 保护的其他组织中\(资源伙伴组织\)。  
  
换而言之，联合身份验证信任关系是企业的具体表现\-级别协议或两个组织之间的合作关系。 下图中所示，可以建立两个企业，这会导致结束之间的联合身份验证信任关系\-到\-端联合身份验证方案。  
  
![联合的 web sso](media/adfs2_FederatedWebSSODesign.gif)  
  
一个\-方法在图中箭头表示联合身份验证的方向信任，这 — Windows 信任关系的方向-始终指向林的帐户端。 这意味着，身份验证将从帐户伙伴组织流向资源伙伴组织。  
  
在此联合 Web SSO 设计中，两个联合身份验证服务器\(一个在 Fabrikam 和 Contoso 中其他\)将从用户帐户的身份验证请求路由到 Web 的 Fabrikam 中\-基于应用程序或服务中为 Contoso。  
  
> [!NOTE]  
> 为了提高安全性，可以使用联合身份验证服务器代理将请求中继到无法直接从 Internet 访问的联合身份验证服务器。  
  
在此示例中，Fabrikam 是标识、帐户或提供程序。 联合 Web SSO 设计的 Fabrikam 部分使用以下 AD FS 部署目标：  
  
-   [为其他组织的应用程序和服务提供 Active Directory 用户访问权限](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso 是资源提供单元。 联合 Web SSO 设计的 Contoso 部分实现以下 AD FS 部署目标：  
  
-   [为另一个组织中的用户提供对声明感知应用程序和服务的访问权限](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [为声明感知应用程序和服务提供 Active Directory 用户访问权限](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
有关可用于计划和部署联合 Web SSO 设计的详细任务的列表，请参阅[核对清单：实现联合的 Web SSO 设计](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
