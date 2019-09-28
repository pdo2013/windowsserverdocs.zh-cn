---
ms.assetid: 824005ae-c3c1-459b-9baa-1660158918ab
title: 何时创建联合服务器
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 91c260dad1bd260a7dad7320fecd15e6472c50a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407880"
---
# <a name="when-to-create-a-federation-server"></a>何时创建联合服务器

当你创建 serverin \(AD FS @ no__t Active Directory 联合身份验证服务的联合身份验证时，你可以提供一种方法，使你的组织能够：  
  
-   与另一组织 @no__t 的 Web single @ no__t-0sign @ no__t/1on \(SSO @ no__t 进行的通信，并在必要时还至少有一台联合服务器 @ 4that-5，并在必要时，必须与自己组织中的员工 @no__t-no__t通过 Internet @ no__t 访问。  
  
-   使前端服务可以使用标识委派向基础结构服务模拟用户。 有关详细信息，请参阅 [When to Use Identity Delegation](When-to-Use-Identity-Delegation.md)。  
  
以下部分介绍了确定何时以及在何处创建一个或多个联合服务器的一些关键决策。  
  
## <a name="determine-the-organizational-role-for-the-federation-server"></a>确定联合服务器的组织角色  
若要明智地决定何时创建新的联合服务器，你必须首先确定服务器将驻留在哪个组织中。 联合服务器在组织中扮演的角色取决于你是将联合服务器放在帐户伙伴组织中还是资源伙伴组织中。  
  
当联合服务器置于帐户伙伴的企业网络中时，其角色是对浏览器、Web 服务或身份选择器客户端的用户凭据进行身份验证，并将安全令牌发送到客户端。 有关详细信息，请参阅[查看帐户伙伴中的联合身份验证服务器的角色](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)。  
  
当联合服务器位于资源伙伴的企业网络中时，其角色是基于由资源伙伴组织中的联合服务器颁发的安全令牌对用户进行身份验证，或其角色是将令牌请求从为客户端所属的帐户伙伴组织配置 Web 应用程序或 Web 服务。 有关详细信息，请参阅[查看资源伙伴中的联合身份验证服务器的角色](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)。  
  
## <a name="determine-which-ad-fs-design-to-deploy"></a>确定要部署的 AD FS 设计  
每当要部署以下任何 AD FS 设计时，你将在组织中创建联合服务器：  
  
-   [Web SSO 设计](Web-SSO-Design.md)  
  
-   [联合 Web SSO 设计](Federated-Web-SSO-Design.md)  
  
如有必要，部署联合 Web SSO 设计的组织可以配置单个联合服务器，使其在帐户伙伴角色和资源伙伴角色中起作用。 在这种情况下，联合服务器可能会基于其自己组织中的用户帐户生成安全断言标记语言 @no__t 0SAML @ no__t 标记，或者根据用户帐户所在的位置将令牌请求重新路由到组织。  
  
> [!NOTE]  
> 对于联合 Web SSO 设计，帐户伙伴中必须至少有一个联合服务器，并且资源伙伴中必须至少有一个联合服务器。  
  
## <a name="differences-between-a-federation-server-and-a-federation-server-proxy"></a>联合服务器与联合服务器代理之间的差异  
联合服务器可以用与联合服务器代理相同的方式为 @ no__t-0in、策略、身份验证和发现提供 Web 页。 联合服务器和联合服务器代理之间的主要区别与联合服务器代理无法执行的操作有关，而联合服务器代理无法执行的操作。  
  
下面是只有联合服务器可以执行的操作：  
  
-   联合服务器执行生成令牌的加密操作。 尽管联合服务器代理无法生成令牌，但是它们可以用于将令牌路由或重定向到客户端，并在必要时将它们重定向到联合服务器。 有关使用联合服务器的详细信息，请参阅[When To Create a Federation Server Proxy](When-to-Create-a-Federation-Server-Proxy.md)。  
  
-   联合服务器支持对企业网络上的客户端使用 Windows 集成身份验证;联合服务器代理不会。 有关将 Windows 集成身份验证用于联合服务器的详细信息，请参阅[何时创建联合服务器场](When-to-Create-a-Federation-Server-Farm.md)。  
  
> [!CAUTION]  
> 联合服务器与 SQL Server 配置数据库、SQL Server 属性存储、域控制器和 AD LDS 实例之间的通信不是默认情况下受保护的完整性或机密性。 若要缓解此问题，请考虑通过使用 IPSEC 或是在所有这些服务器之间使用物理上安全的连接来保护这些服务器之间的通信通道。 对于联合服务器与 SQL 服务器之间的通信，请考虑在连接字符串中使用 SSL 保护。 对于联合服务器与域控制器之间的连接，请考虑启用 Kerberos 签名和加密。 对于 LDAP，AD LDS @ no__t-1AD DS 不支持 LDAP @ no__t-0。  
  
## <a name="how-to-create-a-federation-server"></a>如何创建联合服务器  
您可以使用 AD FS 联合服务器配置向导或 Fsconfig.exe command @ no__t-0line 工具创建联合服务器。 当你使用任一工具时，可以选择以下任何选项来创建联合服务器。  
  
-   创建备用 @ no__t-0alone 联合服务器  
  
    若要详细了解如何设置 no__t-0alone 联合服务器，请参阅[创建独立的联合服务器](../../ad-fs/deployment/Create-a-Stand-Alone-Federation-Server.md)。  
  
-   在联合服务器场中创建第一个联合服务器  
  
    有关如何设置第一个联合服务器或向场中添加联合身份验证服务器的详细信息，请参阅[联合服务器场中创建第一个联合身份验证服务器](../../ad-fs/deployment/Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)。  
  
-   将联合服务器添加到联合服务器场  
  
    有关如何向场中添加联合身份验证服务器的详细信息，请参阅[将联合身份验证服务器添加到联合服务器场](../../ad-fs/deployment/Add-a-Federation-Server-to-a-Federation-Server-Farm.md)。  
  
详细信息，其中每个选项的工作原理的详细信息，请参阅[AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
有关如何设置部署联合服务器所需的所有先决条件的详细信息，请参阅 [Checklist：设置联合服务器 @ no__t。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

