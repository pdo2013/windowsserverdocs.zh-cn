---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: "为你识别索赔应用程序和服务提供你 Active Directory 用户访问"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f6fb37c16c20915c0051e3a24cdb0c147ae92d9c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>为你识别索赔应用程序和服务提供你 Active Directory 用户访问

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

当你管理员帐户合作伙伴组织中的 Active Directory 联合身份验证服务 \(AD FS\) 部署中和已提供给您托管的资源公司的网络上的员工 single\-上 sign\ \(SSO\) 访问的部署目标：  
  
-   登录到公司的网络中的 Active Directory 森林员工可以使用 SSO 多个应用或服务外围自己的组织中网络的访问。 广告 FS 保护这些应用程序和服务。  
  
    例如，Fabrikam 可能希望企业网络员工具有联合对基于 Web\ 的应用程序位于 Fabrikam 外围网络的访问权限。  
  
-   登录到 Active Directory 域远程员工可以从联合在你的组织联合访问广告 FS\ 保护 Web\ 基于应用程序或服务还驻留在你的组织的服务器获取广告 FS 标记。  
  
-   可以为员工的广告 FS 标记填充 Active Directory 特性应用商店中的信息。  
  
以下组件所需的部署这一目标：  
  
-   **Active Directory 域服务 \(AD DS\):**广告 DS 包含用于生成广告 FS 标记的员工的用户帐户。 填充信息，组会员身份和属性，如为广告 FS 标记为组索赔和自定义的索赔。  
  
    > [!NOTE]  
    > 你还可以使用轻型目录访问协议 \(LDAP\) 或结构化查询语言 \(SQL\) 包含用于广告 FS 令牌生成的身份。  
  
-   **公司 DNS:**域名系统 \(DNS\) 此实现包含简单主机 \(A\) 资源记录，以便 intranet 客户可以找到该帐户联合身份验证的服务器。 这种实现 DNS 还可能存放在公司的网络需要其他 DNS 记录。 有关详细信息，请参阅[联合身份验证的服务器的名称分辨率要求](Name-Resolution-Requirements-for-Federation-Servers.md)。  
  
-   **帐户合作伙伴联合服务器：**此联合身份验证的服务器已加入域帐户合作伙伴森林中。 它进行验证员工用户帐户，并生成广告 FS 标记。 客户端计算机员工针对此联盟服务器生成广告 FS 令牌执行 Windows 的集成身份验证。 有关详细信息，请参阅[查看联盟服务器伙伴帐户中的角色](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)。  
  
    帐户合作伙伴联合服务器可以进行以下的用户身份验证：  
  
    -   使用此域中的用户帐户员工  
  
    -   这片森林中的任意位置的用户帐户的员工  
  
    -   随时随地在林用户帐户的员工受这片森林 \（通过 two\ 方式 Windows trust\)  
  
-   **员工：**员工访问 Web\ 基于服务 \(through an application\) Web\ 基于体应用程序或 \（通过受支持的 Web browser\) 时他或她登录到公司的网络。 公司的网络上的员工的客户端计算机通信直接与联合身份验证的服务器。  
  
查看后链接主题中的信息，你就可以开始部署这一目标按照中的步骤[清单：实施联盟 Web SSO 设计](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)。  
  
下图显示了每个所需的组件，该广告 FS 部署目标。  
  
![访问你的索赔均](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
