---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: 联合服务器的放置位置
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 376cec7f3a4fb1f988ac5d458b05220c7b9de970
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857688"
---
# <a name="where-to-place-a-federation-server"></a>联合服务器的放置位置

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

作为安全性最佳实践，Active Directory 联合身份验证服务的位置\(AD FS\)联合身份验证服务器位于防火墙前面并连接到企业网络以防止从 Internet 暴露。 这非常重要，因为联合身份验证服务器具有完整授权，可授予安全令牌。 因此，它们应具有与域控制器相同的保护。 如果联合身份验证服务器受到攻击，恶意用户能够为所有 Web 应用程序和受 Active Directory 联合身份验证服务的联合服务器颁发完全访问令牌\(AD FS\)中所有资源合作伙伴组织。  
  
> [!NOTE]  
> 作为安全性最佳做法，请避免直接访问联合身份验证服务器在 Internet 上。 请考虑仅在设置了一个测试实验室环境或你的组织没有外围网络时为你的联合身份验证服务器提供直接 Internet 访问权限。  
  
对于典型企业网络，intranet\-面向防火墙企业网络和外围网络和 Internet 之间建立\-外围网络之间则通常设有面向防火墙和Internet。 在此情况下，联合身份验证服务器位于公司网络，并不直接访问 Internet 的客户端。  
  
> [!NOTE]  
> 连接到公司网络的客户端计算机可以直接使用通过 Windows 集成身份验证的联合身份验证服务器进行通信。  
  
与 AD FS 配置为使用防火墙服务器之前，应在外围网络中放置联合服务器代理。 有关详细信息，请参阅 [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>为联合服务器配置防火墙服务器  
以便联合身份验证服务器可以直接与联合服务器代理进行通信，intranet 防火墙服务器必须配置为允许安全超文本传输协议\(HTTPS\)来自到联合服务器代理的流量联合身份验证服务器中。 这是一项要求，因为 intranet 防火墙服务器必须发布，以便外围网络中的联合身份验证服务器代理可以访问联合身份验证服务器使用端口 443 的联合身份验证服务器。  
  
此外，在 intranet\-面向防火墙服务器，如运行 Internet Security and Acceleration server \(ISA\)服务器，使用该过程称为服务器发布，分发到的 Internet 客户端请求相应公司的联合身份验证服务器。 这意味着，必须手动运行发布聚集联合身份验证服务器的 URL，例如，http 的 ISA 服务器的 intranet 服务器上创建服务器发布规则：\/\/fs.fabrikam.com。  
  
有关如何在外围网络中配置服务器发布的详细信息，请参阅 [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md)。 有关如何配置 ISA 服务器以发布服务器的信息，请参阅[创建安全的 Web 发布规则](https://go.microsoft.com/fwlink/?LinkId=75182)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
