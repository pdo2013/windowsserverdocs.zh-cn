---
title: 核心网络助理指南
description: 本主题概述了 Windows Server 2016 Core 网络指南的助理指南
manager: brianlic
ms.technology: networking
ms.prod: windows-server
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c0895cfd62d462ef6d158dc39ef59a9ee10a7c98
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406303"
---
# <a name="core-network-companion-guidance"></a>核心网络助理指南

>适用于：Windows Server（半年频道）、Windows Server 2016

Windows Server 2016 [Core 网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)提供了有关如何使用新的根域和支持的网络基础结构部署新的 Active Directory @ no__t-1 林的说明，辅助指南提供了将网络功能。

在你部署完核心网络之后，每个助理指南都允许你完成一个具体的目标。 有些情况下，有多个助理指南，当它们一起部署并且顺序正确的时候，你可以用量化、合算、合理的方式实现非常复杂的目标。

如果你在见到“核心网络指南”之前部署你的 Active Directory 域和核心网络，你仍然可以使用“助理指南”向网络添加功能。 简单地使用“核心网络指南”作为先决条件列表，并且知道，要使用“助理指南”部署附加功能，你的网络必须满足“核心网络指南”提供的先决条件。

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>核心网络助理指南：为 802.1X 有线和无线部署部署服务器证书 

本助理指南介绍了如何通过为运行网络策略服务器 @no__t 的计算机（\(RAS @ no__t-3）和/或远程访问服务部署服务器证书，来在核心网络上进行构建。

使用可扩展的身份验证协议部署基于证书的身份验证方法时需要服务器证书 \(EAP @ no__t 和 Protected EAP \(PEAP @ no__t-3 用于网络访问身份验证。 对于 EAP 和 PEAP 基于证书的身份验证方法，Active Directory 证书服务部署服务器证书 \(AD CS @ no__t-1 具有以下优势：

- 将 NPS 或 RAS 服务器的标识绑定到私钥
- 向域成员 NPS 和 RAS 服务器自动注册证书的经济高效、安全的方法
- 是管理证书和证书颁发机构的有效方法
- 基于证书的身份验证所提供的安全性
- 为其他目的扩展证书用途的功能
  
有关如何部署服务器证书的说明，请参阅为[802.1 x 有线和无线部署部署服务器证书](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md)。  
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>核心网络助理指南：部署基于密码的 802.1X 身份验证无线访问

本助理指南介绍了如何通过提供有关如何使用受保护的 0IEEE @ no__t-1 802.1 X @ no__t-2authenticated IEEE 802.11 无线访问来部署电气和电子 @no__t 工程师协会的说明，在核心网络上进行构建可扩展的身份验证协议 \ – Microsoft 质询握手身份验证协议版本 2 \(PEAP @ no__t-4MS @ no__t-5CHAP v2 @ no__t-6。

身份验证方法 PEAP @ no__t-0MS @ no__t-1CHAP v2 要求身份验证运行网络策略服务器的服务器 \(NPS @ no__t-3 提供具有服务器证书的无线客户端，以便向客户端证明 NPS 标识，但是用户身份验证不是通过使用证书来执行的，而是用户提供其域用户名和密码。

由于 PEAP @ no__t-0MS @ no__t-1CHAP v2 要求用户提供基于密码的凭据，而不是在身份验证过程中使用证书，因此部署比 EAP @ no__t-2TLS 或 PEAP @ no__t-3TLS 更简单、更便宜。

使用 PEAP @ no__t-0MS @ no__t-1CHAP v2 身份验证方法部署无线访问之前，必须执行以下操作：

1. 按照核心网络指南中的说明部署核心网络基础结构，或已在网络上部署了该指南中提供的技术。
2. 按照核心网络助理指南部署用于 802.1 X 有线和无线部署的服务器证书中的说明，或已在网络上部署了该指南中提供的技术。

有关如何使用 PEAP @ no__t-0MS @ no__t-1CHAP v2 部署无线访问的说明，请参阅[部署基于密码的 802.1 x 身份验证无线访问](wireless/a-deploy-8021X-wireless-access.md)。

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>核心网络助理指南：部署 BranchCache 托管缓存模式

本助理指南介绍了如何在一个或多个分支机构中的托管缓存模式下部署 BranchCache。

BranchCache 是一种广域网络（WAN）带宽优化技术，包含在某些版本的 Windows Server 2016 和 Windows 10 操作系统以及更早版本的 Windows 和 Windows Server 中。

当你在托管缓存模式中部署 BranchCache 时，分支机构的内容缓存会托管在一个或多个称为托管缓存服务器的服务器计算机上。 托管缓存服务器除了托管缓存外，还可以运行工作负荷，这允许你将服务器用于分支机构中的多个目的。

BranchCache 托管缓存模式提高了缓存效率，因为即使最初请求和缓存数据的客户端脱机，内容也可用。 由于托管缓存服务器始终可用，更多内容得到缓存，从而节省更多 WAN 带宽，并提高 BranchCache 效率。

当你部署托管缓存模式时，多子网分支机构中的所有客户端都可以访问一个存储在托管缓存服务器上的缓存，即使客户端位于不同的子网中。

有关如何在托管缓存模式下部署 BranchCache 的说明，请参阅[部署 Branchcache 托管缓存模式](bc-hcm/1-Deploy-Bc-Hcm.md)。
