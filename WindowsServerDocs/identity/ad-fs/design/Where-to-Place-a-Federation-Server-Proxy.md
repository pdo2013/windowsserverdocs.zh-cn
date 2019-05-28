---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: 联合服务器代理放置位置
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9cc40920d366c973ace06a0b6d438a1c2d84b03e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190508"
---
# <a name="where-to-place-a-federation-server-proxy"></a>联合服务器代理放置位置

您可以放置 Active Directory 联合身份验证服务\(AD FS\)联合服务器代理在外围网络中以提供针对可能来自 Internet 的恶意用户的保护层。 联合服务器代理不能访问用于创建令牌的私钥，因此非常适合外围网络环境。 但是，联合服务器代理可以有效地将传入请求路由到联合身份验证服务器有权生成这些令牌。  
  
不需要将企业网络内部联合服务器代理放在帐户伙伴或资源伙伴，因为连接到公司网络的客户端计算机可以直接与联合身份验证服务器进行通信。 在此方案中，联合身份验证服务器还为来自企业网络的客户端计算机提供联合身份验证服务器代理功能。  
  
这通常使用外围网络，intranet\-面向防火墙的外围网络和企业网络和 Internet 之间建立\-外围网络之间则通常设有面向防火墙和Internet。 在此方案中，联合服务器代理位于这两个外围网络上的这些防火墙之间。  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>配置联合服务器代理的防火墙服务器  
有关联合服务器代理重定向过程成功，必须配置所有防火墙服务器为允许安全超文本传输协议\(HTTPS\)流量。 需要使用 HTTPS，因为防火墙服务器必须发布，以便外围网络中的联合身份验证服务器代理可以访问公司网络中的联合身份验证服务器使用端口 443，联合服务器代理。  
  
> [!NOTE]  
> 与客户端计算机之间的所有通信往来也通过 HTTPS 进行。  
  
此外，Internet\-面向防火墙服务器，例如一台运行 Microsoft Internet Security 和 Acceleration \(ISA\)服务器，使用该过程称为服务器发布，分发 Internet 客户端在适当的外围和企业网络服务器，如联合服务器代理或联合身份验证服务器的请求。  
  
服务器发布规则用于确定服务器发布的工作原理，即筛选通过 ISA 服务器计算机的所有传入和传出请求。 服务器发布规则将传入客户端请求映射到 ISA 服务器计算机后的相应服务器。 有关如何配置 ISA 服务器以发布服务器的信息，请参阅[创建安全的 Web 发布规则](https://go.microsoft.com/fwlink/?LinkId=75182)。  
  
在 AD FS 的联合世界中，这些客户端请求通常会向特定的 URL，例如，联合身份验证服务器标识符 URL 如 http://fs.fabrikam.com。 因为这些客户端请求会在从 Internet Internet\-面向防火墙服务器必须配置为在外围网络中部署每个联合服务器代理为发布的联合身份验证服务器标识符 URL。  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>配置 ISA 服务器为允许使用 SSL  
为了帮助实现 AD FS 的安全通信，必须配置 ISA 服务器以允许安全套接字层\(SSL\)以下之间的通信：  
  
-   **联合身份验证服务器和联合身份验证服务器代理。** 联合身份验证服务器和联合身份验证服务器代理之间的所有通信需要 SSL 通道。 因此，必须配置 ISA 服务器为允许在企业网络和外围网络之间建立 SSL 连接。  
  
-   **客户端计算机、 联合身份验证服务器和联合服务器代理。** 以便客户端计算机和联合身份验证服务器之间或客户端计算机和联合服务器代理之间可以进行通信，您可以将运行 ISA Server 前面的联合身份验证服务器或联合服务器代理的计算机。  
  
    如果在将运行 ISA Server 前面的联合身份验证服务器或联合服务器代理的计算机，你的组织联合身份验证服务器或联合服务器代理上执行 SSL 客户端身份验证，必须将服务器配置为传递\-通过的 SSL 连接，因为在联合身份验证服务器或联合服务器代理必须终止 SSL 连接。  
  
    如果你的组织不在联合身份验证服务器或联合服务器代理上执行 SSL 客户端身份验证，另一选项是终止 SSL 连接在计算机上运行 ISA Server，然后再重新\-建立 SSL 连接到联合服务器或联合服务器代理。  
  
> [!NOTE]  
> 联合身份验证服务器或联合服务器代理需要通过 SSL 来保护安全令牌的内容进行保护的连接。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
