---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: "放置联合身份验证的服务器代理的位置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4bde30f694c6490962edaa0c3fe1543e74ba7fd7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="where-to-place-a-federation-server-proxy"></a>放置联合身份验证的服务器代理的位置

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在外围网络提供的保护层抵御恶意来自 Internet 的用户，你可以放置服务器 Active Directory 联合身份验证服务 \(AD FS\) 联合身份验证的代理服务器。 因为没有访问用于创建标记的专用键联合身份验证的服务器代理非常适合外围网络环境。 但是，联合身份验证的服务器代理可以高效到获得授权，可生成这些标记的联合身份验证的服务器发送传入的请求。  
  
不需要帐户合作伙伴或资源合作伙伴放置联合服务器代理内企业网络，因为连接到公司的网络的客户端计算机进行通信直接与联合身份验证的服务器。 在此情况下，联合服务器还提供了来自于公司的网络的客户端计算机联盟服务器代理功能。  
  
按原样典型与外围网络，外围网络之间的企业网络，建立 intranet\ 面向防火墙，并且通常外围网络和 Internet 之间建立 Internet \-facing 防火墙。 在此情况下，联合身份验证的服务器代理位于之间这两个这些外围网络上的防火墙。  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>为联合身份验证的服务器代理配置你的防火墙服务器  
联盟服务器代理重定向进程的成功，必须配置所有防火墙服务器允许安全超文本传输协议 \(HTTPS\) 通信。 防火墙服务器必须发布联合身份验证的服务器代理，使用端口 443，以便联合 server 代理外围网络中的可以访问在公司的网络的联合服务器在于需要使用 HTTPS。  
  
> [!NOTE]  
> 客户端计算机的所有通信，也都发生通过 HTTPS。  
  
此外，Internet \-facing 防火墙服务器上，如的计算机运行 Microsoft Internet 安全和加速 \(ISA\) 服务器，用于此过程称为服务器发布分发 Internet 客户端请求相应外围和企业网络服务器，如联合身份验证的服务器代理或联合身份验证的服务器。  
  
服务器发布规则确定服务器发布的工作方式，即筛选通过 ISA 服务器计算机的所有传入和传出请求。 服务器发布规则映射到背后 ISA 服务器计算机的相应服务器传入的客户端请求。 有关如何配置 ISA 服务器发布服务器信息，请参阅[创建安全的 Web 发布规则](https://go.microsoft.com/fwlink/?LinkId=75182)。  
  
广告 FS 联盟世界中，在这些客户端请求通常进行到特定的 URL，例如，一个联盟服务器标识符 URL http://fs.fabrikam.com 等。因为这些客户端请求推出从 Internet 时，Internet \-facing 防火墙服务器必须配置上发布每个联盟联盟服务器标识符 URL 服务器部署在外围网络的代理。  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>配置 ISA 允许 SSL 服务器  
为了便于安全广告 FS 通信，必须配置 ISA 服务器允许之间以下安全套接字层 \(SSL\) 通信：  
  
-   **联合身份验证的服务器和联合服务器代理。** SSL 通道都必须联合身份验证的服务器和联盟服务器代理之间的所有通信。 因此，你必须配置 ISA 服务器，以便之间公司的网络，外围 SSL 连接。  
  
-   **客户端计算机、联合身份验证的服务器和联合身份验证的服务器代理。** 以便之间客户端计算机和联合身份验证的服务器或联合身份验证的服务器代理客户端计算机之间进行通信，您可以将计算机运行放联合身份验证的服务器或联合身份验证的服务器代理服务器 ISA。  
  
    如果你的组织联合身份验证的服务器或联合身份验证的服务器代理执行 SSL 客户身份验证，放置运行 ISA 服务器放联合身份验证的服务器或联合身份验证的服务器代理计算机时，必须为配置服务器 pass\ 取 SSL 连接因为 SSL 连接必须在联合身份验证的服务器或联合身份验证的服务器代理终止。  
  
    如果你的组织不联合身份验证的服务器或联合身份验证的服务器代理执行 SSL 客户身份验证，其他选项是终止在计算机上运行的服务器 ISA，然后 re\ SSL 连接-建立 SSL 连接到的联合身份验证的服务器或服务器联合身份验证的代理。  
  
> [!NOTE]  
> 联合身份验证的服务器或联合身份验证的服务器代理需要连接受 SSL 保护安全令牌的内容。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
