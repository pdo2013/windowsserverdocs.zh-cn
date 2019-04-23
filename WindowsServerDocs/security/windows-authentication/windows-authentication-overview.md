---
title: Windows 身份验证概述
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 485a0774-0785-457f-a964-0e9403c12bb1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 2021ccf1e0015cc910f43966f9400e6bd56a230c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870968"
---
# <a name="windows-authentication-overview"></a>Windows 身份验证概述

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本导航主题面向 IT 专业人员，列出了 Windows 身份验证和登录技术的文档资源，包括产品评估、入门指南、过程、设计和部署指南、技术参考以及命令参考。

## <a name="feature-description"></a>功能说明
身份验证是验证对象、 服务或人员身份的过程。 当你对某个对象进行身份验证时，目的就在于验证该对象是否为正版。 当你对某个服务或人进行身份验证时，目的就在于验证出示的凭据是否可信。

在网络环境中，身份验证是指证明网络应用程序或资源的身份的动作。 通常情况下，标识是通过使用任一密钥只有用户知道-使用公钥加密-或共享的密钥的加密操作的验证。 身份验证交换的服务器端会将签名数据和已知的加密密钥相比较来验证身份验证尝试。

将加密密钥存储在安全的中心位置中可以确保身份验证过程可伸缩且可维护。 Active Directory 域服务是建议和用于存储标识信息的默认技术\(包括是用户的凭据的加密密钥\)。 Active Directory 是默认 NTLM 和 Kerberos 实现所必需的。

标识基于某些内容只有用户知道的类似于密码，使用用户提供的功能更强大安全机制-类似于令牌，公钥证书的用户简单登录过程中的身份验证技术范围和生物识别。 在企业环境中，服务或用户可访问单独一个位置或多个位置中多种服务器类型上的多个应用程序或资源。 鉴于这些原因，身份验证必须支持其他平台和其他 Windows 操作系统的环境。

Windows 操作系统实现一组默认的身份验证协议，包括 Kerberos、 NTLM、 传输层安全性\/安全套接字层\(TLS\/SSL\)，和摘要，作为的一部分可扩展的体系结构。 此外，一些协议被合并到身份验证程序包中，例如 Negotiate 和凭据安全支持提供程序。 这些协议和程序包能够对用户、计算机和服务进行身份验证；反过来身份验证过程使授权的用户和服务能够安全地访问资源。

有关 Windows 身份验证的详细信息包括

-   [Windows 身份验证概念](windows-authentication-concepts.md)

-   [Windows 登录方案](windows-logon-scenarios.md)

-   [Windows 身份验证体系结构](windows-authentication-architecture.md)

-   [安全支持提供程序接口体系结构](security-support-provider-interface-architecture.md)

-   [在 Windows 身份验证的凭据进程](credentials-processes-in-windows-authentication.md)

-   [Windows 身份验证中使用组策略设置](group-policy-settings-used-in-windows-authentication.md)

请参阅[Windows 身份验证技术概述](windows-authentication-technical-overview.md)。

## <a name="practical-applications"></a>实际应用程序
Windows 身份验证用于验证信息是来自受信任来源还是来自个人或计算机对象，例如另一台计算机。 Windows 提供按如下所述实现该目标的多个不同方式。

|收件人...|功能|描述|
|----|------|--------|
|Active Directory 域中的身份验证|Kerberos|Microsoft Windows&nbsp;服务器操作系统实现 Kerberos 版本 5 身份验证协议和对公钥身份验证扩展。 作为安全支持提供程序实现 Kerberos 身份验证客户端\(SSP\)并可以通过安全支持提供程序接口来访问\(SSPI\)。 初始用户身份验证与 Winlogon 单个登录集成\-体系结构。 Kerberos 密钥分发中心\(KDC\)与域控制器上运行其他 Windows Server 安全服务集成。 KDC 使用域的 Active Directory 目录服务数据库作为其安全帐户数据库。 Active Directory 是默认 Kerberos 实现所必需的。<br /><br />有关更多资源，请参阅 [Kerberos 身份验证概述](../kerberos/kerberos-authentication-overview.md)。|
|Web 上的安全身份验证|TLS\/在 Schannel 安全支持提供程序中实现 SSL|传输层安全性\(TLS\)协议版本 1.0、 1.1 和 1.2、 安全套接字层\(SSL\)协议、 版本 2.0 和 3.0、 数据报传输层安全协议版本 1.0 和私有通信传输\(PCT\)协议版本 1.0 基于公钥加密。 安全通道\(Schannel\)提供程序身份验证协议套件提供这些协议。 所有 Schannel 协议使用客户端和服务器模型。<br /><br />有关更多资源，请参阅[TLS-SSL &#40;Schannel SSP&#41;概述](../tls/tls-ssl-schannel-ssp-overview.md)。|
|对 Web 服务或应用程序进行身份验证|集成的 Windows 身份验证<br /><br />摘要式身份验证|有关其他资源，请参阅[“集成 Windows 身份验证”](https://technet.microsoft.com/library/cc758557(v=WS.10).aspx)和[“摘要式身份验证”](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx)，以及[“高级摘要式身份验证”](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx)。|
|对旧版应用程序进行身份验证|NTLM|NTLM 是一项挑战\-响应样式身份验证协议。除了身份验证，NTLM 协议有选择性地提供会话安全-尤其是消息的完整性和保密性通过签名和密封功能在 NTLM。<br /><br />有关更多资源，请参阅 [NTLM 概述](../kerberos/ntlm-overview.md)。|
|利用多重身份验证|智能卡支持<br /><br />生物识别支持|智能卡是防篡改\-坚固的简便方法，为任务，例如客户端身份验证，登录到域中，提供安全解决方案的代码签名和保护电子\-邮件。<br /><br />生物识别依赖于测量个人的不变物理特征以便对此人进行唯一标识。 指纹是最常用的生物识别特征，数以百万计的指纹生物识别设备都嵌入到个人计算机和外围设备中。<br /><br />有关更多资源，请参阅[智能卡技术参考](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)。 |
|提供本地管理、存储和重新使用凭据|凭据管理<br /><br />本地安全机构<br /><br />密码|Windows 中的凭据管理可确保凭据安全存储。 在安全桌面上收集凭据\(进行本地或域访问\)、 通过应用程序或网站，以便每次访问资源时出现正确的凭据。<br /><br />
|扩展对旧版系统的现代身份验证保护|扩展的身份验证保护|此功能可增强的保护和处理的凭据时使用集成 Windows 身份验证的网络连接进行身份验证\(IWA\)。|

## <a name="software-requirements"></a>软件要求
Windows 身份验证经设计与之前版本的 Windows 操作系统兼容。 但每个版本中的改进并不一定适用于以前的版本。 详细信息请参阅特定功能的相关文档。

## <a name="server-manager-information"></a>服务器管理器信息
可使用组策略配置很多身份验证功能，组策略可使用服务器管理器来安装。 Windows 生物识别框架功能是使用服务器管理器安装的。 依赖于身份验证方法，如 Web 服务器的其他服务器角色\(IIS\)也可以使用服务器管理器安装 Active Directory 域服务。

## <a name="related-resources"></a>相关资源

|身份验证技术|资源|
|----------------|-------|
|Windows 身份验证|[Windows 身份验证技术概述](../windows-authentication/windows-authentication-technical-overview.md)<br />包含的主题寻址版本、 常规身份验证概念、 登录方案、 适用于支持的版本，以及适用设置体系结构之间的差异。|
|Kerberos|[Kerberos 身份验证概述](../kerberos/kerberos-authentication-overview.md)<br /><br />[Kerberos 约束委派概述](../kerberos/kerberos-constrained-delegation-overview.md)<br /><br />[Kerberos 身份验证技术参考](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003年\)<br /><br />[Kerberos 生存指南](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx) \(TechNet Wiki\)|
|TLS\/SSL 和 DTLS \(Schannel 安全支持提供程序\)|[TLS-SSL &#40;Schannel SSP&#41;概述](../tls/tls-ssl-schannel-ssp-overview.md)<br /><br />[Schannel 安全支持提供程序技术参考](../tls/schannel-security-support-provider-technical-reference.md)|
|摘要式身份验证|[摘要式身份验证技术参考](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003年\)|
|NTLM|[NTLM 概述](../kerberos/ntlm-overview.md)<br />包含当前和过去资源的链接|
|PKU2U|[介绍 Windows 中的 PKU2U](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|智能卡|[智能卡技术参考](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<br /><br />
|凭据|[凭据保护和管理](../credentials-protection-and-management/credentials-protection-and-management.md)<br />包含当前和过去资源的链接<br /><br />[密码概述](../kerberos/passwords-overview.md)<br />包含当前和过去资源的链接|


