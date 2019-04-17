---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: "在 Windows Server 2012 R2 指导广告 FS 设计"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f618add4c4803142b3bd7278908834a412f30999
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="identify-your-ad-fs-deployment-goals"></a>确定您的广告 FS 部署目标

>适用于：Windows Server 2016，Windows Server 2012 R2

正确识别您的 Active Directory 联合身份验证服务 \(AD FS\) 部署目标至关重要的广告 FS 设计项目成功。 优先考虑并，可能是，结合部署目标，以便你可以设计并将其部署广告 FS 使用迭代的方法。 你随时可以充分利用现有、 记录，并且预定义的开发工作解决方案您的具体情况，相关的广告 FS 设计广告 FS 部署目标。  
  
以前版本的广告 FS 最常部署实现以下：  
  
-   提供你的员工或客户 web\ 基于，SSO 体验时访问 claims\ 基于你的企业中的应用程序。  
  
-   你的员工或客户提供访问资源任何联盟合作伙伴组织中基于 web\ SSO 体验。  
  
-   提供你的员工或客户 Web\ 基于，远程访问内部托管网站或服务时遇到 SSO。  
  
-   为你的员工或客户提供基于 web\ SSO 体验访问资源或在云中的服务时。  
  
除了这些内容，在 Windows Server® 2012 R2 的广告 FS 添加功能，可帮助你获得以下：  
  
-   设备 SSO 和无缝秒的工作区加入因素身份验证。 这使公司允许用户的个人设备的访问权限，并管理存在风险时提供此访问权限。  
  
-   管理 multi\ 因素访问控制性风险。 广告 FS 提供授权丰富级别的控制可以访问哪些应用程序。 这可以根据用户属性 \ (UPN，电子邮件、 安全组成员、 身份验证强度等 \)，设备属性 \ （无论设备是工作区 joined\） 或请求属性 \ （网络位置、 IP 地址或用户 agent\）。  
  
-   管理其他 multi\ 双因素身份验证的敏感应用程序的风险。 广告 FS 允许你控制策略全球或基于每个应用程序可能需要 multi\ 强双因素身份。 此外，广告 FS 提供了扩展点集成深安全、 无缝 multi\ 因素体验为最终用户任何 multi\ 因素供应商。  
  
-   提供访问 web 资源从网受 Web 应用程序代理身份验证和授权的功能。  
  
总结，在 Windows Server 2012 R2 的广告 FS 可以部署实现以下目标，在你的组织中：  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>使你的用户可以从任何地方访问他们的个人设备上的资源  
  
-   使用户可以到公司的 Active Directory 加入他们的个人设备，并因此获得访问和体验无缝访问公司资源从这些设备时的工作区加入。  
  
-   Pre\ 身份验证的企业网络，受保护的 Web 应用程序代理通过从 internet 访问内的资源。  
  
-   更改密码，以使用户可以从任何工作区更改密码已加入他们的密码，以便他们可以继续访问资源到期时的设备。  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>增强你访问控制风险管理工具  
管理风险是一个重要方面的管辖和合规性每个 IT 部门。 有许多访问控制风险管理增强功能在 Windows Server® 2012 R2 的广告 FS 中包括以下：  
  
-   根据来控制如何用户验证来访问广告 FS\ 保护应用程序的网络位置灵活的控件。  
  
-   若要确定是否需要执行根据用户的数据、 设备数据和网络位置 multi\ 强双因素身份一个用户的灵活策略。  
  
-   Per\ 应用程序控件忽略 SSO 和强制用户提供每次访问敏感应用时的凭据。  
  
-   灵活 per\ 应用程序访问策略基于用户数据、 设备数据或网络位置。  
  
-   广告 FS 联网锁定，这使管理员以防止暴力攻击从 internet 上的 Active Directory 的帐户。  
  
-   对于任何工作区访问吊销加入禁用或删除 Active Directory 中的设备。  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>使用广告 FS 增强 sign\ 中的体验  
以下是在 Windows Server® 2012 R2 启用管理员联系以自定义和增强的 sign\ 体验的新广告 FS 功能：  
  
-   统一自定义的广告 FS 服务，其中一次做出并自动传播到其他广告 FS 联盟服务器给定场中所做的更改。  
  
-   已更新的 sign\ 中的页面，通过查看现代自动适应不同外形规格。  
  
-   自动回退到的设备未加入域企业，但仍用于 forms\ 基于身份验证的支持生成企业网络 \(intranet\) 内的访问权限的请求。  
  
-   若要自定义公司徽标图图像，IT 支持主页、 隐私、 等标准链接的简单控件。  
  
-   自定义的 sign\ 在网页中的描述消息。  
  
-   自定义的 web 主题。  
  
-   家庭领域发现 \(HRD\) 基于组织职务的用户提供增强的公司的合作伙伴的隐私。  
  
-   HRD per\ 应用程序的基础上筛选自动选择的应用程序的领域。  
  
-   One\ 单击的错误报告变得更容易 IT 疑难解答。  
  
-   自定义的错误消息。  
  
-   当多个身份验证的提供商可用时的用户身份验证选择。  
  
## <a name="see-also"></a>请参阅  
[在 Windows Server 2012 R2 指导广告 FS 设计](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

