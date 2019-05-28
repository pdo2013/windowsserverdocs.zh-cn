---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: 确定资源伙伴中的联合应用程序策略
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 15a6c095648795badfae6f68f1ba2f9270219db8
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191460"
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>确定资源伙伴中的联合应用程序策略

设计新的 Active Directory 联合身份验证服务的重要组成部分\(AD FS\)资源伙伴组织中的基础结构确定您的应用程序和服务将使用参与的完整集联合身份验证和哪些帐户的合作伙伴将这些资源的收件人。 设计联合应用程序和服务策略之前，请考虑以下问题：  
  
-   将会启用和部署 ASP.NET 应用程序或 Windows Communication Foundation \(WCF\)服务进行联合身份验证？  
  
-   公司网络上的用户是否需要通过 Windows 集成身份验证访问联合应用程序或服务？  
  
-   联合应用程序或服务是否会由外围网络中的用户使用？ 如果是这样，是否需要 Windows 集成身份验证？  
  
-   是承载联合应用程序运行的是 Windows Server 操作系统和 Internet Information Services Web 服务器的所有\(IIS\)？  
  
-   联合应用程序或服务为谁提供资源？  
  
回答这些问题将帮助你规划坚实的 AD FS 设计。 它还可帮助创建具有成本效益且在资源方面十分高效的联合应用程序和服务策略。 有关为你的组织设计最合适的联合应用程序和服务策略的详细信息，请参阅本指南中的以下主题：  
  
-   [为声明感知应用程序和服务提供 Active Directory 用户访问权限](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [为其他组织的应用程序和服务提供 Active Directory 用户访问权限](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [为另一个组织中的用户提供对声明感知应用程序和服务的访问权限](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
详细了解如何创建声明\-识别的 ASP.NET 应用程序或 WCF 服务，请参阅[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

