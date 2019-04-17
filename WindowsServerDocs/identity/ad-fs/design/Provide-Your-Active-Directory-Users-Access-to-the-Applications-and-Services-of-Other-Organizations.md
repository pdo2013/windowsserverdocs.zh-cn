---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: "提供您 Active Directory 用户的访问权限的应用程序和其他公司的服务"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d50d26c5c654e5c91b82f6f209e21f257221c12d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>提供您 Active Directory 用户的访问权限的应用程序和其他公司的服务

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

此 Active Directory 联合身份验证服务 \(AD FS\) 部署目标版本上的目标[你 Active Directory 用户提供访问您的索赔识别应用程序和服务](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)。  
  
当你是管理员帐户合作伙伴组织中的和你拥有的部署目标向员工提供的联盟的访问托管另一个组织中的资源：  
  
-   登录到公司的网络中的 Active Directory 域员工可以使用 single\-上 sign\ \(SSO\) 功能多个 Web\ 基于应用程序或服务，其中受广告 FS 时所在不同的组织的应用程序或服务访问。 有关详细信息，请参阅[联盟 Web SSO 设计](Federated-Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能希望企业网络员工具有联合承载 Contoso 中的 Web 服务的访问。  
  
-   登录到 Active Directory 域远程员工可以从联盟服务器，在你的组织来访问联盟广告保护 FS – Web\ 基于应用程序或服务中的其他组织托管获取广告 FS 标记。  
  
    例如，Fabrikam 可能想要有联合访问广告 FS – 受保护的服务中 Contoso，而无需 Fabrikam 员工 Fabrikam 企业网络上托管其远程员工。  
  
除了中所述的基础组件[你 Active Directory 用户提供访问您的索赔识别应用程序和服务](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)和，在下图中为灰色，以下组件所需的部署这一目标：  
  
-   **帐户合作伙伴联合身份验证的服务器代理：**从 Internet 访问服务联盟或应用程序的员工可以使用该广告 FS 组件进行身份验证。 默认情况下此组件执行形式的身份验证，但它也可以执行基本身份验证。 你还可以配置此组件执行安全套接字层 \(SSL\) 客户端身份验证，如果在你的组织的员工出示的证书。 有关详细信息，请参阅[放置联合身份验证的服务器代理](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
-   **周边 DNS:**域名系统 \(DNS\) 此实现提供主机外围网络名称。 有关如何为联合身份验证的服务器代理配置周边 DNS 的详细信息，请参阅[联合身份验证的服务器代理名称分辨率要求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
-   **远程员工：**远程员工访问 Web\ 基于体应用程序 \（通过受支持的 Web browser\) 或 Web\ 基于服务 \（通过 application\) 员工现场使用 Internet 时使用有效的企业网络，从的凭据。 在远程位置的员工的客户端计算机直接与联合身份验证的服务器代理生成一个标记，并向应用程序或服务身份验证进行通信。  
  
查看后链接主题中的信息，你就可以开始部署这一目标按照中的步骤[清单：实施联盟 Web SSO 设计](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)。  
  
下图显示了每个所需的组件，该广告 FS 部署目标。  
  
![访问你的应用](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
