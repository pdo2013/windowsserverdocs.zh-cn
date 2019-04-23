---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: Windows Server 2012 R2 中的 AD FS 设计指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f618add4c4803142b3bd7278908834a412f30999
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862568"
---
# <a name="identify-your-ad-fs-deployment-goals"></a>标识 AD FS 部署目标

>适用于：Windows Server 2016, Windows Server 2012 R2

正确识别 Active Directory 联合身份验证服务\(AD FS\)部署目标对于 AD FS 设计项目的成功至关重要。 确定优先级，并可能是，合并部署目标，以便您可以设计和部署 AD FS 通过使用一种迭代方法。 您可以充分利用现有的、 已记录在案、 和预定义的与 AD FS 设计和开发工作解决方案适合您情况的 AD FS 部署目标。  
  
以前版本的 AD FS 所最常部署的用于实现以下目标：  
  
-   员工或客户提供与 web\-基于，SSO 在访问时遇到声明\-基于你的企业中的应用程序。  
  
-   员工或客户提供与 web\-基于访问任何联合身份验证伙伴组织中的资源的 SSO 体验。  
  
-   员工或客户提供与 Web\-远程访问内部托管网站或服务时，SSO 体验。  
  
-   员工或客户提供与 web\-访问资源或云中的服务时，SSO 体验。  
  
除此之外，Windows Server® 2012 R2 中的 AD FS 添加功能，有助于您实现以下目标：  
  
-   SSO 设备工作区加入和无缝第二重身份验证。 这使组织能够允许从用户的个人设备访问，并提供此访问权限时管理风险。  
  
-   管理风险的多\-因素访问控制。 AD FS 提供控制谁能够访问什么应用程序的丰富授权级别。 这可以基于用户属性\(UPN、 电子邮件、 安全组成员身份、 身份验证强度等\)，设备属性\(设备是否已加入工作区\)或请求属性\(网络位置、 IP 地址或用户代理\)。  
  
-   管理风险的其他多\-敏感应用程序的身份验证。 AD FS 使您可以控制策略，可能要求多\-全局或基于每个应用程序重身份验证。 此外，AD FS 提供扩展点的任何多\-身份验证供应商以进行安全、 无缝多深度集成\-考虑最终用户体验。  
  
-   提供用于访问来自 extranet 的 web 资源，Web 应用程序代理的受保护的身份验证和授权功能。  
  
总之，可以部署 Windows Server 2012 R2 中的 AD FS 以实现你的组织中的以下目标：  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>让用户从任何位置访问其个人设备上的资源  
  
-   工作区加入使用户能够将个人设备加入企业 Active Directory，因此，他们在从这些设备访问企业资源时能够获得访问权限和无缝体验。  
  
-   Pre\-内部公司网络中保护的 Web 应用程序代理并从 internet 访问的资源进行身份验证。  
  
-   密码更改使用户能够在密码过期时从任何加入工作区的设备更改密码，以便他们能够继续访问资源。  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>增强你的访问控制风险管理工具  
在每个 IT 组织中，管理风险都是管理和合规的一个重要方面。 有大量访问控制风险管理增强功能在 Windows Server® 2012 R2 中 AD FS 中的包括以下：  
  
-   灵活控制，根据网络位置来管理如何用户进行身份验证访问 AD FS\-保护的应用程序。  
  
-   灵活的策略来确定用户是否需要执行多\-身份验证基于用户的数据、 设备数据和网络位置。  
  
-   每个\-应用程序控制，可忽略 SSO 并强制用户在每次访问敏感应用程序提供的凭据。  
  
-   每个灵活\-基于用户数据、 设备数据或网络位置上的应用程序访问策略。  
  
-   AD FS Extranet 锁定，使管理员能够保护 Active Directory 帐户免受来自 Internet 的暴力攻击。  
  
-   访问吊销，可用于 Active Directory 中禁用或删除的任何加入工作区的设备。  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>使用 AD FS 来增强符号\-体验中  
以下是 Windows Server® 2012 R2 中启用管理员联系，以自定义和增强登录的新 AD FS 功能\-体验：  
  
-   统一自定义 AD FS 服务，进行一次更改后，更改随后会自动传播到给定场中的剩余 AD FS 联合服务器。  
  
-   更新登录\-页中的外观更现代和自动适应不同的外形因素。  
  
-   支持自动回退到窗体\-基于设备的未加入企业域但仍使用生成从公司网络中的访问请求的身份验证\(intranet\)。  
  
-   简单控制，可自定义公司徽标、插图图像、IT 支持标准链接、主页、隐私等。  
  
-   签名中的说明自定义消息\-页中。  
  
-   自定义 Web 主题。  
  
-   主领域发现\(HRD\)根据增强公司的合作伙伴的隐私的用户的组织后缀。  
  
-   HRD 筛选每个\-按应用程序以自动选取领域基于应用程序。  
  
-   一个\-单击错误报告变得更容易 IT 故障排除。  
  
-   可自定义错误消息。  
  
-   多个身份验证提供程序可用时，提供用户身份验证选择。  
  
## <a name="see-also"></a>请参阅  
[Windows Server 2012 R2 中的 AD FS 设计指南](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

