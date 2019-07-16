---
ms.assetid: 824005ae-c3c1-459b-9baa-1660158918ab
title: 何时创建联合服务器
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e61c734780baa1482670af3f24697c10345b292
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190586"
---
# <a name="when-to-create-a-federation-server"></a>何时创建联合服务器

当你创建联合身份验证 serverin Active Directory 联合身份验证服务\(AD FS\)，提供你的组织可以通过其中一种方法：  
  
-   参与 Web 单\-符号\-上\(SSO\)– 基于与另一组织的通信\(还具有至少一台联合服务器\)和，如有必要，使用在您自己的组织的员工\(谁需要访问通过 Internet\)。  
  
-   使前端服务可以使用标识委派向基础结构服务模拟用户。 有关详细信息，请参阅 [When to Use Identity Delegation](When-to-Use-Identity-Delegation.md)。  
  
以下各节介绍了一些用于确定何时以及要在其中创建一个或多个联合身份验证服务器的关键决策。  
  
## <a name="determine-the-organizational-role-for-the-federation-server"></a>确定联合服务器的组织角色  
若要做出明智的决策有关何时创建新的联合身份验证服务器，必须首先确定服务器所处的组织。 联合身份验证服务器的组织中所扮演的角色取决于是否是帐户伙伴组织或资源伙伴组织中放置联合身份验证服务器。  
  
当联合身份验证服务器放置在帐户伙伴的企业网络中时，其作用是对浏览器、 Web 服务或标识选择器客户端的用户凭据进行身份验证，并将安全令牌发送到客户端。 有关详细信息，请参阅[查看帐户伙伴中的联合身份验证服务器的角色](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)。  
  
当联合身份验证服务器放置在资源伙伴的企业网络中时，它的作用是用户，基于由资源伙伴组织中的联合服务器颁发的安全令牌进行身份验证或它的作用是从令牌请求重定向配置 Web 应用程序或 Web 服务添加到客户端属于在帐户伙伴组织。 有关详细信息，请参阅[查看资源伙伴中的联合身份验证服务器的角色](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)。  
  
## <a name="determine-which-ad-fs-design-to-deploy"></a>确定要部署的 AD FS 设计  
只要您想要部署任何以下 AD FS 设计你的组织中创建联合身份验证服务器：  
  
-   [Web SSO 设计](Web-SSO-Design.md)  
  
-   [联合 Web SSO 设计](Federated-Web-SSO-Design.md)  
  
如有必要，部署联合 Web SSO 设计的组织可以配置单个联合身份验证服务器，以便它充当在帐户伙伴角色和资源伙伴角色中。 在这种情况下，联合身份验证服务器可能会产生安全断言标记语言\(SAML\)基于其自己的组织或将令牌请求重新路由到组织中的用户帐户的令牌基于用户的帐户所在的位置.  
  
> [!NOTE]  
> 对于联合 Web SSO 设计中，必须在帐户伙伴中的至少一台联合服务器和资源伙伴中的至少一台联合服务器。  
  
## <a name="differences-between-a-federation-server-and-a-federation-server-proxy"></a>联合服务器与联合服务器代理之间的差异  
联合身份验证服务器可以为符号分配网页\-in、 策略、 身份验证和相同的联合服务器代理的方式处理中的发现。 联合身份验证服务器和联合服务器代理之间的主要差异所要做的操作联合身份验证服务器可以执行的联合身份验证服务器代理无法执行。  
  
只有联合身份验证服务器可以执行的操作如下：  
  
-   联合身份验证服务器执行生成令牌的加密操作。 尽管联合服务器代理无法生成令牌，但它们可以用于路由或重定向令牌，向客户端中，并且如有必要，返回到联合身份验证服务器。 有关使用联合身份验证服务器的详细信息，请参阅[何时创建联合服务器代理](When-to-Create-a-Federation-Server-Proxy.md)。  
  
-   联合身份验证服务器支持 Windows 集成身份验证使用的企业网络; 上的客户端联合服务器代理不这样做。 有关使用 Windows 集成身份验证的联合身份验证服务器的详细信息，请参阅[何时创建联合服务器场](When-to-Create-a-Federation-Server-Farm.md)。  
  
> [!CAUTION]  
> 联合服务器与 SQL Server 配置数据库、SQL Server 属性存储、域控制器和 AD LDS 实例之间的通信不是默认情况下受保护的完整性或机密性。 若要缓解此问题，请考虑通过使用 IPSEC 或是在所有这些服务器之间使用物理上安全的连接来保护这些服务器之间的通信通道。 对于联合服务器与 SQL 服务器之间的通信，请考虑在连接字符串中使用 SSL 保护。 对于联合服务器与域控制器之间的连接，请考虑启用 Kerberos 签名和加密。 Ldap，LDAP\/S 不支持为 AD LDS\/AD DS。  
  
## <a name="how-to-create-a-federation-server"></a>如何创建联合服务器  
可以创建联合身份验证服务器使用 AD FS 联合身份验证服务器配置向导或 Fsconfig.exe 命令\-行工具。 当你使用任一工具时，可以选择以下任何选项来创建联合服务器。  
  
-   创建独立\-单独的联合身份验证服务器  
  
    详细了解如何设置独立\-单独的联合身份验证服务器，请参阅[创建独立联合身份验证服务器](../../ad-fs/deployment/Create-a-Stand-Alone-Federation-Server.md)。  
  
-   在联合服务器场中创建第一个联合服务器  
  
    有关如何设置第一个联合服务器或向场中添加联合身份验证服务器的详细信息，请参阅[联合服务器场中创建第一个联合身份验证服务器](../../ad-fs/deployment/Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)。  
  
-   将联合服务器添加到联合服务器场  
  
    有关如何向场中添加联合身份验证服务器的详细信息，请参阅[将联合身份验证服务器添加到联合服务器场](../../ad-fs/deployment/Add-a-Federation-Server-to-a-Federation-Server-Farm.md)。  
  
详细信息，其中每个选项的工作原理的详细信息，请参阅[AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
有关如何设置部署联合服务器所需的所有先决条件的详细信息，请参阅[核对清单：设置联合身份验证服务器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

