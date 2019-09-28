---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: 规划部署 AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ed2082706975d58a1535aaeb61e6c5283d23306a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359499"
---
# <a name="planning-to-deploy-ad-fs"></a>规划部署 AD FS


收集有关环境的信息，并按照[Windows Server 2012 中的 AD FS 设计指南](https://technet.microsoft.com/library/dd807036.aspx)中的指南决定 Active Directory 联合身份验证服务 \(AD FS @ no__t 设计后，可以开始计划部署组织的 AD FS 设计。 通过本主题中的完整设计和信息，你可以确定要执行哪些任务来在你的组织中部署 AD FS。  
  
## <a name="reviewing-your-ad-fs-design"></a>审查你的 AD FS 设计  
如果为你的组织构造原始 AD FS 设计的设计团队与将实际实现部署的部署团队不同，请确保部署团队与设计团队一起审查最终设计。 请查看有关设计的以下几点：  
  
-   设计团队的策略，用来确定在企业网络或外围网络中的联合服务器位置的最佳物理拓扑。 部署团队可以通过查看 AD FS 设计指南中的下列主题来参考有关该主题的文档：  
  
    -   [AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [规划联合服务器的位置](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [规划联合服务器代理的位置](https://technet.microsoft.com/library/dd807130.aspx)  
  
    设计团队很可能将联合服务器或联合服务器代理位置的主题留给部署团队。 然后，部署团队将负责编写文档并实现服务器的物理拓扑。  
  
-   你组织的任命（作为声明提供方、信赖方或两者兼具）的业务原因，在记录的 AD FS 设计范围之内。 确保部署团队成员了解部署 AD FS 的原因，以及联合合作关系中涉及的其他公司或组织。 确保部署团队成员还了解对其他公司或组织存在的约束，@no__t 0limited 硬件、无 extranet 环境等，@ no__t-1，它们可能会以某种方式限制设计的范围。 有关伙伴组织的详细信息，请参阅[规划部署](https://technet.microsoft.com/library/dd807083.aspx)。  
  
设计团队和部署团队在这些问题上达成一致后，他们可以继续部署 AD FS 的设计。 有关详细信息，请参阅[实现你的 AD FS 设计规划](Implementing-Your-AD-FS-Design-Plan.md)。  
