---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: 实现 AD FS 设计规划
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6150b52030734c57b345aea731302650bcbddbfd
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192135"
---
# <a name="implementing-your-ad-fs-design-plan"></a>实现 AD FS 设计规划

以下环境的条件和要求是在 Active Directory 联合身份验证服务的实现中的重要因素\(AD FS\)设计规划：  
  
-   **受支持的合作伙伴：** 您通常使用 AD FS 与合作伙伴组织工作。 若要建立联合身份验证，请确定想要形成合作关系的组织。 基线 AD FS 部署准备就绪后，与合作伙伴操作包括添加合作伙伴、 删除合作伙伴，以及更新合作伙伴信息。 对合作关系的更改可能会发生的原因有多种。 例如，AD FS 部署可能需要合作关系更新，如果你的合作伙伴发生重大改变其业务、 你的组织变得更大的组织或组织的联合身份验证的一部分或你的组织获取由不同公司。 在多个域中的身份联合中的任何方案中，您需要知道域\(合作伙伴\)目前所支持的和表示潜在的合作伙伴的所有其他域。  
  
-   **支持的应用程序和服务类型：** 某些应用程序和服务需要访问操作系统资源，而有些则是"声明感知。" 请务必了解应用程序和服务的 AD FS 支持，这样你就可以构建管理要求的类型。  
  
-   **逻辑和物理体系结构关系图或部署拓扑：** 您需要知道：  
  
    -   是否联合身份验证服务器将以一组的场服务器或单个服务器上工作。  
  
    -   其中您的网络部署防火墙和代理服务器。  
  
    -   资源和用户是否访问从外部和 / 或组织，组织中的资源的位置。  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>如何实现您使用本指南的 AD FS 设计  
实现设计的下一步是确定必须以何种顺序执行每个部署任务。 本指南使用清单来帮助你逐步完成实现设计规划所需的各种服务器和应用程序部署任务。 使用父级和子级清单根据需要用于表示必须处理的任务特定的 AD FS 设计的顺序。  
  
在本部分的指南中使用以下父清单熟悉用于实现组织的首选的 AD FS 设计的部署任务：  
  
-   [清单：实现 Web SSO 设计](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [清单：实现联合 Web SSO 设计](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
