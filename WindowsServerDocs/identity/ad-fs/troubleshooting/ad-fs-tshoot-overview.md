---
title: AD FS 疑难解答
description: 本文档介绍如何对 AD FS 的各个方面进行故障排除
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 60ecf94b72e58aed4d3718b19f6007cdad1c9578
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840948"
---
# <a name="troubleshooting-ad-fs"></a>AD FS 疑难解答
AD FS 具有许多移动部分、 涉及许多不同的事物和有许多不同的依赖关系。  当然，这可能导致各种问题引发。  本文档旨在帮助你开始对这些问题进行疑难解答。  本文档将向您介绍您应集中精力，如何启用的其他信息，并可用于跟踪问题的各种工具的功能的典型区域。  

>[!NOTE]
>有关其他信息，请参阅[ADFS 帮助](http://adfshelp.microsoft.com)其中提供了一个有效的工具将更容易为用户和管理员以解决身份验证问题，以更快的速度。 


## <a name="what-to-check-first"></a>要检查第一个的内容
深入了解深入的故障排除之前，有几件事，您应首先检查。  它们分别是：
- **DNS 配置**-可以将联合身份验证服务的名称解析？  这应解析为任一负载均衡器 IP 地址或其中一个 AD FS 服务器场中的 IP 地址。  有关详细信息请参阅[AD FS 故障排除-DNS](ad-fs-tshoot-dns.md)。
- **AD FS 终结点**-您可以浏览到 AD FS 终结点？  通过浏览到此您可以确定在 AD FS web 服务器可以响应的请求。  可以转到此文件后，如果您知道 AD FS 用通过 443 可以很好地处理请求。  有关详细信息请参阅[AD FS 故障排除-终结点](ad-fs-tshoot-endpoints.md)。
- **Idp 发起的登录**-你可以登录和身份验证通过 Idp-Initiated 登录页？  您需要确保已启用此页，因为默认情况下禁用。  使用`Set-AdfsProperties -EnableIdPInitiatedSignOn $true`以启用页。  如果可以登录和身份验证您知道的 AD FS 在此区域中正常工作。  有关详细信息请参阅[AD FS 故障排除-登录](ad-fs-tshoot-initiatedsignon.md)。
##  <a name="common-troubleshooting-areas"></a>常见故障排除的区域

|名称|描述|
|-----|-----|
|[事件日志记录和审核](ad-fs-tshoot-logging.md)|使用 Windows 事件日志来查看高级别和低级别的信息通过的管理和跟踪日志。  此外可以用于查看安全审核。|
|[SQL 连接](ad-fs-tshoot-sql.md)|测试 AD FS 服务器和后端 SQL 数据库之间的连接信息|
|[声明颁发](ad-fs-tshoot-claims-issuance.md)|确定 AD FS 是否正确发出声明的信息。|
|[循环检测](ad-fs-tshoot-loop.md)|有关确定并阻止用户正在 RP Idp 之间来回退回的信息。|
|[证书](ad-fs-tshoot-certs.md)|可能出现的典型证书问题|
|[Fiddler](ad-fs-tshoot-fiddler.md)|如何安装和使用 Fiddler 信息|
|[WS 联合身份验证使用 Fiddler](ad-fs-tshoot-fiddler-ws-fed.md)|详细的 WS 联合身份验证交互的 Fiddler 跟踪|
|[声明规则](ad-fs-tshoot-claims-rules.md)|故障排除的声明规则和其语法的信息|
|[集成的 Windows 身份验证](ad-fs-tshoot-iwa.md)|排查集成身份验证的信息。|
|[Azure AD](ad-fs-tshoot-azure.md)|排查 Azure AD 与 AD FS 交互的信息。|
|[AD FS 诊断分析器](ad-fs-diagnostics-analyzer.md)|AD FS 帮助诊断分析器可帮助执行基本的 AD FS 检查使用诊断 PowerShell 模块。 