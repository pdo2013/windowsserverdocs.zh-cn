---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: 为声明感知应用程序和服务提供 Active Directory 用户访问权限
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f6fb37c16c20915c0051e3a24cdb0c147ae92d9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835868"
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>为声明感知应用程序和服务提供 Active Directory 用户访问权限

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

如果要在 Active Directory 联合身份验证服务在帐户伙伴组织中的管理员\(AD FS\)部署，并且您具有的部署目标是提供单一\-登录\-上\(SSO\)雇员在公司网络到你的托管资源的访问：  
  
-   登录到企业网络中的 Active Directory 林的员工可以使用 SSO 访问你自己组织的外围网络中的多个应用程序或服务。 这些应用程序和服务受 AD FS。  
  
    例如，Fabrikam 可能希望企业网络员工具有联合访问权限 Web\-基于 fabrikam 外围网络中托管的应用程序。  
  
-   在登录到 Active Directory 域的远程员工可以从你获取到 AD FS 的联合访问权限的组织中的联合服务器获取 AD FS 令牌\-保护 Web\-基于应用程序或服务也驻留在你的组织。  
  
-   Active Directory 属性存储中的信息可以填充到员工的 AD FS 令牌中。  
  
此部署目标需要以下组件：  
  
-   **Active Directory 域服务\(AD DS\):** AD DS 包含用于生成 AD FS 令牌的员工用户帐户。 组成员身份和属性等信息将作为组声明和自定义声明填充到 AD FS 令牌中。  
  
    > [!NOTE]  
    > 此外可以使用轻型目录访问协议\(LDAP\)或结构化查询语言\(SQL\)包含适用于 AD FS 标识令牌生成。  
  
-   **企业 DNS：** 此实现的域名系统\(DNS\)包含简单的主机\(A\)资源记录，以便 intranet 客户端可以找到帐户联合身份验证服务器。 DNS 的此实现形式也可以托管企业网络所需的其他 DNS 记录。 有关详细信息，请参阅 [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md)。  
  
-   **帐户伙伴联合身份验证服务器：** 此联合身份验证服务器加入帐户伙伴林中的域。 它对员工用户帐户进行身份验证并生成 AD FS 令牌。 该员工的客户端计算机执行 Windows 集成身份验证对此联合身份验证服务器，以生成 AD FS 令牌。 有关详细信息，请参阅 [Review the Role of the Federation Server in the Account Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)。  
  
    帐户伙伴联合身份验证服务器可以对以下用户进行身份验证：  
  
    -   此域中具有用户帐户的员工  
  
    -   位于此林中任意位置并具有用户帐户的员工  
  
    -   具有林中的任意位置的用户帐户的员工都受此林\(通过两个\-Windows 信任的方式\)  
  
-   **员工：** 员工可访问 Web\-基于服务\(通过应用程序\)或 Web\-基于应用程序\(通过受支持的 Web 浏览器\)时他或她登录到公司网络。 企业网络上的员工的客户端计算机直接与联合身份验证服务器进行身份验证进行通信。  
  
查看链接的主题中的信息之后, 可以开始部署此目标中的步骤[核对清单：实现联合的 Web SSO 设计](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)。  
  
下图显示了每个 AD FS 部署目标所需的组件。  
  
![访问您的声明](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
