---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: 联合服务器代理放置位置
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 73e68d03e4f2f76dbaf4a497da551640476d0438
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407826"
---
# <a name="where-to-place-a-federation-server-proxy"></a>联合服务器代理放置位置

可以将 Active Directory 联合身份验证服务 \(AD FS @ no__t-1federation 服务器代理放在外围网络中，以针对可能来自 Internet 的恶意用户提供保护层。 联合服务器代理不能访问用于创建令牌的私钥，因此非常适合外围网络环境。 但是，联合服务器代理可以将传入请求高效路由到有权生成这些令牌的联合服务器。  
  
不需要将联合服务器代理放在帐户伙伴或资源伙伴的企业网络内，因为连接到企业网络的客户端计算机可以与联合服务器直接通信。 在此方案中，联合服务器还为来自企业网络的客户端计算机提供联合服务器代理功能。  
  
与外围网络相同的典型情况是，外围网络和企业网络之间建立了 intranet @ no__t-0facing 防火墙，Internet @ no__t-1facing 防火墙通常在外围网络和 Internet 之间建立。 在此方案中，联合服务器代理位于外围网络中的这两个防火墙之间。  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>配置联合服务器代理的防火墙服务器  
为了使联合服务器代理重定向过程成功，必须将所有防火墙服务器配置为允许安全超文本传输协议 \(HTTPS @ no__t 通信。 需要使用 HTTPS，因为防火墙服务器必须使用端口443发布联合服务器代理，以便外围网络中的联合服务器代理可以访问企业网络中的联合服务器。  
  
> [!NOTE]  
> 与客户端计算机之间的所有通信往来也通过 HTTPS 进行。  
  
此外，Internet @ no__t-0facing 防火墙服务器（例如运行 Microsoft Internet Security 和加速 \(ISA @ no__t 服务器的计算机）使用称为服务器发布的过程将 Internet 客户端请求分发到适当的外围网络服务器和企业网络服务器，如联合服务器代理或联合服务器。  
  
服务器发布规则用于确定服务器发布的工作原理，即筛选通过 ISA 服务器计算机的所有传入和传出请求。 服务器发布规则将传入客户端请求映射到 ISA 服务器计算机后的相应服务器。 有关如何配置 ISA 服务器以发布服务器的信息，请参阅[创建安全的 Web 发布规则](https://go.microsoft.com/fwlink/?LinkId=75182)。  
  
在 AD FS 的联合世界中，这些客户端请求通常会向特定的 URL，例如，联合身份验证服务器的标识符 URL，如 http:\//fs.fabrikam.com 。 由于这些客户端请求来自 Internet，因此必须将 Internet @ no__t-0facing firewall 服务器配置为发布外围网络中部署的每个联合服务器代理的联合服务器标识符 URL。  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>配置 ISA 服务器为允许使用 SSL  
为了便于安全 AD FS 通信，必须配置 ISA 服务器以允许在以下各项之间进行安全套接字层 \(SSL @ no__t 通信：  
  
-   **联合服务器和联合服务器代理。** 联合服务器和联合服务器代理之间的所有通信都需要 SSL 通道。 因此，必须配置 ISA 服务器为允许在企业网络和外围网络之间建立 SSL 连接。  
  
-   **客户端计算机、联合服务器和联合服务器代理。** 为了在客户端计算机与联合服务器之间或客户端计算机与联合服务器代理之间进行通信，你可以将运行 ISA 服务器的计算机放在联合服务器或联合服务器代理的前面。  
  
    如果你的组织在联合服务器或联合服务器代理上执行 SSL 客户端身份验证，则将运行 ISA 服务器的计算机放在联合服务器或联合服务器代理的前面时，必须将该服务器配置为 pass @ no__0through SSL 连接，因为 SSL 连接必须在联合服务器或联合服务器代理上终止。  
  
    如果你的组织不在联合服务器或联合服务器代理上执行 SSL 客户端身份验证，则附加选项是在运行 ISA 服务器的计算机上终止 SSL 连接，然后将 no__t-0establish a SSL 连接重新连接到联合服务器或联合服务器代理。  
  
> [!NOTE]  
> 联合服务器或联合服务器代理要求连接受 SSL 保护，以保护安全令牌的内容。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
