---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: 联合服务器代理容量规划
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: eedb0f2ae4b6f600eb578c5db857cc1d79bccbd1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407984"
---
# <a name="planning-for-federation-server-proxy-capacity"></a>联合服务器代理容量规划

联合服务器代理的容量规划有助于您估算：  
  
-   每个联合服务器代理的相应硬件要求。  
  
-   要放在每个组织中的联合服务器和联合服务器代理的数量。  
  
联合服务器代理将安全令牌从企业网络中受保护的联合服务器重定向到联合用户。 部署联合服务器代理的目的是允许外部用户连接到联合服务器。 它不会对令牌进行实际签名，也不会在 AD FS 配置数据库中写入数据。 因此，联合服务器代理的硬件要求通常低于联合服务器的硬件要求。  
  
由于对联合服务器代理的每个请求都会导致向联合服务器或联合服务器场发出请求，因此必须并行执行联合服务器和联合服务器代理的容量规划。  
  
为联合服务器代理估计每秒的峰值符号 @ no__t-0ins 需要了解将通过联合服务器代理登录的联合用户的使用模式。 在许多部署中，使用联合服务器代理登录的联合用户位于 Internet 上。 可以通过在 AD FS 将保护的现有 Web 应用程序上查看这些联合用户的使用模式，来估算每秒的峰值符号 @ no__t。  
  
> [!NOTE]  
> 对于生产部署，我们建议为每个部署的联合服务器场实例至少使用两个联合服务器代理。  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>估计组织所需的联合服务器代理的数目  
你首先需要确定要在组织中部署的联合服务器的总数，然后才能估算所需的 AD FS 联合服务器代理计算机的数量。 有关如何执行此操作的详细信息，请参阅[规划联合服务器容量](Planning-for-Federation-Server-Capacity.md)。  
  
确定联合服务器的数目后，请将此数量的服务器乘以你预计将从企业网络 @ no__t-1 之外的外部用户 @no__t 的传入联合身份验证请求所占的百分比。 此计算的值将向你提供要处理外部用户的传入身份验证请求的预计联合服务器代理数。  
  
例如，如果建议的联合服务器数为3，并且预计将从外部用户发出的身份验证请求总数大约为联合身份验证请求总数的 60%，则计算结果将等于 1.8 \(3 X .60 @ no__t-1，可以向上舍入到2。  因此，在这种情况下，需要部署两个联合服务器代理计算机，以容纳三个联合服务器的外部用户身份验证请求的负载。  
  
在 AD FS 产品团队执行的测试中，发现每个联合服务器代理上的总体 CPU 利用率明显低于同一场的联合服务器上观察到的 CPU 使用率。  在一个测试中，当一台联合服务器 CPU 指示它已完全饱和时，为同一场提供代理服务的联合服务器代理的 CPU 的使用率仅为 20%。 因此，我们的测试表明，联合服务器代理的 CPU 上的负载（使用本部分前面所述的类似硬件规范）可以合理地处理大约三台联合服务器的处理负载。  
  
但出于容错目的，我们建议为每个部署的联合服务器场至少使用两个联合服务器代理。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
