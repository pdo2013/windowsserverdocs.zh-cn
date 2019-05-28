---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: Windows Server 2012 中的 AD FS 设计指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3f2a6df6a9c9a5cbdfa9c64bc6521e92f4982a15
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191733"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>在 Windows Server 中的 AD FS 设计指南 


  
> [!NOTE]  
> 有关如何部署 Windows Server 2012 R2 中的 AD FS 的信息，请参阅[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)。  
  
可以使用 Active Directory® 联合身份验证服务\(AD FS\)与 Windows Server® 2012年操作系统中的联合身份验证服务提供程序角色进行无缝身份验证用户的任何 Web\-基于服务或驻留在资源伙伴组织中，而无需管理员可以创建或维护外部信任或林信任网络的这两个组织，而无需用户登录的第二个时间之间的应用程序。 访问另一个网络中的资源时对一个网络身份验证的过程，而无需用户重复的登录操作的负担，称为单一登录\-上\(SSO\)。  
  
## <a name="about-this-guide"></a>关于本指南  
本指南提供建议来帮助你规划的新部署的 AD FS 中，根据你的组织的要求\(也称为本指南中的部署目标\)和你想要创建的特定设计。 本指南供基础结构专家或系统架构师使用。 计划 AD FS 部署时，它重点介绍主要决策点。 在阅读本指南之前，应熟悉的功能级别上的 AD FS 工作原理。 此外应在 AD FS 设计中非常了解将反映的组织要求。  
  
本指南介绍了一套基于三个主 AD FS 设计的部署目标，以及它帮助你确定最合适的设计您的环境。 你可以使用这些部署目标构建下列某一个综合的以下 AD FS 设计或自定义设计，以满足您的环境需要：  
  
-   联合 Web SSO 以支持业务\-到\-业务\(B2B\)方案和支持业务部门与独立林之间的协作  
  
-   Web SSO 以支持客户访问应用程序中业务\-到\-使用者\(B2C\)方案  
  
对于每个设计，你可以找到用于收集所需的有关你的环境的数据。 然后可以使用这些准则来规划和设计你的 AD FS 部署。 在阅读本指南并完成收集、 记录和映射你组织的要求后，将具有所需开始部署 AD FS 中的指南信息[Windows Server 2012 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md).  
  
## <a name="in-this-guide"></a>本指南包含的内容  
  
-   [标识 AD FS 部署目标](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [将部署目标映射到 AD FS 设计](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [确定 AD FS 部署拓扑](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [规划部署](Planning-Your-Deployment.md)  
  
-   [规划联合服务器的位置](Planning-Federation-Server-Placement.md)  
  
-   [规划联合服务器代理的位置](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [AD FS 服务器容量规划](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [附录 A：查看 AD FS 要求](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

