---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: "何时创建联合身份验证的服务器代理服务器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1f0253dfb5a690371dae1a2bfcb6b7520077d473
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="when-to-create-a-federation-server-proxy"></a>何时创建联合身份验证的服务器代理服务器

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在你的组织中创建联合身份验证的服务器代理添加额外的安全层 Active Directory 联合身份验证服务 \(AD FS\) 部署。 请考虑时你希望这样部署联盟服务器代理服务器在你的组织外围网络：  
  
-   防止外部客户端计算机直接访问联合身份验证的服务器。 通过部署联合 server 代理外围网络中的，您有效地找出你联合身份验证的服务器，以便他们可以访问仅通过登录到外部客户端计算机代表采取措施的联合 server 代理通过公司的网络的客户端计算机。 联合身份验证的服务器代理没有访问用于产生标记的专用键。 有关详细信息，请参阅[放置联合身份验证的服务器代理](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
-   提供了方便地区分来自于 Internet 而不是来自于您使用 Windows 的集成身份验证的企业网络用户的用户 sign\ 中的体验。 联合身份验证的服务器代理从 Internet 客户端计算机使用登录、注销和联合身份验证的服务器代理服务器上存储的身份提供商发现 \(homerealmdiscovery.aspx\) 页面收集凭据或主页领域详细信息。  
  
    相反，来自于不同的体验，公司网络遇到的客户端计算机基于联合身份验证的服务器的配置。 公司的网络联合服务器经常配置为 Windows 集成认证，它的企业网络上的用户提供了无缝 sign\ 体验。  
  
是否或资源合作伙伴公司帐户合作伙伴公司中放置联合身份验证的服务器代理取决于联合身份验证的服务器代理播放你的组织中的角色。 例如，当联合身份验证的服务器代理置于外围帐户合作伙伴的网络，它的作用是从浏览器的客户端收集用户身份信息。 联合身份验证的服务器代理置于外围资源合作伙伴的网络，它将继安全令牌资源联盟服务器请求，并且会产生响应由其帐户合作伙伴安全令牌组织的安全标记。  
  
有关详细信息，请参阅[查看联合服务器代理伙伴帐户中的角色](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)和[查看联合服务器代理资源伙伴中的角色](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
## <a name="how-to-create-a-federation-server-proxy"></a>如何创建联合身份验证的服务器代理服务器  
你可以创建联合 server 代理使用广告 FS 联合身份验证的代理配置向导或 Fsconfig.exe command\ 行工具。 有关如何执行此操作的说明，请参阅[将计算机配置为联合身份验证的服务器代理角色](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。  
  
有关如何设置所有先决条件部署联合身份验证的服务器代理必要的常规信息，请参阅[清单：设置向上联盟服务器代理](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
