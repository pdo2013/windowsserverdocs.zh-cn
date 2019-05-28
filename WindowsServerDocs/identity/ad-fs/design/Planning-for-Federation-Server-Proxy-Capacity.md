---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: 联合服务器代理容量规划
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c3efbb4081336ebfdfe9d3ab8a2b91412aa82dee
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191076"
---
# <a name="planning-for-federation-server-proxy-capacity"></a>联合服务器代理容量规划

容量规划联合服务器代理可帮助你估计：  
  
-   每个联合服务器代理的合适的硬件要求。  
  
-   联合身份验证服务器和联合服务器代理要放置在每个组织中的数量。  
  
联合服务器代理重定向到联合用户从公司网络中的受保护的联合身份验证服务器的安全令牌。 部署联合服务器代理的目的是允许外部用户连接到联合身份验证服务器。 它实际上不会执行令牌签名或写入到的 AD FS 配置数据库中的数据。 因此，联合服务器代理的硬件要求是通常低于联合身份验证服务器的硬件要求。  
  
为联合服务器代理的每个请求都会请求到联合身份验证服务器或联合服务器场，因为必须并行执行的联合身份验证服务器和联合身份验证服务器代理容量规划。  
  
估计峰值登录\-联合服务器代理的每秒的项，需要通过联合身份验证服务器代理将在登录的联合用户的使用模式的了解。 在许多部署中，使用联合服务器代理登录的联合的用户位于 Internet 上。 你可以估计峰值登录\-每秒通过查看它们的使用模式的项的联合用户将受 AD FS 的现有 Web 应用程序。  
  
> [!NOTE]  
> 对于生产部署，我们建议在部署每个联合服务器场实例的两个联合服务器代理的最小值。  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>估计所需的组织的联合服务器代理数  
可以估计数之前需要的 AD FS 联合服务器代理计算机，你首先需要确定将在你的组织中部署的联合身份验证服务器的总数。 有关如何执行此操作的详细信息，请参阅[联合身份验证服务器容量规划](Planning-for-Federation-Server-Capacity.md)。  
  
一旦您已决定联合身份验证服务器的数量乘以此数量的情况下的传入的联合身份验证的百分比，服务器请求预期将从外部用户进行\(位于企业网络之外\). 此计算的值将为您使用的联合服务器代理将处理传入的身份验证请求，为外部用户的估计数。  
  
例如，如果建议的联合身份验证服务器数量为 3，并预计，将从外部用户进行身份验证请求的总数将是大约 60%的联合身份验证请求的总数在计算就等于 1.8 \(3 X.60\)的舍入最多 2 个。  因此，在这种情况下，您需要部署两个联合服务器代理计算机以适应三个联合服务器的外部用户身份验证请求的负载。  
  
在测试中执行的 AD FS 产品团队，每个联合服务器代理上的总体 CPU 使用率已发现显著低于观察到同一个场的联合身份验证服务器的 CPU 使用率。  在一个测试中，而一个联合身份验证服务器 CPU 时，该值指示已完全达到饱和，提供该同一个场的代理服务的联合身份验证服务器代理的 CPU 观察到以只有 20%的利用率。 因此，我们的测试表明，联合服务器代理，它使用类似的硬件规范所述前面在本部分中，在 CPU 上的负载可以合理地处理大约三个联合身份验证服务器的处理负载。  
  
但是，出于容错目的，我们建议你部署的每个联合服务器场的两个联合服务器代理的最小值。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
