---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: 何时创建联合服务器代理
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b41c2194940c85e39e5a3724f747dd12c2544259
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190645"
---
# <a name="when-to-create-a-federation-server-proxy"></a>何时创建联合服务器代理

在你的组织中创建联合服务器代理将额外的安全层添加到 Active Directory 联合身份验证服务\(AD FS\)部署。 请考虑根据需要部署你组织的外围网络中的联合身份验证服务器代理：  
  
-   阻止外部客户端计算机直接访问联合身份验证服务器。 通过部署联合服务器代理在外围网络中的，可以有效地隔离你的联合身份验证服务器，以便它们可以访问仅由登录到联合服务器代理，代表通过公司网络的客户端计算机外部客户端计算机。 联合服务器代理不能访问用于生成令牌的私钥。 有关详细信息，请参阅 [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
-   提供的方便的方法来区分符号\-中来自 Internet 而不是来自企业网络使用 Windows 集成身份验证的用户的用户体验。 联合服务器代理的凭据或主页领域详细信息从计算机中收集 Internet 客户端通过使用登录、 注销和标识提供程序发现\(homerealmdiscovery.aspx\)存储在联合身份验证的页面服务器代理。  
  
    与此相反，来自企业网络遇到不同的体验，客户端计算机基于联合身份验证服务器的配置。 企业网络联合身份验证服务器通常配置为 Windows 集成身份验证，这提供了无缝登录\-在企业网络上的用户体验。  
  
联合服务器代理在组织中所扮演的角色取决于是否是帐户伙伴组织或资源伙伴组织中放置联合服务器代理。 例如，当联合服务器代理放在帐户伙伴的外围网络中时，其作用是从浏览器客户端收集用户凭据信息。 当联合服务器代理放置在资源伙伴的外围网络中时，中继安全令牌要求传递到资源联合身份验证服务器，并生成组织的安全令牌来响应由提供的安全令牌及其帐户伙伴。  
  
有关详细信息，请参阅[查看的帐户伙伴中联合服务器代理角色](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)和[查看的资源伙伴中联合服务器代理角色](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
## <a name="how-to-create-a-federation-server-proxy"></a>如何创建联合服务器代理  
可以创建联合服务器代理使用 AD FS 联合服务器代理配置向导或 Fsconfig.exe 命令\-行工具。 有关如何执行此操作的说明，请参阅[将计算机配置为联合服务器代理角色](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。  
  
有关如何设置部署联合服务器代理所需的所有先决条件的常规信息，请参阅[核对清单：设置联合服务器代理](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md)。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
