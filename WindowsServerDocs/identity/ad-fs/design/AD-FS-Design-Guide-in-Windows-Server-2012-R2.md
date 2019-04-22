---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Windows Server 2012 R2 中的 AD FS 设计指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 498b399818fb8c9e463f9990fa13c87648c0a33d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822148"
---
# <a name="ad-fs-design-guide-in-windows-server-2012-r2"></a>Windows Server 2012 R2 中的 AD FS 设计指南

>适用于：Windows Server 2016, Windows Server 2012 R2

Active Directory 联合身份验证服务\(AD FS\)提供了简便而安全的标识联合身份验证和 Web 单一登录\-上\(SSO\)的最终用户想要访问的应用程序的功能与 AD FS 内\-保护企业，在联合身份验证伙伴组织中或在云中。  
  
在 Windows Server® 2012 R2 AD FS 包含一个联合身份验证服务角色服务，充当标识提供者\(用户提供向信任 AD FS 应用程序的安全令牌进行身份验证\)或作为联合身份验证提供程序\(使用来自其他标识提供者的令牌，并提供到信任 AD FS 的应用程序的安全令牌\)。  
  
现在由一个称为 Web 应用程序代理的新远程访问角色服务来提供对在 Windows Server 2012 R2 中受 AD FS 保护的应用程序和服务的 Extranet 访问。 这与之前版本的 Windows Server 使用 AD FS 联合服务器代理处理此功能有所不同。 Web 应用程序代理是旨在为 AD FS 提供访问的服务器角色\-相关的 extranet 方案和其他 extranet 方案。 Web 应用程序代理的详细信息，请参阅[Web 应用程序代理操作实例指南](https://technet.microsoft.com/library/dn280944.aspx)。  
  
## <a name="about-this-guide"></a>关于本指南  
本指南提供建议来帮助你规划 AD FS 中，根据你的组织要求的新部署。 本指南供基础结构专家或系统架构师使用。 计划 AD FS 部署时，它重点介绍主要决策点。 在阅读本指南之前，应熟悉的功能级别上的 AD FS 工作原理。 有关详细信息，请参阅 [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)。  
  
## <a name="in-this-guide"></a>本指南包含的内容  
  
-   [标识 AD FS 部署目标](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [规划 AD FS 部署拓扑](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [AD FS 要求](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>请参阅  
[AD FS 设计](../../ad-fs/AD-FS-Design.md)  
  

