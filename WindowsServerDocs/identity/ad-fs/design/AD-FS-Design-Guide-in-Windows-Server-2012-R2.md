---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: "在 Windows Server 2012 R2 指导广告 FS 设计"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 498b399818fb8c9e463f9990fa13c87648c0a33d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-design-guide-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 指导广告 FS 设计

>适用于：Windows Server 2016，Windows Server 2012 R2

Active Directory 联合身份验证服务 \(AD FS\) 提供适用于想要访问广告 FS\ 保护企业内，在联盟合作伙伴组织中，或在云中的应用程序的最终用户身份简化、安全联盟和 Web 单个 sign\ 上 \(SSO\) 功能。  
  
在 Windows Server® 2012 R2 广告 FS 包含联合身份验证服务角色服务充当身份提供商的名字 \（身份验证的用户提供的应用程序信任广告 FS\ 安全令牌）或作为联盟提供商 \（消耗标记来自其他身份提供商，然后提供安全令牌信任广告 FS\ 的应用程序）。  
  
现在，新远程访问角色服务称为 Web 应用程序代理执行提供给应用程序和服务进行保护的在 Windows Server 2012 R2 的广告 FS 外部访问的功能。 这是从以前版本的 Windows Server 的此功能由广告 FS 联合身份验证的服务器代理航班出发。 Web 应用程序代理是旨在提供访问广告 FS\ 相关网方案和其他联网方案服务器角色。 Web 应用程序代理服务器上的详细信息，请参阅[Web 应用程序代理演练指南](https://technet.microsoft.com/library/dn280944.aspx)。  
  
## <a name="about-this-guide"></a>有关此指南  
本指南提供建议，以帮助你计划广告 FS，根据你的组织的要求的新部署。 本指南专基础结构专员或系统设计师供使用。 它会突出显示你的主决策点规划广告 FS 部署。 阅读本指南之前，你应该拥有更好地理解广告 FS 功能的级别上的工作方式。 有关详细信息，请参阅[了解关键广告 FS 概念](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)。  
  
## <a name="in-this-guide"></a>本指南中  
  
-   [确定您的广告 FS 部署目标](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [计划你的广告 FS 部署拓扑](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [广告 FS 要求](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>请参阅  
[广告 FS 设计](../../ad-fs/AD-FS-Design.md)  
  

