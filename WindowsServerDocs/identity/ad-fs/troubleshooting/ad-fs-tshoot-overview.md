---
title: AD FS 疑难解答
description: 本文档介绍如何排查 AD FS 的各个方面
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 416d9a68326927fcdf5884ef794a74e24a25bb8b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366100"
---
# <a name="troubleshooting-ad-fs"></a>AD FS 疑难解答
AD FS 有许多移动部分，涉及许多不同的事情，并具有许多不同的依赖关系。  当然，这可能会导致各种问题。  本文档旨在帮助你开始排查这些问题。  本文档将介绍你应重点关注的典型领域，如何启用其他信息的功能，以及可用于跟踪问题的各种工具。  

>[!NOTE]
>有关其他信息，请参阅[ADFS 帮助](http://adfshelp.microsoft.com)，它在一个位置提供有效工具，使用户和管理员可以更轻松地解决身份验证问题。 


## <a name="what-to-check-first"></a>首先要检查的内容
深入了解故障排除之前，请先检查一些事项。  它们分别是：
- **DNS 配置**-是否可以解析联合身份验证服务的名称？  这应该解析为服务器场中某个 AD FS 服务器的负载均衡器的 IP 地址或 IP 地址。  有关详细信息，请参阅[AD FS 故障排除-DNS](ad-fs-tshoot-dns.md)。
- **AD FS 终结点**-能否浏览到 AD FS 终结点？  通过浏览到此，你可以确定 AD FS web 服务器是否响应请求。  如果你可以访问此文件，则你知道 AD FS 通过443对请求提供服务。  有关详细信息，请参阅[故障排除-终结点 AD FS](ad-fs-tshoot-endpoints.md)。
- **Idp 启动的登录**-是否可以通过 Idp 启动的登录页登录并进行身份验证？  你需要确保已启用此页面，因为默认情况下它处于禁用状态。  使用 `Set-AdfsProperties -EnableIdPInitiatedSignOn $true` 启用页面。  如果你可以登录并进行身份验证，则你知道 AD FS 在此区域中工作。  有关详细信息，请参阅[AD FS 故障排除-登录](ad-fs-tshoot-initiatedsignon.md)。
  ##  <a name="common-troubleshooting-areas"></a>常见疑难解答区域

|名称|描述|
|-----|-----|
|[事件日志记录和审核](ad-fs-tshoot-logging.md)|使用 Windows 事件日志，通过管理日志和跟踪日志查看高级和低级信息。  它还可用于查看安全审核。|
|[SQL 连接](ad-fs-tshoot-sql.md)|有关测试 AD FS 服务器与后端 SQL 数据库之间的连接的信息|
|[声明颁发](ad-fs-tshoot-claims-issuance.md)|有关确定 AD FS 是否正确颁发声明的信息。|
|[循环检测](ad-fs-tshoot-loop.md)|有关确定和阻止用户在 Idp 与 RP 之间来回退回的信息。|
|[证书](ad-fs-tshoot-certs.md)|可能出现的 Typcial 证书问题|
|[Fiddler](ad-fs-tshoot-fiddler.md)|有关如何安装和使用 Fiddler 的信息|
|[WS-联合身份验证与 Fiddler](ad-fs-tshoot-fiddler-ws-fed.md)|WS 联合身份验证交互的详细 Fiddler 跟踪|
|[声明规则](ad-fs-tshoot-claims-rules.md)|有关声明规则和语法疑难解答的信息|
|[集成的 Windows 身份验证](ad-fs-tshoot-iwa.md)|集成身份验证疑难解答的信息。|
|[Azure AD](ad-fs-tshoot-azure.md)|有关解决 AD FS 与 Azure AD 交互的信息。|
|[AD FS 诊断分析器](ad-fs-diagnostics-analyzer.md)|AD FS 帮助诊断分析器可帮助使用诊断 PowerShell 模块执行基本 AD FS 检查。 