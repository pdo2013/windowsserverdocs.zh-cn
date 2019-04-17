---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: "在 Windows Server 2012 指导广告 FS 设计"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e660c1dabcc5a683fa74068ea148fd4efbeee569
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-design-guide-in-windows-server-2012"></a>在 Windows Server 2012 指导广告 FS 设计

>适用于：Windows Server 2012
  
> [!NOTE]  
> 有关如何部署 Windows Server 2012 R2 中的广告 FS 的信息，请参阅[Windows Server 2012 R2 广告 FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)。  
  
你可以使用 Active Directory® 与 Windows Server 的联合身份验证服务 \(AD FS\)® 2012 年操作系统联合身份验证服务提供商角色无缝用户进行身份验证你的任何 Web\ 基于服务或位于资源合作伙伴公司，而无需管理员，可以创建维护外部信任或林这两个组织的网络之间的信任和而无需登录到第二次用户的应用程序。 访问在其他网络资源时到一个网络身份验证过程，没有重复的登录操作的用户的负担，称为单个 sign\ 上 \(SSO\)。  
  
## <a name="about-this-guide"></a>有关此指南  
本指南提供建议，以帮助你计划广告 FS，根据你的组织的要求的新部署 \（也称为作为部署 goals\ 本指南中）和你想要创建的特定设计。 本指南专基础结构专员或系统设计师供使用。 它会突出显示你的主决策点规划广告 FS 部署。 阅读本指南之前，你应该拥有更好地理解广告 FS 功能的级别上的工作方式。 你还应该在您的广告 FS 设计拥有将反映组织要求更好地理解。  
  
本指南介绍了一套基于三个主要广告 FS 设计，设计的部署目标，并且有助于你确定您的环境的最适合设计。 你可以使用这些部署目标形成以下全面的广告 FS 设计或满足您的环境的自定义设计之一：  
  
-   联盟的 Web SSO 支持 business\ to\ 业务 \(B2B\) 方案，从而支持在与独立林企业单位之间协作  
  
-   SSO 支持在 business\ to\ 消费者 \(B2C\) 方案中客户应用程序访问 web  
  
为每个设计，你将找到用于收集有关您的环境的所需的数据的指南。 你可以使用这些原则规划和设计广告 FS 部署。 请阅读本指南，并完成收集、记录，以及映射要求你的组织后，你将有必要开始部署广告 FS 使用中的指南信息[Windows Server 2012 广告 FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md)。  
  
## <a name="in-this-guide"></a>本指南中  
  
-   [确定您的广告 FS 部署目标](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [广告 FS 设计映射部署目标](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [确定您的广告 FS 部署拓扑](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [计划部署](Planning-Your-Deployment.md)  
  
-   [放置规划联合身份验证的服务器](Planning-Federation-Server-Placement.md)  
  
-   [计划联合身份验证的代理放置服务器](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [规划广告 FS 服务器容量](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [附录 a：检查广告 FS 要求](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

