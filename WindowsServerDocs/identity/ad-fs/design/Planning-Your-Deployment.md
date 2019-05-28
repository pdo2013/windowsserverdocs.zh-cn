---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: 规划部署
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0206197b24f13d80019cbc864057e99e195ebc4b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191136"
---
# <a name="planning-your-deployment"></a>规划部署

当你规划跨\-组织\(联合身份验证\-基于\)协作使用 Active Directory 联合身份验证服务\(AD FS\)，首先确定你的组织将托管 Web 资源可由其他组织通过 Internet 访问或如果将你的组织中的员工提供对 Web 资源的访问。 此结果会影响如何部署 AD FS 中，和它是在 AD FS 基础结构的规划的基础。  
  
> [!NOTE]  
> 请确保所有各方都清楚地了解组织在联合协议中所扮演的角色。  
  
有关[联合 Web SSO 设计](Federated-Web-SSO-Design.md)，如 AD FS 使用的条款*帐户伙伴*\(也称为*标识提供者*AD FS 管理管理单元\-中\)并*资源伙伴*\(也称为*信赖方*AD FS 管理管理单元\-中\)到帮助区分承载帐户的组织\(帐户伙伴\)从承载 Web 组织\-基于资源\(资源伙伴\)。  
  
在 [Web SSO Design](Web-SSO-Design.md)中，组织同时扮演帐户伙伴和资源伙伴角色，因为它会向其用户提供对其应用程序的访问。  
  
以下主题介绍一些 AD FS 合作伙伴组织概念。 它们还包含 AD FS 部署指南中的主题，包含有关设置和配置帐户伙伴组织和你的 AD FS 部署目标基于资源伙伴组织的信息的链接。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [AD FS 安全规划和部署的最佳做法](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)  
  
-   [规划与 AD FS 1.x 的互操作性](Planning-for-Interoperability-with-AD-FS-1.x.md)  
  
-   [何时使用标识委托](When-to-Use-Identity-Delegation.md)  
  
-   [在帐户伙伴组织中部署 AD FS](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)  
  
-   [在资源伙伴组织中部署 AD FS](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)


