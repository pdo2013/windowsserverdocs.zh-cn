---
title: 安全支持提供程序接口体系结构
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de09e099-5711-48f8-adbd-e7b8093a0336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 8b0a74089c5d8a88a380f8a56e3b9e03b84081c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827018"
---
# <a name="security-support-provider-interface-architecture"></a>安全支持提供程序接口体系结构

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本参考主题面向 IT 专业人员介绍了在安全支持提供程序接口 (SSPI) 体系结构中使用的 Windows 身份验证协议。

Microsoft 安全支持提供程序接口 (SSPI) 是 Windows 身份验证的基础。 应用程序和基础结构服务的要求进行身份验证使用 SSPI 提供它。

SSPI 是实现的通用安全服务 API (GSSAPI) 在 Windows Server 操作系统中。 有关 GSSAPI 的详细信息，请参阅 RFC 2743 和 RFC 2744 IETF RFC 数据库中。

默认安全支持提供程序 (Ssp) 的调用在 Windows 中的特定身份验证协议为 Dll 合并到 SSPI。 这些默认 Ssp 以下各节所述。 如果才可以使用 SSPI 进行操作，可整合其他 Ssp。

下图中所示，在 Windows SSPI 提供了一种机制，现有客户端计算机和服务器之间的通信通道将传递身份验证令牌。 当两个计算机或设备需要进行身份验证，以便它们可以安全地通信时，进行身份验证请求路由到 SSPI，后者会完成身份验证过程时，而不考虑当前正在使用的网络协议。 SSPI 返回透明的二进制大型对象。 它们将应用程序，此时它们可以传递给 SSPI 层之间传递。 因此，SSPI 允许应用程序使用计算机或网络上可用的各种安全模型，而无需更改安全系统的接口。

![显示安全支持提供程序接口体系结构关系图](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

以下部分介绍使用 SSPI 交互默认 Ssp。 在 Windows 操作系统中的不同方式使用的 Ssp 用于不安全网络环境中的安全通信。

-   [Kerberos 安全支持提供程序](#BKMK_KerbSSP)

-   [NTLM 安全支持提供程序](#BKMK_NTLMSSP)

-   [摘要式安全支持提供程序](#BKMK_DigestSSP)

-   [Schannel 安全支持提供程序](#BKMK_SchannelSSP)

-   [协商安全支持提供程序](#BKMK_NegoSSP)

-   [凭据安全支持提供程序](#BKMK_CredSSP)

-   [协商扩展安全支持提供程序](#BKMK_NegoExtsSSP)

-   [PKU2U 安全支持提供程序](#BKMK_PKU2USSP)

本主题中还包括：

[安全性支持提供商选项](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="BKMK_KerbSSP"></a>Kerberos 安全支持提供程序
此 SSP 使用 Kerberos 版本 5 协议，如 Microsoft 实施的。 此协议基于网络 Working Group 的 RFC 4120 以及其草稿版本。 它是使用密码或智能卡用于交互登录的行业标准协议。 它也是 Windows 中的服务的首选身份验证方法。

Kerberos 协议自 Windows 2000 以来已默认身份验证协议，因为所有的域服务都支持 Kerberos ssp。 这些服务包括：

-   使用轻型目录访问协议 (LDAP) 的 active Directory 查询

-   使用远程过程调用服务的远程服务器或工作站管理

-   打印服务

-   客户端-服务器身份验证

-   使用服务器消息块 (SMB) 协议 （也称为通用 Internet 文件系统或 CIFS） 的远程文件访问

-   分布式的文件系统管理和引用

-   Intranet 身份验证到 Internet 信息服务 (IIS)

-   有关 Internet 协议安全性 (IPsec) 安全机构身份验证

-   为域用户和计算机的 Active Directory 证书服务的证书申请

位置： %windir%\Windows\System32\kerberos.dll

此提供程序中指定版本中默认情况下包含**适用于**此主题，以及 Windows Server 2003 和 Windows XP 的开始处的列表。

**Kerberos 协议和 Kerberos SSP 的其他资源**

-   [Microsoft Kerberos (Windows)](https://msdn.microsoft.com/library/aa378747(VS.85).aspx)

-   [\[MS KILE\]:Kerberos 协议扩展](https://msdn.microsoft.com/library/cc233855(PROT.10).aspx)

-   [\[MS SFU\]:Kerberos 协议扩展：用户服务和约束的委派协议规范](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)

-   [Kerberos SSP/AP (Windows)](https://msdn.microsoft.com/library/aa377942(VS.85).aspx)

-   [Kerberos 增强功能](https://technet.microsoft.com/library/cc749438(v=ws.10).aspx)适用于 Windows Vista

-   [Kerberos 身份验证中的更改](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx)为 Windows 7 

-   [Kerberos 身份验证技术参考](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)

### <a name="BKMK_NTLMSSP"></a>NTLM 安全支持提供程序
NTLM 安全支持提供程序 (NTLM SSP) 是一个二进制消息传送协议用于通过安全支持提供程序接口 (SSPI)，允许 NTLM 质询-响应身份验证并协商了完整性和保密性的选项。 只要使用 SSPI 身份验证，包括服务器消息块或 CIFS 身份验证、 HTTP 协商身份验证 （例如，Internet Web 身份验证），和远程过程调用服务，将使用 NTLM。 NTLM SSP 包括 NTLM 和 NTLM 版本 2 (NTLMv2) 身份验证协议。

支持的 Windows 操作系统可以针对以下情况使用 NTLM SSP:

-   客户端/服务器身份验证

-   打印服务

-   通过使用 CIFS (SMB) 文件访问

-   安全的远程过程调用服务或 DCOM 服务

Location: %windir%\Windows\System32\msv1_0.dll

此提供程序中指定版本中默认情况下包含**适用于**此主题，以及 Windows Server 2003 和 Windows XP 的开始处的列表。

**NTLM 协议和 NTLM SSP 的其他资源**

-   [Msv1_0 身份验证包 (Windows)](https://msdn.microsoft.com/library/aa378753(VS.85).aspx)

-   [NTLM 身份验证中的更改](https://technet.microsoft.com/library/dd566199(v=ws.10).aspx)在 Windows 7 

-   [Microsoft NTLM (Windows)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)

-   [审核和限制 NTLM 使用指南](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

### <a name="BKMK_DigestSSP"></a>摘要式安全支持提供程序
摘要式身份验证是用于轻型目录访问协议 (LDAP) 和 web 身份验证的行业标准。 摘要式身份验证在网络上传输的凭据作为 MD5 哈希或消息摘要。

摘要 SSP (Wdigest.dll) 用于以下目的：

-   Internet Explorer 和 Internet 信息服务 (IIS) 的访问权限

-   LDAP 查询

位置： %windir%\Windows\System32\Digest.dll

此提供程序中指定版本中默认情况下包含**适用于**此主题，以及 Windows Server 2003 和 Windows XP 的开始处的列表。

**摘要协议和摘要 SSP 的其他资源**

-   [Microsoft 摘要式身份验证 (Windows)](https://msdn.microsoft.com/library/aa378745(VS.85).aspx)

-   [\[MS DPSP\]:摘要协议扩展](https://msdn.microsoft.com/library/cc227906(PROT.13).aspx)

### <a name="BKMK_SchannelSSP"></a>Schannel 安全支持提供程序
安全通道 (Schannel) 用于基于 web 的服务器身份验证，例如当用户尝试访问安全的 web 服务器。

TLS 协议、 SSL 协议、 专用通信技术 (PCT) 协议和数据报传输层 (DTLS) 协议基于公钥加密。 Schannel 提供所有这些协议。 所有 Schannel 协议均使用客户端/服务器模型。 Schannel SSP 使用公钥证书验证参与方。 当对参与方进行身份验证，Schannel SSP 按以下顺序的首选项选择一种协议：

-   传输层安全性 (TLS) 1.0 版

-   传输层安全性 (TLS) 1.1 版

-   传输层安全性 (TLS) 1.2 版

-   安全套接字层 (SSL) 2.0 版

-   安全套接字层 (SSL) 3.0 版

-   私人通信传输 (PCT)

    **请注意**PCT 默认处于禁用状态。

选择的协议是客户端和服务器可以支持的首选身份验证协议。 例如，如果服务器支持所有 Schannel 协议和客户端仅都支持 SSL 3.0 和 SSL 2.0，身份验证过程将使用 SSL 3.0。

DTLS 用于应用程序显式调用。 有关 DTLS 和 Schannel 提供程序使用的其他协议的详细信息，请参阅[Schannel 安全支持提供程序技术参考](../tls/schannel-security-support-provider-technical-reference.md)。

位置： %windir%\Windows\System32\Schannel.dll

此提供程序中指定版本中默认情况下包含**适用于**此主题，以及 Windows Server 2003 和 Windows XP 的开始处的列表。

> [!NOTE]
> 此提供程序在 Windows Server 2008 R2 和 Windows 7 中引入了 TLS 1.2。 此提供程序在 Windows Server 2012 和 Windows 8 中引入了 DTLS。

**TLS 和 SSL 协议和 Schannel SSP 的其他资源**

-   [安全通道 (Windows)](https://msdn.microsoft.com/library/aa380123(VS.85).aspx)

-   [TLS/SSL 技术参考](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)

-   [\[MS TLSP\]:传输层安全性 (TLS) 配置文件](https://msdn.microsoft.com/library/dd207968(PROT.13).aspx)

### <a name="BKMK_NegoSSP"></a>协商安全支持提供程序
简单的受保护 GSS-API 协商机制 (SPNEGO) 窗体和基础协商 ssp 中，该操作用于协商特定身份验证协议。 当应用程序调入 SSPI 以登录到网络时，它可以指定一个 SSP 来处理该请求。 如果应用程序指定协商 SSP，它将分析该请求并选取适当的访问接口来处理该请求，根据客户配置的安全策略。

SPNEGO 是在 RFC 2478 中指定的。

在受支持版本的 Windows 操作系统，Negotiate 安全支持提供程序选择 Kerberos 协议和 NTLM 之间。 协商选择在 Kerberos 协议默认情况下，除非该协议不能由参与身份验证，系统中的一个，或调用应用程序未提供足够的信息来使用 Kerberos 协议。

位置： %windir%\Windows\System32\lsasrv.dll

此提供程序中指定版本中默认情况下包含**适用于**此主题，以及 Windows Server 2003 和 Windows XP 的开始处的列表。

**协商 SSP 的其他资源**

-   [Microsoft 协商 (Windows)](https://msdn.microsoft.com/library/aa378748(VS.85).aspx)

-   [\[MS SPNG\]:简单和受保护 GSS-API 协商机制 (SPNEGO) 扩展](https://msdn.microsoft.com/library/cc247021(PROT.13).aspx)

-   [\[MS N2HT\]:协商和 Nego2 HTTP 身份验证协议规范](https://msdn.microsoft.com/library/dd303576(PROT.13).aspx)

### <a name="BKMK_CredSSP"></a>凭据安全支持提供程序
启动新的终端服务和远程桌面服务会话时，凭据安全服务提供程序 (CredSSP) 提供了单一登录 (SSO) 用户体验。 CredSSP 允许应用程序用户的凭据从客户端计算机委托 （通过使用客户端的 SSP） 到目标服务器 （通过服务器端 SSP)，根据客户端的策略。 通过使用组策略配置 CredSSP 策略和默认关闭委派凭据。

位置： %windir%\Windows\System32\credssp.dll

此提供程序中指定版本中默认情况下包含**适用于**本主题开头的列表。

**有关凭据 SSP 的其他资源**

-   [\[MS CSSP\]:凭据安全支持提供程序 (CredSSP) 协议规范](https://msdn.microsoft.com/library/cc226764(PROT.13).aspx)

-   [对于终端凭据安全服务提供程序和 SSO 服务登录](https://technet.microsoft.com/library/cc749211(v=ws.10).aspx)

### <a name="BKMK_NegoExtsSSP"></a>协商扩展安全支持提供程序
协商扩展 (NegoExts) 是协商的 Ssp，NTLM 或 Kerberos 协议以外使用的身份验证包的应用程序和方案实现由 Microsoft 和其他软件公司。

此扩展到 Negotiate 软件包允许以下方案：

-   **联合系统中的丰富客户端可用性。** SharePoint 站点上访问文档，它们可以通过使用全功能的 Microsoft Office 应用程序进行编辑。

-   **对 Microsoft Office 服务支持丰富的客户端。** 用户可以登录到 Microsoft Office 服务，并使用全功能的 Microsoft Office 应用程序。

-   **托管的 Microsoft Exchange Server 和 Outlook。** 不存在域信任建立，因为 Exchange Server 托管在 web 上找到。 Outlook 使用的 Windows Live 服务来对用户进行身份验证。

-   **客户端计算机和服务器之间的丰富客户端可用性。** 会使用操作系统的联网和身份验证组件。

有关 Kerberos 和 NTLM 一样，Windows 协商包相同的方式将 NegoExts SSP。 NegoExts.dll 被加载到在启动时的系统本地颁发机构 (LSA)。 当收到的身份验证请求时，基于请求的源 NegoExts 协商受支持的 Ssp 之间。 它会收集凭据和策略，加密，并将该信息发送到相应的 SSP，其中创建安全令牌。

支持的 NegoExts Ssp 不是独立的 Ssp，例如 Kerberos 和 NTLM。 因此，NegoExts SSP 中的身份验证方法出于任何原因失败时的身份验证失败消息将显示或记录。 重新协商或回退身份验证的任何方法不都可能。

位置： %windir%\Windows\System32\negoexts.dll

此提供程序中指定版本中默认情况下包含**适用于**本主题中，不包括 Windows Server 2008 和 Windows Vista 开始处的列表。

### <a name="BKMK_PKU2USSP"></a>PKU2U 安全支持提供程序
PKU2U 协议已引入，作为 SSP 在 Windows 7 和 Windows Server 2008 R2 中实现。 此 SSP 启用对等身份验证，特别是通过媒体和文件共享名为 Windows 7 中引入的家庭组功能。 该功能允许不是域的成员的计算机之间共享。

位置： %windir%\Windows\System32\pku2u.dll

此提供程序中指定版本中默认情况下包含**适用于**本主题中，不包括 Windows Server 2008 和 Windows Vista 开始处的列表。

**PKU2U 协议和 PKU2U SSP 的其他资源**

-   [介绍联机标识集成](https://technet.microsoft.com/library/dd560662(v=ws.10).aspx)

## <a name="BKMK_SecuritySupportProviderSelection"></a>安全性支持提供商选项
Windows SSPI 可以使用任何已安装的安全支持提供程序通过支持的协议。 但是，由于并非所有操作系统都支持的 SSP 包为运行 Windows Server 的任何给定计算机，客户端和服务器必须协商要使用一个协议，它们都支持。 Windows Server 更适合于创建客户端计算机和应用程序时要使用 Kerberos 协议，强大的基于标准的协议，可行的但是操作系统将继续允许客户端计算机和客户端不支持 Kerberos 的应用程序要进行身份验证协议。

身份验证可能需要两个位置进行通信之前计算机必须达成协议，它们都可以支持。 若要能够通过 SSPI 任何协议，每台计算机必须具有相应的 ssp。 例如，对于客户端计算机和服务器以使用 Kerberos 身份验证协议，它们必须都支持 Kerberos v5。 Windows Server 使用函数**EnumerateSecurityPackages**来标识支持的 Ssp 的计算机和这些 Ssp 的功能是。

可以在以下两种方式之一进行处理的身份验证协议的选择：

1.  [单一身份验证协议](#BKMK_SingleAuth)

2.  [协商选项](#BKMK_Negotiate)

### <a name="BKMK_SingleAuth"></a>单一身份验证协议
当在服务器上指定一个可接受协议时，客户端计算机必须支持指定的协议，否则通信将失败。 如果指定一个可接受协议，则身份验证交换发生，如下所示：

1.  客户端计算机请求对服务的访问。

2.  服务器对请求的答复，并指定将使用的协议。

3.  客户端计算机将检查回复和检查，以确定它是否支持指定的协议的内容。 如果客户端计算机支持指定的协议，将继续进行身份验证。 如果客户端计算机不支持该协议，身份验证失败，而不考虑是否在客户端计算机是否有权访问的资源。

### <a name="BKMK_Negotiate"></a>协商选项
可以使用 negotiate 选项以允许客户端和服务器来尝试找到可接受的协议。 这基于简单和受保护 GSS-API 协商机制 (SPNEGO)。 身份验证开始时的身份验证协议协商的选项，进行 SPNEGO 交换发生，如下所示：

1.  客户端计算机请求对服务的访问。

2.  它可以支持的身份验证协议和身份验证质询或响应，根据是其第一个选择的协议的列表使用的服务器回复。 例如，服务器可能会列出的 Kerberos 协议和 NTLM，并将 Kerberos 身份验证响应发送。

3.  客户端计算机将检查答复和检查，以确定它是否支持指定的任何的协议的内容。

    -   如果客户端计算机支持首选的协议，则继续身份验证。

    -   如果客户端计算机不支持的首选的协议，但它支持一个按服务器列出的其他协议，客户端计算机可告知服务器并支持，身份验证协议和身份验证将继续。

    -   如果客户端计算机不支持任何列出的协议，身份验证交换失败。

## <a name="see-also"></a>请参阅
[Windows 身份验证体系结构](https://technet.microsoft.com/library/dn169024(v=ws.10).aspx)


