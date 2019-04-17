---
title: "Windows 身份验证概述"
description: "Windows Server 安全"
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
ms.openlocfilehash: c2ec55ed6b09628d1d80a24be766259980e84d6e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="windows-authentication-overview"></a>Windows 身份验证概述

>适用于：Windows Server（半年通道），Windows Server 2016

面向 IT 专业人员导航本文列出了对 Windows 身份验证和登录技术，其中包括产品评估，以获取入门的指南、 过程、 设计和部署的指南、 技术参考和命令参考文档资源。

## <a name="feature-description"></a>功能描述
验证是用于对象、 服务或人的身份进行验证过程。 当你验证身份一个对象时，目标是验证对象是正版。 当您进行身份验证服务或联系人时，的目标是出示的凭据是可信验证。

在网络的上下文，身份验证是到的网络应用程序或资源证明身份的操作。 通常情况下，经过加密操作，使用任一密钥仅用户知道的与公钥-或共享的键通过验证身份。 身份验证更换费用的服务器端比较用已知密钥来验证身份验证尝试签名的数据。

存储在中心位置安全密钥使身份验证过程，可缩放和维护。 Active Directory 域服务是推荐默认技术来存储身份的信息和 \ （包括是用户 credentials\ 加密密钥）。 Active Directory 是必需的默认 NTLM 和 Kerberos 实现。

身份验证方法范围是从一个简单的登录，标识用户基于仅用户知道的密码，如使用具有用户-标记公钥证书，生物识别等内容的功能更强大安全机制到的内容。 商业环境中，在服务或用户可能会访问多个应用上或资源多种类型的服务器，在一个位置，或在多个位置。 鉴于上述原因，身份验证必须支持环境对其他平台和其他 Windows 操作系统。

Windows 操作系统实现默认的一组身份验证协议，包括 Kerberos，NTLM、 传输层 Security\/安全套接字层 \(TLS\/SSL\) 和摘要，可扩展体系结构的一部分。 此外，某些协议会合并到如协商和凭据安全支持的提供商的验证包。 这些协议和包启用用户、 计算机和其他形式的身份验证身份验证过程中，依次使授权的用户和服务安全的方式访问资源。

有关详细信息，包括 Windows 身份验证

-   [Windows 身份验证的概念](windows-authentication-concepts.md)

-   [Windows 登录方案](windows-logon-scenarios.md)

-   [Windows 身份验证建筑](windows-authentication-architecture.md)

-   [安全支持的提供商界面建筑](security-support-provider-interface-architecture.md)

-   [在 Windows 身份验证的凭据进程](credentials-processes-in-windows-authentication.md)

-   [在 Windows 身份验证使用组策略设置](group-policy-settings-used-in-windows-authentication.md)

请参阅[Windows 身份验证 Technical 概述](windows-authentication-technical-overview.md)。

## <a name="practical-applications"></a>实际应用
Windows 身份验证用于从用户或计算机对象，如另一台计算机是否的验证信息来自受信任的源。 Windows 提供了许多不同的方式来实现这一目标，如下所述。

|自。。。|功能|描述|
|----|------|--------|
|身份验证的 Active Directory 域中|Kerberos|Microsoft Windows&nbsp;服务器操作系统实现 Kerberos 5 版本身份验证协议和公钥身份验证的扩展。 身份验证 Kerberos 客户实现为安全支持的提供商 \(SSP\)，可以通过安全支持的提供商接口 \(SSPI\) 访问。 与 Winlogon 单个 sign\ 上建筑集成初始用户身份验证。 Kerberos 密钥 Distribution 中心 \(KDC\) 是已集成与其他 Windows Server 安全服务，域控制器上运行。 KDC 使用的域的 Active Directory 目录服务数据库作为其安全帐户数据库。 Active Directory 是必需的默认 Kerberos 实现。<br /><br />有关其他资源，请参阅[Kerberos 身份验证概述](../kerberos/kerberos-authentication-overview.md)。|
|在 web 上的安全身份验证|当在 Schannel 安全支持的提供商中实现 TLS\/SSL|传输层安全性 \(TLS\) 协议 1.0，1.1、 1.2 安全套接字层 \(SSL\) 协议，版本 2.0 和数据报传输层安全性 3.0 协议版本版本 1.0 而言和专用通信传输 \(PCT\) 协议，版 1.0 而言，基于公用加密。 安全通道 \(Schannel\) 提供商身份验证协议套件提供以下协议。 所有 Schannel 协议都使用客户端和服务器模型。<br /><br />有关其他资源，请参阅[TLS-SSL & #40;Schannel SSP & #41;概述](../tls/tls-ssl-schannel-ssp-overview.md)。|
|通过 web 服务或应用程序身份验证|集成的 Windows 身份验证<br /><br />简要身份验证|有关其他资源，请参阅 [集成 Windows 身份验证] (https://technet.microsoft.com/library/cc758557(v=WS.10.aspx and [Digest Authentication](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx), and [Advanced Digest Authentication](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx).|
|通过旧版应用程序的身份验证|NTLM|NTLM 是 challenge\ 响应样式身份验证 protocol.In 加入身份验证，NTLM 协议 （可选） 提供适用于会话安全-专门消息完整性和保密通过登录和封装 NTLM 中的功能。<br /><br />有关其他资源，请参阅[NTLM 概述](../kerberos/ntlm-overview.md)。|
|利用多重身份验证|智能卡支持<br /><br />生物支持|智能卡是 tamper\ 溅和笔记本电脑的方式提供任务，如登录到域的客户端身份验证的安全解决方案签名和保护 e\ 邮件的代码。<br /><br />测量唯一标识该用户的用户不变物理特征依赖生物识别。 指纹是一种最常使用的生物特征，有数百万指纹嵌入在个人计算机和外围设备的生物识别设备的用户。<br /><br />有关其他资源，请参阅[智能卡技术参考](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)。 |
|提供本地管理、 存储和重新使用凭据|凭据管理<br /><br />本地安全颁发机构<br /><br />密码|在 Windows 中的凭据管理确保凭据将安全地存储。 在安全的桌面上收集凭据 \ （适用于本地或域 access\) 通过应用或网站，以便显示了正确的凭据每次访问的资源。<br /><br />
|扩展到旧系统现代身份验证保护|扩展的保护进行身份验证|此功能可进行网络连接使用集成 Windows 身份验证 \(IWA\) 身份验证时增强的保护和处理的凭据。|

## <a name="software-requirements"></a>软件要求
Windows 身份验证设计为可与以前版本的 Windows 操作系统兼容。 但是，每个版本中的改进并不一定适用于以前的版本。 请参阅有关特定功能的详细信息的文档。

## <a name="server-manager-information"></a>服务器管理器信息
可以使用组策略，可以使用服务器管理器安装配置许多身份验证功能。 安装 Windows 的生物识别框架功能使用服务器管理器。 这取决于身份验证方法、 Web 服务器 \(IIS\) 和 Active Directory 域服务，例如其他服务器角色还使用服务器管理器安装。

## <a name="related-resources"></a>相关的资源

|身份验证技术|资源|
|----------------|-------|
|Windows 身份验证|[Windows 身份验证 Technical 概述](../windows-authentication/windows-authentication-technical-overview.md)<br />包括主题解决版本常规身份验证的概念、 登录的情况下，用于受支持的版本，并且适用设置体系结构之间的区别。|
|Kerberos|[Kerberos 身份验证概述](../kerberos/kerberos-authentication-overview.md)<br /><br />[Kerberos 约束的委派概述](../kerberos/kerberos-constrained-delegation-overview.md)<br /><br />[Kerberos身份验证技术参考](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003\)<br /><br />[Kerberos生存指南](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx)\(TechNet Wiki\)|
|TLS\/SSL 和 DTLS \ (Schannel 安全支持 provider\)|[TLS-SSL & #40;Schannel SSP & #41;概述](../tls/tls-ssl-schannel-ssp-overview.md)<br /><br />[Schannel 安全支持提供商的技术参考](../tls/schannel-security-support-provider-technical-reference.md)|
|简要身份验证|[摘要身份验证技术参考](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003\)|
|NTLM|[NTLM 概述](../kerberos/ntlm-overview.md)<br />包含的当前和过去资源的链接|
|PKU2U|[引入 Windows 中的 PKU2U](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|智能卡|[智能卡技术参考](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<br /><br />
|凭据|[凭据保护和管理](../credentials-protection-and-management/credentials-protection-and-management.md)<br />包含的当前和过去资源的链接<br /><br />[密码概述](../kerberos/passwords-overview.md)<br />包含的当前和过去资源的链接|


