---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: 规划部署 AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ca9e53d7d98f3ae5e6b7b329e52d4979e8c10215
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831688"
---
# <a name="planning-to-deploy-ad-fs"></a>规划部署 AD FS

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012


在收集有关您的环境信息并确定 Active Directory 联合身份验证服务后\(AD FS\)中的指南的设计[Windows Server 2012 中 AD FS 设计指南](https://technet.microsoft.com/library/dd807036.aspx)，你可以开始规划部署你组织的 AD FS 设计。 使用已完成的设计和本主题中的信息，您可以确定由哪些任务执行，以部署你的组织中的 AD FS。  
  
## <a name="reviewing-your-ad-fs-design"></a>审查你的 AD FS 设计  
如果构造原始 AD FS 设计团队设计为你的组织与将实际实现部署，请确保部署团队审查最终设计与设计团队的部署团队不同。 请查看有关设计的以下几点：  
  
-   设计团队的策略，用来确定在企业网络或外围网络中的联合服务器位置的最佳物理拓扑。 部署团队可以通过查看 AD FS 设计指南中的下列主题来参考有关此主题的文档：  
  
    -   [AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [规划联合服务器的位置](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [规划联合服务器代理的位置](https://technet.microsoft.com/library/dd807130.aspx)  
  
    设计团队很可能将联合服务器或联合服务器代理位置的主题留给部署团队。 然后，部署团队将负责编写文档并实现服务器的物理拓扑。  
  
-   你组织的任命（作为声明提供方、信赖方或两者兼具）的业务原因，在记录的 AD FS 设计范围之内。 确保部署团队成员了解部署 AD FS 的原因和其他公司或组织的是联合合作关系中涉及。 确保部署团队成员还了解存在的其他公司或组织的约束\(有限的硬件、 没有 extranet 环境等\)，可能会限制以某种方式设计的作用域。 有关伙伴组织的详细信息，请参阅[规划部署](https://technet.microsoft.com/library/dd807083.aspx)。  
  
之后设计团队和部署团队达成这些问题，他们可以继续部署 AD FS 设计。 有关详细信息，请参阅[实现你的 AD FS 设计规划](Implementing-Your-AD-FS-Design-Plan.md)。  
