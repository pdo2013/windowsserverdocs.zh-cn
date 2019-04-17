---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: "规划联合身份验证的服务器代理容量"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e57f34b173c10e9e753c7f3b8dcd88d7bf6742c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-federation-server-proxy-capacity"></a>规划联合身份验证的服务器代理容量

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

容量计划为服务器联合身份验证的代理可帮助你评估：  
  
-   每个联合身份验证的服务器代理服务器的相应的硬件要求。  
  
-   联合身份验证的服务器和联合放置在每个组织的服务器代理数。  
  
联合身份验证的服务器代理重定向到联盟用户的企业网络中的受保护的联合服务器的安全标记。 部署联合身份验证的服务器代理服务器的目的是允许外部用户连接到的联合身份验证的服务器。 它不是实际上不登录标记或写入广告 FS 配置数据库中的数据。 因此，联合身份验证的服务器代理服务器硬件要求都通常低于联合服务器硬件要求。  
  
每个请求联合身份验证的服务器代理到结果请求中向联合服务器或联盟服务器电场的日落，因为必须并行执行规划联合身份验证的服务器和联盟服务器代理容量。  
  
评估峰值 sign\ 接每秒联合身份验证的服务器代理需要了解将通过联合身份验证的服务器代理登录联盟用户的使用模式。 许多部署，联盟使用联合身份验证的服务器代理登录的用户均位于 Internet。 你可以通过现有的 Web 应用程序受广告 FS 上查看这些联盟用户的使用模式估计峰值 sign\ 接每秒。  
  
> [!NOTE]  
> 生产部署，我们建议你部署的每个联合身份验证的服务器电场的日落实例的两个联盟服务器代理服务器最少。  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>估计联合所需的你的组织的服务器代理数  
可以估算数之前所需广告 FS 联盟服务器代理计算机，首先需要确定总数联合身份验证的服务器，你将在你的组织中部署。 有关如何执行此操作的详细信息，请参阅[规划联合身份验证的服务器容量](Planning-for-Federation-Server-Capacity.md)。  
  
一旦决定数量的联合身份验证的服务器上乘传入联合身份验证的百分比服务器此数请求你预期会进行外部用户从 \（位于之外公司 network\）。 此计算值将提供你会如何处理你外部用户传入的身份验证请求的联合 server 代理预计号码。  
  
例如，如果推荐的联盟服务器数是 3 平板电脑，并且希望将由外部用户的身份验证请求总数将大约 60%的联合身份验证请求总数，你计算就等于 1.8 \ (3 X。60\) 其中可以轮最多 2。  因此，在此情况下，你需要部署两个联盟服务器代理计算机以适应外部用户身份验证的请求三个联合身份验证的服务器加载。  
  
在测试由广告 FS 产品团队中，在每个联合身份验证的服务器代理整体的 CPU 使用率找到要显著低于观察到同一场联合身份验证的服务器的 CPU 使用率。  一个测试，请在一个联盟服务器 CPU，它完全饱满，用于指示时提供该相同场代理服务联盟服务器代理服务器的 CPU 观察到利用率仅为 20%。 因此，我们的测试表明，使用类似的硬件规格所述早些时候在此部分中，联盟服务器代理服务器 CPU 负载合理可能处理处理负载大约三个联合身份验证的服务器。  
  
但是，为了能力故障，我们建议你部署每个联盟服务器农场里的两个联盟服务器代理服务器最少。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
