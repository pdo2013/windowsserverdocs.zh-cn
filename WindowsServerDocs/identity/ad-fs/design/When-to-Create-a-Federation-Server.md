---
ms.assetid: 824005ae-c3c1-459b-9baa-1660158918ab
title: "何时创建联盟服务器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8013764b88a1061cfcaa3a507466c111bfd59aad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="when-to-create-a-federation-server"></a>何时创建联盟服务器

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

当您创建联盟 serverin Active Directory 联合身份验证服务 \(AD FS\) 时，您提供你的组织可以的一种方法：  
  
-   在 Web single\-上 sign\ 与 \ (SSO\) – 基于与另一个组织进行通信 \（，还具有至少一个联盟 server\），如有必要，使用你自己的组织中的员工 \ (谁需要访问 Internet 上 \)。  
  
-   启用前端服务模拟用户的基础结构服务使用的身份委派。 有关详细信息，请参阅[时使用的身份委派给](When-to-Use-Identity-Delegation.md)。  
  
以下部分介绍了一些用于确定何时以及在何处创建一个或多个联合身份验证的服务器关键的决策。  
  
## <a name="determine-the-organizational-role-for-the-federation-server"></a>确定联合身份验证的服务器组织角色  
若要使明智的决定。有关时创建一个新联合身份验证的服务器，必须先确定服务器将驻留哪些组织中。 联合服务器播放某个组织中的角色取决于是否或资源合作伙伴公司帐户合作伙伴公司中放置联合身份验证的服务器。  
  
在联合服务器置于帐户合作伙伴公司网络，它的作用是验证的浏览器、Web 服务或选择器身份的客户端用户凭据和发送给客户的安全标记。 有关详细信息，请参阅[查看联盟服务器伙伴帐户中的角色](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)。  
  
当联合服务器置于资源合作伙伴公司网络时，它的作用是根据资源合作伙伴组织中, 联盟服务器发送一个安全标记的用户身份验证或其角色是重定向到属于 client 帐户合作伙伴公司从配置的 Web 应用程序或 Web 服务的标记请求。 有关详细信息，请参阅[查看联盟服务器资源伙伴中的角色](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)。  
  
## <a name="determine-which-ad-fs-design-to-deploy"></a>确定哪些广告 FS 设计部署  
每当你想要部署任何以下广告 FS 设计，请在你的组织中创建联合身份验证的服务器：  
  
-   [Web SSO 设计](Web-SSO-Design.md)  
  
-   [Web 联盟的 SSO 设计](Federated-Web-SSO-Design.md)  
  
如有必要，部署联合 Web SSO 设计组织可以配置单个联盟服务器，以便它在帐户合作伙伴角色和资源合作伙伴角色进行操作。 在此情况下，联合服务器可能会产生根据其自己的组织中的用户帐户的安全性肯定标记语言 \(SAML\) 标记或重排标记请求组织机构对基于所在用户的帐户。  
  
> [!NOTE]  
> 联合 Web SSO 的设计，必须在至少有一个联盟服务器伙伴帐户中的并资源伙伴中的至少一个联盟服务器。  
  
## <a name="differences-between-a-federation-server-and-a-federation-server-proxy"></a>联合服务器和联合身份验证的服务器代理之间的差异  
联合服务器可用于出网页 sign\ 中策略、身份验证和发现联合身份验证的服务器代理情况一样。 联合服务器和联合身份验证的服务器代理之间的主要区别需要执行哪些操作联盟服务器可以执行联合身份验证的服务器代理无法执行。  
  
以下是联盟服务器可以执行的操作：  
  
-   联合身份验证的服务器执行产生令牌加密操作。 虽然联合身份验证的服务器代理无法产生标记，但他们可以使用路线，或向客户端中，并在必要，返回到联合身份验证的服务器时定向这些标记。 有关使用联合身份验证的服务器的详细信息，请参阅[何时创建联合身份验证的服务器代理](When-to-Create-a-Federation-Server-Proxy.md)。  
  
-   联合身份验证的服务器上的企业网络; 为客户支持使用 Windows 的集成身份验证未起效的联合身份验证的服务器代理。 有关使用联合身份验证的服务器上的 Windows 的集成身份验证的详细信息，请参阅[何时创建联合身份验证的服务器场](When-to-Create-a-Federation-Server-Farm.md)。  
  
> [!CAUTION]  
> 联合身份验证的服务器和配置数据库的 SQL Server、SQL Server 特性官方商城、域控制器和广告 LDS 实例之间进行通信不完整性或保密默认保护。 缓解这种情况，请考虑保护这些服务器使用 IPSEC 或使用所有这些服务器之间的物理安全连接之间的信道。 联合身份验证的服务器和 SQL server 之间进行通信，请考虑使用在连接字符串 SSL 保护。 联合身份验证的服务器和域控制器之间的连接，请考虑打开 Kerberos 签名和加密。 对于 LDAP，LDAP\/S 不支持的广告 LDS\/广告 DS。  
  
## <a name="how-to-create-a-federation-server"></a>如何创建联盟服务器  
你可以创建联盟服务器使用广告 FS 联盟服务器配置向导或 Fsconfig.exe command\ 行工具。 当您使用这些工具任一时，你可以选择以下选项来创建联盟服务器任一。  
  
-   创建 stand\ 单独联盟服务器  
  
    有关如何设置 stand\ 单独联盟服务器的详细信息，请参阅[创建独立联合服务器](../../ad-fs/deployment/Create-a-Stand-Alone-Federation-Server.md)。  
  
-   联合身份验证的服务器场中创建的第一个联盟服务器  
  
    有关如何第一台联合身份验证的服务器设置，或添加到场联合服务器的详细信息，请参阅[联合身份验证的服务器场中创建第一个联盟服务器](../../ad-fs/deployment/Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)。  
  
-   添加到联合身份验证的服务器场联合服务器  
  
    有关如何添加到场联合服务器的详细信息，请参阅[添加到联合身份验证的服务器场联合服务器](../../ad-fs/deployment/Add-a-Federation-Server-to-a-Federation-Server-Farm.md)。  
  
有关详细信息有关如何其中的每个选项起作用，请参阅[广告 FS 配置数据库中的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
有关如何设置所有先决条件部署联盟服务器所需的详细信息，请参阅[清单：设置联合服务器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)

