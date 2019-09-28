---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: 为另一个组织中的用户提供对声明感知应用程序和服务的访问权限
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a0b2429599036f2893f23df7921a11c8232d9f67
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359067"
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>为另一个组织中的用户提供对声明感知应用程序和服务的访问权限


当你是中的资源伙伴组织中的管理员时 Active Directory 联合身份验证服务 @no__t 0AD FS @ no__t-1，并且你的部署目标是为另一个组织中的用户提供联合访问 \(the 帐户伙伴组织 @ no__t 到 no__t-4aware 应用程序或位于你的组织中的 Web @ no__t-5based 服务 \(the 资源伙伴组织 @ no__t-7：  
  
-   你的组织中的联合用户和已为你的组织配置联合信任的组织中的联合用户 \(account partner 组织 @ no__t 可以访问组织托管的 AD FS 安全的应用程序或服务。 有关详细信息，请参阅 [Federated Web SSO Design](Federated-Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能希望其企业网络员工对 Contoso 中托管的 Web 服务具有联合访问权限。  
  
-   与受信任组织没有直接关联的联合用户 @no__t 0such 作为个人客户 @ no__t （登录到在外围网络中托管的属性存储）的用户可以访问多个 AD FS @ no__t 2secured 应用程序。也可以通过从位于 Internet 上的客户端计算机登录一次，在外围网络中托管。 换句话说，当你托管客户帐户以实现对外围网络中的应用程序或服务的访问时，你在属性存储中托管的客户只需登录一次，便可访问外围网络中的一个或多个应用程序或服务。 有关详细信息，请参阅 [Web SSO Design](Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能希望其客户对其外围网络中托管的多个应用程序或服务具有单个 @ no__t-0sign @ no__t-1on \(SSO @ no__t。  
  
此部署目标需要以下组件：  
  
-   **Active Directory 域服务 @no__t 1AD DS @ no__t-2：** 资源伙伴联合服务器必须加入到 Active Directory 域。  
  
-   **外围 DNS：** 域名系统 \(DNS @ no__t 应包含一个简单的主机 \(A @ no__t 资源记录，以便客户端计算机可以找到资源伙伴联合服务器和 Web 服务器。 DNS 服务器可以托管在外围网络中也需要的其他 DNS 记录。 有关详细信息，请参阅 [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md)。  
  
-   **资源伙伴联合服务器：** 资源伙伴联合服务器验证帐户伙伴发送 AD FS 令牌。 帐户伙伴发现是通过此联合服务器执行的。 有关详细信息，请参阅[查看资源伙伴中的联合身份验证服务器的角色](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)。  
  
-   **Web 服务器：** Web 服务器可以托管 Web 应用程序或 Web 服务。 Web 服务器先确认它从联合用户收到有效 AD FS 令牌，然后才允许访问受保护的 Web 应用程序或 Web 服务。  
  
    通过使用 Windows Identity Foundation \(WIF @ no__t，你可以开发 Web 应用程序或服务，以便它接受使用任何标准登录方法（如用户名和密码）进行的联合用户登录请求。  
  
在查看链接的主题中的信息后，可以按照 [Checklist 中的步骤开始部署此目标：实现联合 Web SSO 设计 @ no__t-0 和 [Checklist：实现 Web SSO 设计 @ no__t-0。  
  
下图显示了此 AD FS 部署目标所需的每个组件。  
  
![对声明的访问权限](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
