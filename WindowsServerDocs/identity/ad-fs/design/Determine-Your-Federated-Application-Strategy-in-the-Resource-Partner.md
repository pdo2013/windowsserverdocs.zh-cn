---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: "确定你联合应用程序策略资源伙伴中"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aca47658cc5a20f63dbd59a26ebe135dd04def92
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>确定你联合应用程序策略资源伙伴中

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

重要组成部分设计资源合作伙伴组织中的新 Active Directory 联合身份验证服务 \(AD FS\) 基础结构确定你全套应用程序和服务将用于参与联盟和哪个帐户合作伙伴将这些资源收件人。 你一种联合应用程序和服务策略设计之前，请考虑以下问题：  
  
-   将会启用和部署 ASP.NET 应用程序或窗口通信基础 \(WCF\) 服务联盟？  
  
-   将你的企业网络上的用户需要对联合应用程序或通过 Windows 的集成身份验证服务的访问权限？  
  
-   将联合应用程序或服务使用通过外围网络中的用户？ 如果是这样，，将需要先集成身份验证的 Windows？  
  
-   是否的 Web 服务器所有运行 Windows Server 操作系统和 Internet 信息服务 \(IIS\) 该主机联合应用？  
  
-   谁会联合应用程序或服务提供的资源？  
  
回答这些问题将帮助你在规划稳定的广告 FS 设计。 它还将帮助您在创建联合应用程序和服务是成本效益的策略和资源高效。 有关设计最适合联合应用程序和服务策略针对你的组织的详细信息，请参阅本指南中的以下主题：  
  
-   [为你识别索赔应用程序和服务提供你 Active Directory 用户访问](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [提供您 Active Directory 用户的访问权限的应用程序和其他公司的服务](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [提供您的索赔识别的应用程序和服务中的用户另一个组织访问](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
有关如何创建 claims\ 感知 ASP.NET 应用程序或 WCF 服务的详细信息，请参阅[Windows 身份基础 SDK](https://go.microsoft.com/fwlink/?LinkId=122266)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)

