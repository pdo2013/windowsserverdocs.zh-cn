---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: "提供您的索赔识别的应用程序和服务中的用户另一个组织访问"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b4ec08182e2523b0fcb16088ec9c1d094a5923fe
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>提供您的索赔识别的应用程序和服务中的用户另一个组织访问

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

当您是在资源合作伙伴组织中的 Active Directory 联合身份验证服务 \(AD FS\) 管理员且您部署目标为另一台组织中的用户提供联盟的访问 \ (帐户合作伙伴 organization\) 到 claims\ 识别的应用程序或位于你的组织中基于 Web\ 服务 \ (资源合作伙伴 organization\):  
  
-   联合的用户都可以和你的组织中配置了联合身份验证的企业为你的组织信任 \ (帐户合作伙伴 organizations\) 可以访问广告 FS 保护应用程序或由你的组织托管的服务。 有关详细信息，请参阅[联盟 Web SSO 设计](Federated-Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能希望其企业网络员工具有联合承载 Contoso 中的 Web 服务的访问。  
  
-   联合具有无意直接与受信任的某一组织用户 \（如个别 customers\) 人登录到特性官方商城托管外围网络中，可以访问多广告 FS\ 保护应用程序，它还托管外围网络中, 通过从 Internet 上的客户端计算机一次登录。 换言之，当主机客户帐户以启用应用程序或服务中外围网络的访问、特性存储在主机的客户可以访问一个或多个应用或服务外围网络只需通过一次登录。 有关详细信息，请参阅[Web SSO 设计](Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能需要客户能够 single\-上 sign\ \(SSO\) 访问的多个应用或服务承载它周边网络中。  
  
以下组件所需的部署这一目标：  
  
-   **Active Directory 域服务 \(AD DS\):**必须到 Active Directory 域加入资源合作伙伴联合身份验证的服务器。  
  
-   **周边 DNS:**域名称系统 \(DNS\) 应包含一个简单的主机 \(A\) 资源记录，以便客户端计算机可以找到资源合作伙伴联合身份验证的服务器和 Web 服务器。 DNS 服务器可能举办其他还需要外围网络中的 DNS 记录。 有关详细信息，请参阅[联合身份验证的服务器的名称分辨率要求](Name-Resolution-Requirements-for-Federation-Servers.md)。  
  
-   **资源合作伙伴联合服务器：**资源合作伙伴联合服务器验证帐户合作伙伴发送的广告 FS 标记。 帐户合作伙伴发现都通过此联合身份验证的服务器。 有关详细信息，请参阅[查看联盟服务器资源伙伴中的角色](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)。  
  
-   **Web 服务器：** Web 服务器可以托管 Web 应用程序或 Web 服务。 Web 服务器确认它接收来自联盟用户有效的广告 FS 标记之前它允许受保护的 Web 应用程序或 Web 服务的访问。  
  
    通过使用 Windows 的身份基础 \(WIF\)，您可以开发你的 Web 应用程序或服务，以便它接受联合的用户登录请求所做的任何标准登录方法，如用户名和密码。  
  
查看后链接主题中的信息，你就可以开始部署这一目标按照中的步骤[清单：实施联盟 Web SSO 设计](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)和[清单：实施 Web SSO 设计](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md)。  
  
下图显示了每个所需的组件，该广告 FS 部署目标。  
  
![访问你的索赔均](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
