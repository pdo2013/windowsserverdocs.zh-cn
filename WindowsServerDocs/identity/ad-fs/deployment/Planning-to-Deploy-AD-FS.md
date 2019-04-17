---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: "计划部署广告 FS"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ca9e53d7d98f3ae5e6b7b329e52d4979e8c10215
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="planning-to-deploy-ad-fs"></a>计划部署广告 FS

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012


有关您的环境中收集的信息，并通过遵循的指南中决定上 Active Directory 联合身份验证服务 \(AD FS\) 设计后[广告 FS 设计指南 Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx)，你可以开始规划你的组织的广告 FS 设计部署。 使用已完成的设计和本主题中的信息，你可以决定要执行部署在你的组织的广告 FS 哪些任务。  
  
## <a name="reviewing-your-ad-fs-design"></a>查看你的广告 FS 设计  
构建原始广告 FS 设计团队设计为你的组织与不同的实际实施部署、确保部署团队审核最终的设计，设计团队的部署团队。 查看有关设计以下几点：  
  
-   若要确定联盟服务器，在你的公司的网络或外围网络位置的最佳物理拓扑的设计团队的策略。 部署团队可以参阅有关该主题的文档通过查看广告 FS 设计指南中的以下主题：  
  
    -   [广告 FS 配置数据库的作用](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [放置规划联合身份验证的服务器](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [计划联合身份验证的代理放置服务器](https://technet.microsoft.com/library/dd807130.aspx)  
  
    有可能设计团队可能会使联合身份验证的服务器或联盟服务器代理放置部署团队的主题。 部署团队负责然后记录并实现服务器的物理拓扑。  
  
-   作为索赔提供商、依赖方，或两者的文件的广告 FS 设计范围内你的组织标志业务原因。 确保部署团队的成员，了解部署广告 FS 的原因和其他公司或组织的都参与了联盟合作。 请确保部署团队的成员还理解存在的公司或组织约束 \（有限的硬件、未联网环境和如此 forth\），可能限制某些方面设计的范围。 关于合作伙伴公司的详细信息，请参阅[部署规划](https://technet.microsoft.com/library/dd807083.aspx)。  
  
后设计团队和部署团队达成这些问题，他们可以继续进行部署广告 FS 设计。 有关详细信息，请参阅[实现广告 FS 设计计划](Implementing-Your-AD-FS-Design-Plan.md)。  
