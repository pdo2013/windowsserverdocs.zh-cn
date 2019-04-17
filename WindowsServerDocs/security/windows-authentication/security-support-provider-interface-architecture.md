---
title: "安全支持的提供商界面建筑"
description: "Windows Server 安全"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="security-support-provider-interface-architecture"></a>安全支持的提供商界面建筑

>适用于：Windows Server（半年通道），Windows Server 2016

IT 专业人员的此参考主题介绍了在安全支持提供程序接口 (SSPI) 体系结构中使用的 Windows 身份验证协议。

Microsoft Security 支持提供程序接口 (SSPI) 是 Windows 身份验证的基础。 应用程序和需要身份验证的基础结构服务使用 SSPI 提供。

SSPI 是实现的通用安全服务 API (GSSAPI) 在 Windows Server 操作系统中。 有关 GSSAPI 详细信息，请参阅 RFC 2743 和 RFC 2744 IETF RFC 数据库中。

默认安全支持提供商 (Ssp)，调用 Windows 中特定的身份验证协议作为 Dll 并入 SSPI。 这些默认 Ssp 以下各部分所述。 如果才可以使用 SSPI 进行操作，可以包含其他 Ssp。

下图所示 SSPI Windows 中的提供了执行轻现有的信道之间客户端计算机和服务器身份验证令牌机制。 当两台计算机或设备需要进行身份验证，以便他们可以安全地进行通信时，身份验证的请求被送交 SSPI，完成身份验证过程，无论当前正在使用的网络协议。 SSPI 返回透明二进制较大的对象。 它们将传递之间的应用，以便此时他们可以将传递到 SSPI 层。 因此，SSPI 启用应用程序，而无需更改安全系统接口的计算机上使用各种安全型号可用。

![显示安全支持的提供商界面结构的图示](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

以下部分介绍使用 SSPI 交互默认 Ssp。 Windows 操作系统中不同的方式使用 Ssp 推广在安全网络环境中的安全通信。

-   [Kerberos 安全性支持供应商](#BKMK_KerbSSP)

-   [NTLM 安全性支持供应商](#BKMK_NTLMSSP)

-   [摘要安全性支持供应商](#BKMK_DigestSSP)

-   [Schannel 安全性支持供应商](#BKMK_SchannelSSP)

-   [协商安全性支持供应商](#BKMK_NegoSSP)

-   [凭据安全性支持供应商](#BKMK_CredSSP)

-   [协商扩展安全性支持供应商](#BKMK_NegoExtsSSP)

-   [PKU2U 安全性支持供应商](#BKMK_PKU2USSP)

本主题中，还包括：

[安全支持的提供商所选内容](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="BKMK_KerbSSP"></a>Kerberos 安全性支持供应商
此 SSP 用作由 Microsoft 实现 Kerberos 5 版本协议。 本协议基于网络工作组的 RFC 4120 和草稿修订。 它是使用密码或一张智能卡用于交互式登录行业标准协议。 它也是 Windows 中的服务的首选身份验证方法。

因为 Kerberos 协议已默认身份验证协议 Windows 2000 起，所有域服务都支持 Kerberos ssp。 这些服务包括：

-   使用便捷目录访问协议 (LDAP) 的 active Directory 查询

-   使用远程过程调用服务远程服务器或工作站管理

-   打印服务

-   客户端服务器身份验证

-   访问远程文件使用了服务器消息块 (SMB) 协议 （也称为常见 Internet 文件系统或 CIFS）

-   分布式的文件系统管理和参考

-   身份验证 intranet 到 Internet 信息服务 (IIS)

-   对于 Internet 协议安全安全颁发机构身份验证

-   用户域和计算机的 Active Directory 证书服务到证书请求

位置： %windir%\Windows\System32\kerberos.dll

默认情况下，在版本中指定包括该提供商**适用于**列表中的本主题，以及 Windows Server 2003 和 Windows XP 的开头。

**更多资源 Kerberos 协议和 Kerberos SSP**

-   [Microsoft Kerberos (Windows)](https://msdn.microsoft.com/library/aa378747(VS.85).aspx)

-   [\[MS-KILE\]: Kerberos 协议扩展](https://msdn.microsoft.com/library/cc233855(PROT.10).aspx)

-   [\[MS-SFU\]: Kerberos 协议扩展： 用户和约束的委派协议规范服务](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)

-   [Kerberos SSP/接入点 (Windows)](https://msdn.microsoft.com/library/aa377942(VS.85).aspx)

-   [Kerberos 增强](https://technet.microsoft.com/library/cc749438(v=ws.10).aspx)适用于 Windows Vista

-   [在 Kerberos 身份验证更改](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx)适用于 Windows 7 

-   [Kerberos 身份验证技术参考](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)

### <a name="BKMK_NTLMSSP"></a>NTLM 安全性支持供应商
NTLM 安全性支持供应商 (NTLM SSP) 是消息使用安全支持提供程序接口 (SSPI)，以允许 NTLM 挑战响应身份验证和协商完整性和保密选项协议二进制文件。 使用 NTLM 使用 SSPI 身份验证、 服务器消息块或 CIFS 身份验证、 HTTP 协商身份验证 （如 Internet Web 身份验证） 和远程过程调用服务包括任何地方。 NTLM SSP 包括 NTLM 和 NTLM 版本 (NTLMv2) 2 身份验证的协议。

受支持的 Windows 操作系统可用 NTLM SSP 以下操作：

-   客户端月服务器身份验证

-   打印服务

-   文件使用 CIFS (SMB) 的访问权限

-   安全远程过程调用服务或 DCOM 服务

位置： %windir%\Windows\System32\msv1_0.dll

默认情况下，在版本中指定包括该提供商**适用于**列表中的本主题，以及 Windows Server 2003 和 Windows XP 的开头。

**更多资源 NTLM 协议和 NTLM SSP**

-   [MSV1_0 身份验证程序包 (Windows)](https://msdn.microsoft.com/library/aa378753(VS.85).aspx)

-   [在 NTLM 身份验证更改](https://technet.microsoft.com/library/dd566199(v=ws.10).aspx)Windows 7 中 

-   [Microsoft NTLM (Windows)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)

-   [审核和限制 NTLM 使用量指南](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

### <a name="BKMK_DigestSSP"></a>摘要安全性支持供应商
摘要身份验证是一个用于便捷目录访问协议 (LDAP) 和 web 身份验证的行业标准。 简要身份验证作为 MD5 希或邮件摘要传输整个网络的凭据。

摘要 SSP (Wdigest.dll) 用于:

-   Internet Explorer，Internet 信息服务 (IIS) 的访问权限

-   LDAP 查询

位置： %windir%\Windows\System32\Digest.dll

默认情况下，在版本中指定包括该提供商**适用于**列表中的本主题，以及 Windows Server 2003 和 Windows XP 的开头。

**摘要协议和摘要 SSP 其他资源**

-   [Microsoft 简要身份验证 (Windows)](https://msdn.microsoft.com/library/aa378745(VS.85).aspx)

-   [\[MS-DPSP\]: 摘要协议扩展](https://msdn.microsoft.com/library/cc227906(PROT.13).aspx)

### <a name="BKMK_SchannelSSP"></a>Schannel 安全性支持供应商
对于基于 web 的服务器身份验证，如用户尝试访问安全的 web 服务器时使用安全通道 (Schannel)。

基于公钥加密的 TLS 协议、 SSL 协议、 专用通信技术 （百分比） 协议和数据报传输层 (DTLS) 协议。 Schannel 提供了所有这些协议。 所有 Schannel 协议都使用客户端月服务器模型。 Schannel SSP 使用公钥证书方身份验证。 进行方身份验证时, Schannel SSP 将选择以下顺序的偏好随意选择一种协议：

-   传输层安全性 (TLS) 1.0 版本

-   传输层安全性 (TLS) 1.1 版本

-   传输层安全性 (TLS) 1.2 版

-   安全套接字层 (SSL) 版本 2.0

-   安全套接字层 (SSL) 版本 3.0

-   专用通信技术 （百分比）

    **注意**百分比默认处于禁用状态。

协议所选是可支持的客户端和服务器的身份验证的首选的协议。 例如，如果客户端仅支持 SSL 3.0 和 SSL 2.0 服务器支持所有 Schannel 协议，身份验证过程使用 SSL 3.0。

DTLS 时显式调用应用程序使用。 有关 DTLS 和习惯 Schannel 提供商提供的所有其他协议的详细信息，请参阅[Schannel 安全支持提供商的技术参考](../tls/schannel-security-support-provider-technical-reference.md)。

位置： %windir%\Windows\System32\Schannel.dll

默认情况下，在版本中指定包括该提供商**适用于**列表中的本主题，以及 Windows Server 2003 和 Windows XP 的开头。

> [!NOTE]
> 在此提供在 Windows Server 2008 R2 和 Windows 7 引入了 TLS 1.2。 在此提供在 Windows Server 2012 和 Windows 8 中引入了 DTLS。

**其他资源的 TLS 和 SSL 协议和 Schannel SSP**

-   [安全通道 (Windows)](https://msdn.microsoft.com/library/aa380123(VS.85).aspx)

-   [TLS 月 SSL 技术参考](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)

-   [\[MS-TLSP\]: 传输层安全性 (TLS) 配置文件](https://msdn.microsoft.com/library/dd207968(PROT.13).aspx)

### <a name="BKMK_NegoSSP"></a>协商安全性支持供应商
简单和保护 GSS-API 协商机制 （可） 个协商，构成的基础 whichcan 用于协商特定的身份验证的协议。 当应用连接到 SSPI 登录到某个网络时，它可以指定 SSP 处理该请求。 如果应用程序将指定协商 SSP，分析请求，并将选择相应的提供商以处理该请求，根据客户配置的安全策略。

可在 RFC 2478 中指定。

在受支持版本的 Windows 操作系统中，协商安全支持 Kerberos 协议和 NTLM 之间的提供商选择。 协商选择 Kerberos 协议默认情况下，除非的参与身份验证，系统之一不能使用该协议或调用应用未提供足够的信息，用于 Kerberos 协议。

位置： %windir%\Windows\System32\lsasrv.dll

默认情况下，在版本中指定包括该提供商**适用于**列表中的本主题，以及 Windows Server 2003 和 Windows XP 的开头。

**更多资源个协商**

-   [Microsoft 协商 (Windows)](https://msdn.microsoft.com/library/aa378748(VS.85).aspx)

-   [\[MS-SPNG\]: 简单且受保护 GSS-API 协商机制 （可） 扩展](https://msdn.microsoft.com/library/cc247021(PROT.13).aspx)

-   [\[MS-N2HT\]: 协商和 Nego2 HTTP 验证协议规范](https://msdn.microsoft.com/library/dd303576(PROT.13).aspx)

### <a name="BKMK_CredSSP"></a>凭据安全性支持供应商
启动新的和远程桌面服务终端服务会话时，凭据安全服务提供商 (CredSSP) 提供了一种登录 (SSO) 用户体验。 CredSSP 使目标基于到服务器 （通过服务器端 SSP) 客户端的策略应用程序用户的凭据从客户端计算机委派 （通过使用客户端 SSP）。 使用组策略配置 CredSSP 策略和的凭据委派默认情况下已关闭。

位置： %windir%\Windows\System32\credssp.dll

默认情况下，在版本中指定包括该提供商**适用于**开头的本主题列表。

**更多资源个凭据**

-   [\[MS-CSSP\]: 凭据安全支持的提供商 (CredSSP) 协议规范](https://msdn.microsoft.com/library/cc226764(PROT.13).aspx)

-   [对于终端凭据安全服务提供商和 SSO 服务登录](https://technet.microsoft.com/library/cc749211(v=ws.10).aspx)

### <a name="BKMK_NegoExtsSSP"></a>协商扩展安全性支持供应商
协商扩展 (NegoExts) 是身份验证程序包协商 Ssp，NTLM 或 Kerberos 协议，之外使用的应用程序和方案实现由 Microsoft 和其他软件公司。

此扩展到协商程序包允许以下情况下：

-   **丰富客户端的可用性联盟系统内。** 可以在 SharePoint 网站上访问文档，可以使用齐全的 Office 应用程序编辑。

-   **Microsoft Office 服务的支持丰富的客户端。** 用户可以在登录到 Microsoft Office 服务，并使用齐全的 Office 应用程序。

-   **托管的 Microsoft Exchange Server 和 Outlook。** 没有任何建立因为 Exchange 服务器在 web 上的域信任。 Outlook 使用 Windows Live 服务用户进行身份验证。

-   **之间的客户端和服务器丰富客户端可用性。** 使用操作系统的网络连接并验证组件。

Kerberos 和 NTLM 一样，Windows 协商包将 NegoExts SSP 处理方式相同。 NegoExts.dll 加载到在启动时的系统本地机构 (LSA)。 收到身份验证请求后，基于请求源 NegoExts 协商受支持的 Ssp 之间。 它会收集凭据和策略，加密它们，并将该信息发送到适当 SSP，创建安全标记的位置。

受 NegoExts Ssp 不可独立 Ssp Kerberos 和 NTLM 等。 因此内 NegoExts SSP 中，, 当身份验证方法发生故障时的任何伤害，身份验证失败消息显示或登录。 可能会没有重新协商或回退身份验证方法。

位置： %windir%\Windows\System32\negoexts.dll

默认情况下，在版本中指定包括该提供商**适用于**开头的本主题中，不包括 Windows Server 2008 和 Windows Vista 的列表。

### <a name="BKMK_PKU2USSP"></a>PKU2U 安全性支持供应商
PKU2U 协议已引入，而作为 Windows 7 和 Windows Server 2008 R2 SSP 实现。 此 SSP 使对等身份验证，尤其是通过媒体和文件共享称为家庭组，在 Windows 7 引入了功能。 该功能将允许不是属于某个域中的计算机之间共享。

位置： %windir%\Windows\System32\pku2u.dll

默认情况下，在版本中指定包括该提供商**适用于**开头的本主题中，不包括 Windows Server 2008 和 Windows Vista 的列表。

**更多资源 PKU2U 协议和 PKU2U SSP**

-   [引入联机身份集成](https://technet.microsoft.com/library/dd560662(v=ws.10).aspx)

## <a name="BKMK_SecuritySupportProviderSelection"></a>安全支持的提供商所选内容
Windows SSPI 可以使用任何支持完成安装安全支持的提供商的协议。 但是，并非所有操作系统都支持运行 Windows Server 任何给定计算机 SSP 包，因为客户端和服务器必须达成使用它们都支持协议。 Windows Server 倾向于客户端计算机和应用程序使用 Kerberos 协议，强标准基于协议，当可行的但是操作系统会继续允许客户端计算机和客户端应用程序不支持 Kerberos 协议进行身份验证。

身份验证拍摄位置二者通信之前计算机必须达成的协议，它们均可支持。 对于任何要通过 SSPI 可用的协议，必须相应 SSP 每台计算机。 例如的客户端计算机和 server 使用 Kerberos 身份验证协议，它们都必须支持 Kerberos 第 v5。 Windows Server 使用函数**EnumerateSecurityPackages**找出哪些 Ssp 受支持的计算机和在这些 Ssp 的功能是。

可以通过以下两种方式之一处理的身份验证协议所选内容：

1.  [身份验证的单个协议](#BKMK_SingleAuth)

2.  [协商选项](#BKMK_Negotiate)

### <a name="BKMK_SingleAuth"></a>身份验证的单个协议
在服务器上指定的单个接受协议后，客户端计算机必须支持指定的协议或通信失败。 指定单个接受协议后，身份验证发生交换如下所示：

1.  客户端计算机请求服务的访问权限。

2.  服务器回复该请求，并指定将使用的协议。

3.  客户端计算机检查回复和检查，以确定它是否支持指定的协议的内容。 如果客户端计算机支持指定的协议，将继续进行身份验证。 如果客户端计算机不支持协议，身份验证无法正常工作，无论是否客户端计算机是否有权访问资源。

### <a name="BKMK_Negotiate"></a>协商选项
协商选项可用于允许客户端和服务器以尝试查找接受的协议。 这取决于简单和保护 GSS-API 协商机制 （可）。 身份验证开头协商身份验证协议的选项，当可发生交换如下所示：

1.  客户端计算机请求服务的访问权限。

2.  服务器的身份验证协议，它可以支持身份验证质询或基于为其首选项的协议的响应列表回复。 例如，服务器可能列出 Kerberos 协议和 NTLM，并发送 Kerberos 身份验证响应。

3.  客户端计算机检查回复并检查，以确定它是否支持指定任何的协议内容。

    -   如果客户端计算机支持首选的协议，身份验证过程。

    -   如果客户端计算机不支持首选的协议，但它列出服务器协议之一支持，客户端计算机使服务器知道哪个身份验证支持的协议，并身份验证过程。

    -   如果不支持的客户端计算机任何列出的协议，身份验证 exchange 无法正常工作。

## <a name="see-also"></a>请参阅
[Windows 身份验证建筑](https://technet.microsoft.com/library/dn169024(v=ws.10).aspx)


