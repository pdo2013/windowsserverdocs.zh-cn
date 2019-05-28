---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: 为另一个组织中的用户提供对声明感知应用程序和服务的访问权限
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4a13332cd7cf6361824f05ead4568a45211cc70a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191025"
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>为另一个组织中的用户提供对声明感知应用程序和服务的访问权限


如果要在 Active Directory 联合身份验证服务资源伙伴组织中的管理员\(AD FS\) ，并且必须为另一组织中的用户提供联合访问权限的部署目标是\(帐户伙伴组织\)为索赔\-感知应用程序或 Web\-基于位于你的组织中的服务\(资源伙伴组织\):  
  
-   联合的用户在组织中以及在已配置联合身份验证的组织中与你的组织信任\(帐户合作伙伴组织\)可以访问由承载 AD FS 保护的应用程序或服务应用组织。 有关详细信息，请参阅 [Federated Web SSO Design](Federated-Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能希望其企业网络员工对 Contoso 中托管的 Web 服务具有联合访问权限。  
  
-   联合与受信任的组织没有直接关联的用户\(如个人客户\)，登录到的属性存储的托管在外围网络中，可以访问多个 AD FS\-受保护的应用程序，也托管在外围网络中，通过一次从位于 Internet 上的客户端计算机上的日志记录。 换句话说，当你托管客户帐户以实现对外围网络中的应用程序或服务的访问时，你在属性存储中托管的客户只需登录一次，便可访问外围网络中的一个或多个应用程序或服务。 有关详细信息，请参阅 [Web SSO Design](Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能希望其客户可以对单一\-符号\-上\(SSO\)访问多个应用程序或在其外围网络中托管的服务。  
  
此部署目标需要以下组件：  
  
-   **Active Directory 域服务\(AD DS\):** 资源伙伴联合身份验证服务器必须加入到 Active Directory 域。  
  
-   **外围 DNS：** 域名系统\(DNS\)应包含简单的主机\(A\)资源记录，以便客户端计算机可以找到资源伙伴联合身份验证服务器和 Web 服务器。 DNS 服务器可以托管在外围网络中也需要的其他 DNS 记录。 有关详细信息，请参阅 [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md)。  
  
-   **资源伙伴联合身份验证服务器：** 资源伙伴联合身份验证服务器验证帐户伙伴发送的 AD FS 令牌。 帐户伙伴发现通过此联合身份验证服务器执行。 有关详细信息，请参阅 [Review the Role of the Federation Server in the Resource Partner](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)。  
  
-   **Web 服务器：** Web 服务器可以托管 Web 应用程序或 Web 服务。 Web 服务器先确认它从联合用户收到有效 AD FS 令牌，然后才允许访问受保护的 Web 应用程序或 Web 服务。  
  
    通过使用 Windows Identity Foundation \(WIF\)，可以开发 Web 应用程序或服务，以便它可以接受联合用户都是使用任何标准登录方法，如用户名和密码的登录请求。  
  
查看链接的主题中的信息之后, 可以开始部署此目标中的步骤[核对清单：实现联合的 Web SSO 设计](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)和[核对清单：实现 Web SSO 设计](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md)。  
  
下图显示了每个 AD FS 部署目标所需的组件。  
  
![访问您的声明](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
