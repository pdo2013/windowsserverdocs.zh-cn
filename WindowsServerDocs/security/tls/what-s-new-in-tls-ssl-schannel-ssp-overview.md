---
title: TLS 的 SSL (Schannel SSP) 概述
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c8836345-16bb-4dcc-8d2b-2b9b687456a3
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 05/16/2018
ms.openlocfilehash: b1af556200c9dd497bac835f1480479cca075dab
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447282"
---
# <a name="overview-of-tls---ssl-schannel-ssp"></a>TLS 的 SSL (Schannel SSP) 的概述

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows 10

面向 IT 专业人员的本主题介绍的功能在 Schannel 安全支持提供程序 (SSP)，其中包括传输层安全性 (TLS)、 安全套接字层 (SSL) 和数据报传输层安全性 (DTLS) 更改身份验证协议，为 Windows Server 2012 R2、 Windows Server 2012、 Windows 8.1 和 Windows 8。

Schannel 是实现 SSL、TLS 和 DTLS Internet 标准身份验证协议的安全支持提供程序 (SSP)。 安全支持提供程序接口 (SSPI) 是 Windows 系统用于执行安全相关功能（包括身份验证）的 API。 SSPI 用作几个安全支持提供程序 (SSP)（包括 Schannel SSP）的通用接口。

TLS 和 SSL 在 Schannel SSP 中的 Microsoft 实现的详细信息，请参阅[TLS/SSL 技术参考 (2003)](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)。


## <a name="tlsssl-schannel-ssp-features"></a>TLS/SSL (Schannel SSP) 功能
下面描述了 Schannel SSP 中的 TLS 的功能

### <a name="tls-session-resumption"></a>TLS 会话恢复
传输层安全性 (TLS) 协议是 Schannel 安全支持提供程序的一个组成部分，用于保护跨不受信任的网络在应用程序之间发送的数据。 TLS/SSL 可以用于对服务器和客户端计算机进行身份验证，以及对经身份验证的参与方之间的消息进行加密。

将 TLS 连接到服务器的设备经常由于会话到期而需要重新连接。 Windows 8.1 和 Windows Server 2012 R2 现在支持 RFC 5077 （而无需服务器端状态 TLS 会话恢复）。 此修改提供了与 Windows Phone 和 Windows RT 设备：

-   在服务器上减少了资源使用率

-   减少了带宽，这可提高客户端连接的效率

-   减少了由于连接恢复而在 TLS 握手方面花费的时间。

> [!NOTE]
> Windows 8 中新增了 RFC 5077 的客户端实现。

有关无状态 TLS 会话恢复的信息，请参阅 IETF 文档 [RFC 5077](http://www.ietf.org/rfc/rfc5077)。

### <a name="application-protocol-negotiation"></a>应用程序协议协商
 Windows Server 2012 R2 和 Windows 8.1 支持客户端 TLS 应用程序协议协商，因此应用程序可以利用在 HTTP 2.0 标准开发的一部分的协议和用户可以访问联机服务，如 Google 和 Twitter 使用运行的应用SPDY 协议。

**其工作原理**

客户端和服务器应用程序通过提供支持的应用程序协议 ID 列表（按首选项的降序排列），来实现应用程序协议协商扩展。 TLS 客户端在 ClientHello 消息中指示，它通过在客户端支持的协议列表中包含应用层协议协商 (ALPN) 扩展来支持应用程序协议协商。

当 TLS 客户端向服务器进行请求时，TLS 服务器会在其支持的协议列表中读取客户端也支持的最首选的应用程序协议。 如果找到这类协议，则服务器使用所选协议 ID 进行响应并像往常一样继续进行握手。 如果没有公用的应用程序协议，则服务器会发送严重握手失败警报。

### <a name="BKMK_TrustedIssuers"></a>客户端身份验证的受信任颁发者的管理
需要使用 SSL 或 TLS 进行客户端计算机的身份验证时，服务器可以配置为发送受信任证书颁发者的列表。 此列表包含服务器将信任的证书颁发者集合，可提示客户端计算机要选择哪个客户端证书（如果存在多个证书）。 此外，必须根据配置的受信任颁发者列表来验证客户端计算机发送到服务器的证书链。

在 Windows Server 2012 和 Windows 8 之前, 应用程序或进程，使用 Schannel SSP （包括 HTTP.sys 和 IIS） 可以提供它们支持用于客户端身份验证通过证书信任列表 (CTL) 的受信任颁发者的列表。

在 Windows Server 2012 和 Windows 8 中，对基础身份验证过程已进行了更改，以便：

-   不再支持基于 CTL 的受信任颁发者列表管理。

-   发送受信任颁发者列表的行为在默认情况下是关闭的：SendTrustedIssuerList 注册表项的默认值现在是 0（默认情况下关闭）而不是 1。

-   保留与以前版本的 Windows 操作系统的兼容性。

**这增添了什么价值？**

从 Windows Server 2012 开始，CTL 的使用具有已替换为证书基于存储的实现。 这样可以通过 PowerShell 提供程序的现有证书管理 commandlet 以及命令行工具（如 certutil.exe）实现更熟悉的可管理性。

虽然 Schannel SSP 支持 (16 KB) 的受信任的证书颁发机构列表的最大大小保持与 Windows Server 2008 R2 中的相同，但 Windows Server 2012 中还有一个新的专用的证书存储用于客户端身份验证颁发者，以便不相关的证书不包含在消息中。

**它的工作原理？**

Windows Server 2012 中使用的证书存储; 配置受信任的颁发者列表一个默认全局计算机证书存储区，每个站点可选的另一个。 按如下方式确定列表的源：

-   如果为站点配置了特定凭据存储，则它会用作源

-   如果应用程序定义的存储中不存在任何证书，则 Schannel 会检查本地计算机上的“客户端身份验证颁发者”  存储，如果证书存在，则使用该存储作为源。 如果未在任一存储中找到证书，则检查受信任根存储。

-   如果全局和本地存储都不包含证书，则 Schannel 提供程序将使用**受信任的根证书颁发机构**存储作为受信任的颁发者列表的源。 （这是 Windows Server 2008 R2 的行为）。

如果**受信任的根证书颁发机构**存储包含根 （自签名） 和证书颁发机构 (CA) 颁发者证书的混合，则默认情况下，仅将 CA 颁发者证书将发送到服务器。

**如何配置 Schannel 以将证书存储用于受信任颁发者**

Windows Server 2012 中的 Schannel SSP 体系结构将默认使用上文所述的存储管理受信任的颁发者列表。 你仍可以使用 PowerShell 提供程序的现有证书管理 commandlet 以及命令行工具（如 Certutil）来管理证书。

有关使用 PowerShell 提供程序管理证书的信息，请参阅 [Windows 中的 AD CS 管理 Cmdlet](https://technet.microsoft.com/library/hh848365(v=wps.620).aspx)。

有关使用证书实用工具管理证书的信息，请参阅 [certutil.exe](https://technet.microsoft.com/library/cc732443.aspx)。

有关为 Schannel 凭据定义的数据（包括应用程序定义的存储）的信息，请参阅 [SCHANNEL_CRED 结构 (Windows)](https://msdn.microsoft.com/library/windows/desktop/aa379810(v=vs.85).aspx)。

**用于信任模式的默认值**

Schannel 提供程序支持三种客户端身份验证信任模式。 信任模式控制如何执行验证的客户端的证书链和由 REG_DWORD"clientauthtrustmode"HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel 下进行控制的系统范围设置.

|值|信任模式|描述|
|-----|-------|--------|
|0|计算机信任（默认值）|需要由受信任颁发者列表中的证书颁发客户端证书。|
|1|独占根信任|需要客户端证书链接到调用方指定的受信任颁发者存储中包含的根证书。 也必须由受信任颁发者列表中的颁发者颁发证书|
|2|独占 CA 信任|需要客户端证书链接到中间 CA 证书或调用方指定的受信任颁发者存储中的根证书。|

有关由于受信任颁发者配置问题而导致的身份验证失败的信息，请参阅知识库文章 [280256](https://support.microsoft.com/kb/2802568)。

### <a name="BKMK_SNI"></a>对服务器名称指示符 (SNI) 扩展的 TLS 支持
服务器名称指示符功能扩展了 SSL 和 TLS 协议，以便在单台服务器上运行大量虚拟映像时正确标识服务器。 为了正确保护客户端计算机和服务器之间的通信安全，客户端计算机将从服务器请求数字证书。 服务器响应请求并发送证书后，客户端计算机将进行检查，将其用于加密通信，然后继续执行正常的请求-响应交换。 但是，在虚拟主机托管方案中，几个域（都具有自己的可能不同证书）将托管在服务器上。 在这种情况下，服务器无法提前了解要发送给客户端计算机的证书。 SNI 让客户端计算机可在协议早期通知目标域，且这让服务器可以正确选择相应证书。

**这增添了什么价值？**

此附加功能：

-   让你可以在单个 IP 和端口组合中托管多个 SSL 网站

-   在单台 Web 服务器上托管多个 SSL 网站时，减少内存使用

-   让更多用户可同时连接到我的 SSL 网站

-   让你可以通过计算机接口向最终用户提供提示，以便在身份验证过程中选择正确的证书。

**其工作原理**

Schannel SSP 维护客户端允许的客户端连接状态的内存缓存。 这让客户端计算机可以快速重新连接到 SSL 服务器，而不受后续访问时的完全 SSL 握手影响。  证书管理这种高效使用允许更多网站托管在单个 Windows Server 2012 相对于以前的操作系统版本上。

通过让你可以构造向最终用户提供有关选择哪个名称的提示的可能证书颁发者名称列表，增强了最终用户的证书选择。 此列表可使用组策略配置。

### <a name="BKMK_DTLS"></a>数据报传输层安全性 (DTLS)
DTLS 版本 1.0 协议已添加到 Schannel 安全支持提供程序。 DTLS 协议提供有关数据报协议的通信隐私。 此协议让客户端/服务器应用程序可采用旨在防止窃听、篡改或消息伪造的方式进行通信。 DTLS 协议基于传输层安全 (TLS) 协议并提供等效的安全保证，从而减少使用 IPsec 或设计自定义应用程序层安全协议的需求。

**这增添了什么价值？**

数据报是常见的流媒体，如游戏或受保护视频会议。 将 DTLS 协议添加到 Schannel 提供程序后，你就可以使用熟悉的 Windows SSPI 模型保护客户端计算机和服务器之间的通信安全。 DTLS 专门设计为尽可能类似于 TLS，这既可将新安全发明降到最低，又可最大化代码和基础结构重复使用量。

**其工作原理**

通过 UDP 使用 DTLS 的应用程序可以使用 Windows Server 2012 和 Windows 8 中的 SSPI 模型。 某些加密套件可用于配置，这类似于 TLS 配置方式。 Schannel 继续使用 CNG 加密提供程序，从而利用 Windows Vista 中引入的 FIPS 140 认证。

### <a name="BKMK_Deprecated"></a>弃用的功能
在 Windows Server 2012 和 Windows 8 的 Schannel SSP，有无已弃用的功能。

## <a name="see-also"></a>请参阅
-   [私有云安全模型-包装程序功能](https://social.technet.microsoft.com/wiki/contents/articles/6756.private-cloud-security-model-wrapper-functionality.aspx)



