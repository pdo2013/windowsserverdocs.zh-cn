---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: "放置联合服务器的位置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 376cec7f3a4fb1f988ac5d458b05220c7b9de970
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="where-to-place-a-federation-server"></a>放置联合服务器的位置

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

作为安全性最佳实践，位置放防火墙 Active Directory 联合身份验证服务 \(AD FS\) 联盟服务器，将其连接到你的企业网络，以防止曝光度从 Internet。 由于联合身份验证的服务器具有完整授权，以使具有安全标记，这很重要。 因此，这些应有域控制器为相同的保护。 联合服务器受到威胁后，如果恶意用户将具有能够向所有 Web 应用程序以及受 Active Directory 联合身份验证服务 \(AD FS\) 所有资源合作伙伴组织中的联合服务器发送标记的完全访问权限。  
  
> [!NOTE]  
> 有价证券作为最佳做法，避免在 Internet 上有直接访问您联合身份验证的服务器。 请考虑设置的测试实验环境向上或你的组织中没有外围网络时，仅提供联盟服务器直接 Internet 访问。  
  
对于典型公司的网络，intranet\ 面向防火墙建立外围网络，公司网络之间，并且通常外围网络和 Internet 之间建立 Internet \-facing 防火墙。 在此情况下，联合身份验证的服务器内的公司的网络，且不 Internet 客户端直接访问。  
  
> [!NOTE]  
> 连接到公司的网络的客户端计算机可以直接与 Windows 的集成身份验证通过联盟服务器通信。  
  
联合身份验证的服务器代理应位于外围网络配置为使用你防火墙服务器与广告 FS 之前。 有关详细信息，请参阅[放置联合身份验证的服务器代理](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>配置防火墙服务器联盟服务器  
以便联合身份验证的服务器可以直接与联合身份验证的服务器代理通信，必须配置 intranet 防火墙服务器以允许安全超文本传输协议 \(HTTPS\) 交通从联合身份验证的服务器代理联合身份验证的服务器。 这是一个要求，因为防火墙 intranet 服务器必须发布联盟服务器，以便在外围网络联合 server 代理可以访问联盟服务器使用 443 端口。  
  
此外，intranet\ 面向防火墙服务器上，如 Internet 安全和加速运行的服务器 \(ISA\) 服务器，用于此过程称为服务器发布分发到相应部门联合身份验证的服务器 Internet 客户端请求。 这意味着您必须手动创建服务器发布规则运行发布聚集的联盟服务器的 URL，例如，http:///\/fs.fabrikam.com ISA 服务器的 intranet 服务器上。  
  
有关如何配置服务器发布外围网络中的详细信息，请参阅[放置联合身份验证的服务器代理](Where-to-Place-a-Federation-Server-Proxy.md)。 有关如何配置 ISA 服务器发布服务器信息，请参阅[创建安全的 Web 发布规则](https://go.microsoft.com/fwlink/?LinkId=75182)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
