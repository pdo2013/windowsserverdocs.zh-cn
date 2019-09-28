---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: 为声明感知应用程序和服务提供 Active Directory 用户访问权限
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 48436f8e98af965f2bc2b38d296c4a15924e4db1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407955"
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>为声明感知应用程序和服务提供 Active Directory 用户访问权限

如果你是 Active Directory 联合身份验证服务 \(AD FS @ no__t 部署中帐户伙伴组织的管理员，并且你的部署目标是为员工提供单个 @ no__t-2sign @ no__t-3on \(SSO @ no__t-5 访问托管资源的企业网络：  
  
-   登录到企业网络中的 Active Directory 林的员工可以使用 SSO 访问你自己组织的外围网络中的多个应用程序或服务。 这些应用程序和服务受 AD FS 保护。  
  
    例如，Fabrikam 可能希望企业网络员工具有对 Fabrikam 外围网络中托管的 Web @ no__t-0based 应用程序的联合访问权限。  
  
-   登录到 Active Directory 域的远程员工可以从组织中的联合服务器获取 AD FS 令牌，以获取对 AD FS @ no__t-0secured Web @ no__t 1based 应用程序或服务的联合访问权限，这些应用程序或服务也驻留在你的内部.  
  
-   Active Directory 属性存储中的信息可以填充到员工的 AD FS 令牌中。  
  
此部署目标需要以下组件：  
  
-   **Active Directory 域服务 @no__t 1AD DS @ no__t-2：** AD DS 包含用于生成 AD FS 令牌的员工用户帐户。 组成员身份和属性等信息将作为组声明和自定义声明填充到 AD FS 令牌中。  
  
    > [!NOTE]  
    > 你还可以使用轻型目录访问协议 \(LDAP @ no__t 或结构化查询语言 \(SQL @ no__t，以包含用于 AD FS 令牌生成的标识。  
  
-   **企业 DNS：** 此域名系统的实现 \(DNS @ no__t，其中包含一个简单的主机 \(A @ no__t 资源记录，以便 intranet 客户端可以找到帐户联合服务器。 DNS 的此实现形式也可以托管企业网络所需的其他 DNS 记录。 有关详细信息，请参阅 [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md)。  
  
-   **帐户伙伴联合服务器：** 此联合服务器已加入帐户伙伴林中的域。 它对员工用户帐户进行身份验证并生成 AD FS 令牌。 员工的客户端计算机对此联合服务器执行 Windows 集成身份验证，以生成 AD FS 令牌。 有关详细信息，请参阅[查看帐户伙伴中的联合身份验证服务器的角色](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)。  
  
    帐户伙伴联合服务器可对以下用户进行身份验证：  
  
    -   此域中具有用户帐户的员工  
  
    -   位于此林中任意位置并具有用户帐户的员工  
  
    -   在受此林信任的林中的任意位置具有用户帐户的员工 \(through a 1way Windows 信任 @ no__t-2  
  
-   **员工：** 员工在登录到企业网络时，将访问一个 Web @ no__t-0based service \(through a application @ no__t 或 Web @ no__t-3based 应用程序 \(through 一个受支持的 Web 浏览器 @ no__t。 企业网络上员工的客户端计算机与联合服务器直接通信以进行身份验证。  
  
在查看链接的主题中的信息后，可以按照 [Checklist 中的步骤开始部署此目标：实现联合 Web SSO 设计 @ no__t-0。  
  
下图显示了此 AD FS 部署目标所需的每个组件。  
  
![对声明的访问权限](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
