---
title: 核心网络助理指南
description: 本主题提供 Windows Server 2016 核心网络指南的助理指南的概述
manager: brianlic
ms.technology: networking
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b757e1914ee263a041f39e9767d3cb8af38403dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816798"
---
# <a name="core-network-companion-guidance"></a>核心网络助理指南

>适用于：Windows 服务器 （半年频道），Windows Server 2016

尽管 Windows Server 2016[核心网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)说明了如何部署新的 Active Directory&reg;具有新的根级域和支持的网络基础结构，助理指南林提供能够将功能添加到你的网络。

在你部署完核心网络之后，每个助理指南都允许你完成一个具体的目标。 有些情况下，有多个助理指南，当它们一起部署并且顺序正确的时候，你可以用量化、合算、合理的方式实现非常复杂的目标。

如果你在见到“核心网络指南”之前部署你的 Active Directory 域和核心网络，你仍然可以使用“助理指南”向网络添加功能。 简单地使用“核心网络指南”作为先决条件列表，并且知道，要使用“助理指南”部署附加功能，你的网络必须满足“核心网络指南”提供的先决条件。

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>核心网络助理指南：为 802.1X 有线和无线部署部署服务器证书 

此助理指南解释了如何构建在核心网络时通过部署用于运行网络策略服务器的计算机的服务器证书\(NPS\)，远程访问服务\(RAS\)，和 / 或。

部署基于证书的身份验证方法使用可扩展身份验证协议时，服务器证书所需\(EAP\)和受保护的 EAP \(PEAP\)进行网络访问身份验证。 部署与 Active Directory 证书服务的服务器证书\(AD CS\) EAP 和 PEAP 的基于证书的身份验证方法提供以下优势：

- 绑定到私钥的 NPS 或 RAS 服务器的标识
- 自动注册证书向域成员 NPS 和 RAS 服务器经济高效且安全的方法
- 是管理证书和证书颁发机构的有效方法
- 基于证书的身份验证所提供的安全性
- 为其他目的扩展证书用途的功能
  
有关如何部署服务器证书的说明，请参阅[802.1x 有线和无线部署部署服务器证书](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md)。  
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>核心网络助理指南：部署基于密码的 802.1X 身份验证无线访问

此助理指南解释了如何构建在核心网络时通过提供有关如何部署电气和电子工程师协会说明\(IEEE\) 802.1x\-经过身份验证 IEEE 802.11 无线访问使用受保护的可扩展身份验证 Protocol\-Microsoft 质询握手身份验证协议版本 2 \(PEAP\-MS\-CHAP v2\)。

身份验证方法 PEAP\-MS\-CHAP v2 需要该进行身份验证服务器运行网络策略服务器\(NPS\)无线客户端提供服务器证书来证明该 NPS 身份与客户端，但不进行用户身份验证使用的证书-相反，用户提供其域用户名和密码。

因为 PEAP\-MS\-CHAP v2 要求用户提供基于密码的凭据，而不是证书提供身份验证过程，通常更容易且成本更低比 EAP 部署\-TLS 或 PEAP\-TLS。

然后使用本指南以部署无线访问与 PEAP\-MS\-CHAP v2 身份验证方法，必须执行以下操作：

1. 按照在核心网络指南中的说明部署核心网络基础结构，或已具有提到在网络上部署该指南中的技术。
2. 按照的核心网络助理指南部署服务器证书中的说明为 802.1x 有线和无线部署，或已具有提到在网络上部署该指南中的技术。

有关说明如何部署无线访问与 PEAP\-MS\-CHAP v2，请参阅[部署基于密码的 802.1 X 身份验证无线访问](wireless/a-deploy-8021X-wireless-access.md)。

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>核心网络助理指南：部署 BranchCache 托管缓存模式

此助理指南解释了如何部署一个或多个分支机构中托管缓存模式中的 BranchCache。

BranchCache 是广域网 (WAN) 带宽优化技术，在某些版本的 Windows Server 2016 和 Windows 10 操作系统，以及在 Windows 和 Windows Server 的早期版本中所包含。

当你在托管缓存模式中部署 BranchCache 时，分支机构的内容缓存会托管在一个或多个称为托管缓存服务器的服务器计算机上。 托管的缓存服务器可以运行除托管缓存，以便您可以为分支机构中的多个目的使用服务器工作负荷。

BranchCache 托管缓存模式提高缓存效率，因为即使最初请求和缓存数据的客户端处于脱机状态，内容也可用。 由于托管缓存服务器始终可用，更多内容得到缓存，从而节省更多 WAN 带宽，并提高 BranchCache 效率。

当您部署托管的缓存模式下时，多子网分支机构中的所有客户端可以访问存储在托管的缓存服务器上，即使客户端位于不同子网的单个缓存。

有关如何部署 BranchCache 托管缓存模式中的说明，请参阅[部署 BranchCache 托管缓存模式](bc-hcm/1-Deploy-Bc-Hcm.md)。
