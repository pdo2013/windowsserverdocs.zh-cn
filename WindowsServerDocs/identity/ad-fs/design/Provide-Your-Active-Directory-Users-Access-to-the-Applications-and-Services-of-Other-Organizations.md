---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: 为其他组织的应用程序和服务提供 Active Directory 用户访问权限
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d50d26c5c654e5c91b82f6f209e21f257221c12d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843578"
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>为其他组织的应用程序和服务提供 Active Directory 用户访问权限

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

此 Active Directory 联合身份验证服务\(AD FS\)部署目标基于中的目标[提供您 Active Directory 用户访问您的声明感知应用程序和服务](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)。  
  
当你是帐户伙伴组织中的管理员，并且你的部署目标是为员工提供对另一组织中的托管资源的联合访问权限时：  
  
-   在登录到公司网络中的 Active Directory 域的员工可以使用单个\-符号\-上\(SSO\)功能，用于访问多个 Web\-基于应用程序或服务，其中受保护的 AD FS 中，当应用程序或服务位于不同的组织。 有关详细信息，请参阅 [Federated Web SSO Design](Federated-Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能希望企业网络员工具有对 Contoso 中托管的 Web 服务的联合访问权限。  
  
-   在登录到 Active Directory 域的远程员工可以从您的组织获取联合到 AD FS 保护的 Web 访问中的联合服务器获取 AD FS 令牌\-基于应用程序或在另一个托管的服务组织。  
  
    例如，Fabrikam 可能希望其远程员工具有联合到 AD FS 保护的服务托管在 Contoso，而无需 Fabrikam 员工在 Fabrikam 企业网络上的访问。  
  
除了 [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) 中所述的基本组件和以下插图中以阴影显示的组件之外，比部署目标还需要以下组件：  
  
-   **帐户伙伴联合服务器代理：** 从 Internet 访问联合的服务或应用程序的员工可以使用此 AD FS 组件来执行身份验证。 默认情况下，此组件执行窗体身份验证，但它还可以执行基本身份验证。 你还可以配置此组件来执行安全套接字层\(SSL\)如果您的组织的员工可以提供证书的客户端身份验证。 有关详细信息，请参阅 [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
-   **外围 DNS：** 此实现的域名系统\(DNS\)提供外围网络的主机名。 有关如何为联合服务器代理配置外围 DNS 的详细信息，请参阅[联合服务器代理的名称解析要求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
-   **远程员工：** 远程员工可访问 Web\-基于应用程序\(通过受支持的 Web 浏览器\)或 Web\-基于服务\(通过应用程序\)，使用有效凭据公司的网络中，员工时非现场使用 Internet。 在远程位置中的员工的客户端计算机直接与要生成令牌并向应用程序或服务进行身份验证的联合身份验证服务器代理进行通信。  
  
查看链接的主题中的信息之后, 可以开始部署此目标中的步骤[核对清单：实现联合的 Web SSO 设计](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)。  
  
下图显示了每个 AD FS 部署目标所需的组件。  
  
![访问您的应用程序](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
