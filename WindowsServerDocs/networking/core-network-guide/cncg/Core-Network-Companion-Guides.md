---
title: Core 网络助手指南
description: 本主题提供 Windows Server 2016 Core 网络指南助手指南的概述
manager: brianlic
ms.technology: networking
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3c272c51cc69017b75e50e79e58186c0ea7c6391
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-companion-guides"></a>Core 网络助手指南

>适用于：Windows Server（半年通道），Windows Server 2016

Windows Server 2016 的同时[Core 网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)提供有关如何部署新的 Active Directory 的说明进行操作&reg;森林具有一个新根域和支持的网络基础结构，助手指南将为你提供添加到你的网络的功能的能力。

每个助手指南允许你部署核心网络后，请完成特定目标。 在某些情况下，有多个助手指导，部署在一起并按照正确的顺序时，您可以实现很复杂目标测量、 成本效益、 合理的方式。

如果你遇到 Core 网络指南之前部署 Active Directory 域和 core 网络，你仍然可以使用助手指南添加到你的网络功能。 只需 Core 网络指南用作先决条件的列表，然后知道，部署助手指南与其他功能，你的网络必须满足所提供的核心网络指南先决条件。

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>部署服务器 802.1 X 有线和无线部署的证书，core 网络助手指南： 

此助手指南介绍了如何通过部署的计算机运行的网络策略服务器 \(NPS\) 和 / 或远程访问服务 \(RAS\) 服务器证书构建 core 网络。

部署可扩展身份验证协议 \(EAP\) 和保护 EAP \(PEAP\) 网络的访问权限身份验证的证书基于身份验证方法时，都需要 server 证书。 部署使用 Active Directory 证书服务服务器证书 \(AD CS\) EAP 和 PEAP 证书基于身份验证方法为提供以下优势：

- 绑定到私密盘 NPS 或 RAS 服务器的身份
- 自动注册域成员 NPS 和 RAS 服务器对证书成本高效和安全的方法
- 用于管理证书和证书颁发机构有效的方法
- 提供的证书基于身份验证的安全
- 能够展开用于其他用途证书的使用
  
有关如何部署证书服务器上的说明进行操作，请参阅[802.1 X 有线和无线部署部署服务器证书](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md)。  
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>Core 网络助手指南： 部署密码基于的 802.1 X 身份验证的无线接入

此助手指南介绍了如何版本核心网络时，通过提供有关如何部署电气和电子工程师 \(IEEE\) 802.1X\ 的说明进行操作-通过 IEEE 802.11 使用保护可扩展身份验证 Protocol\-Microsoft 挑战握手身份验证协议版本 2 的无线接入身份验证 \ (PEAP\ MS\ CHAP v2\)。

身份验证方法 PEAP\ MS\ CHAP v2 要求，身份验证的服务器用向客户端证明 NPS 服务器身份一个服务器证书运行网络策略服务器 \(NPS\) 存在无线客户端，不过用户身份验证不由使用证书-相反，用户提供了域用户名和密码。

因为 PEAP\ MS\ CHAP v2 所需的用户身份验证过程中，提供基于密码的凭据，而不是一个证书，则通常变得轻松又成本较低比 EAP\ TLS 或 PEAP\ TLS 部署。

使用本指南部署 PEAP\ MS\ CHAP v2 身份验证方法与无线接入之前，你必须执行以下操作：

1. 按照说明 Core 网络指南中的部署 core 网络基础结构，或已已经提到该部署到你的网络上的指南中的技术。
2. 按照 802.1 X 有线和无线部署 Core 网络助手指南部署服务器证书中的说明或已已经提到该部署到你的网络上的指南中的技术。

有关如何部署 PEAP\ MS\ CHAP v2 与无线接入说明，请参阅[基于部署密码 802.1 X 身份验证无线访问](wireless/a-deploy-8021X-wireless-access.md)。

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>Core 网络助手指南： 部署分支缓存托管的缓存模式

此助手指南介绍了如何部署分支缓存型托管缓存中一个或多个分支机构。

分支缓存是一种宽的区域网络 (WAN) 带宽优化技术，包含在某些版本的 Windows Server 2016 和 Windows 10 操作系统，以及在较早版本的 Windows 和 Windows Server。

分支缓存托管的缓存型部署时，某个上托管内容缓存分支机构或称为的更多服务器计算机托管缓存的服务器。 托管的缓存服务器可以运行工作负载除了举办缓存，这允许你为 branch 办公室中的多个目的使用服务器。

分支缓存托管缓存模式增加缓存效率，即使最初请求和缓存的数据的客户端处于脱机状态，因为内容提供。 因为托管的缓存 server 始终可用，缓存更多内容提供更大 WAN 带宽流量节省类型，并提高效率分支缓存。

部署托管的缓存模式时，多子网 branch 办公室中的所有客户可以都访问存储在托管的缓存服务器上，即使这些客户端是不同子网单个缓存。

有关如何部署型托管缓存分支缓存的说明，请参阅[托管缓存模式部署分支缓存](bc-hcm/1-Deploy-Bc-Hcm.md)。
