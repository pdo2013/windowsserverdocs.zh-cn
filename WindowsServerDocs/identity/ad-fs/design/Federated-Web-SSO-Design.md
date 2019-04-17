---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: "Web 联盟的 SSO 设计"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b85f49ac0556bf9b3542a23514d7fcbf82d2d88e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="federated-web-sso-design"></a>Web 联盟的 SSO 设计

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

联合 Web Single\-Sign\-On \(SSO\) 设计 Active Directory 联合身份验证服务 \(AD FS\) 中的涉及的跨多防火墙、外围网络和 name\ 分辨率服务器的安全通信，除了整个 Internet 路由基础结构。  
  
通常，两个组织同意创建联盟信任关系某个组织中允许用户在使用此设计 \ (帐户合作伙伴 organization\) 来访问 Web\ 基于应用程序或服务，其中广告 FS，在另一个组织都不会得到 \ (资源合作伙伴 organization\)。  
  
换言之，联盟信任关系是 business\ 级别协议或之间两个组织合作体现。 在下图所示，你可以联盟信任之间建立关系两个企业，这会导致 to\ end\ 端联盟方案。  
  
![联盟的 web sso](media/adfs2_FederatedWebSSODesign.gif)  
  
图中的 one\ 方式箭头指示方向联合身份验证的信任，这，如 Windows 信任的方向，始终指向森林帐户一侧。 这意味着身份验证到资源的合作伙伴公司帐户合作伙伴公司从的版本流。  
  
在此联合 Web SSO 设计，两台联合身份验证的服务器 \（一个 Fabrikam 在和其他中 Contoso\）将发送的用户帐户的身份验证请求中 Fabrikam 给 Web\ 基于应用程序或服务 Contoso。  
  
> [!NOTE]  
> 其他安全，你可以使用联盟服务器代理传达请求到联合身份验证的服务器所无法直接通过 Internet 访问。  
  
在此示例中，Fabrikam 是的身份或帐户，提供。 联合 Web SSO 设计的 Fabrikam 部分使用以下广告 FS 部署目标：  
  
-   [提供您 Active Directory 用户的访问权限的应用程序和其他公司的服务](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso 是资源提供商。 联合 Web SSO 设计的 Contoso 部分，可实现以下广告 FS 部署目标：  
  
-   [提供您的索赔识别的应用程序和服务中的用户另一个组织访问](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [为你识别索赔应用程序和服务提供你 Active Directory 用户访问](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
你可以使用计划，并将其部署联合 Web SSO 设计的详细的任务列表，请参阅[清单：实施联盟 Web SSO 设计](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
