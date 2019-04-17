---
title: "TLS-SSL (Schannel SSP) 概述"
description: "Windows Server 安全"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c8836345-16bb-4dcc-8d2b-2b9b687456a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: b846ed54a1f7c8ef7a85ea9f836ffa75d0036ae6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="overview-of-tls---ssl-schannel-ssp"></a>TLS-SSL (Schannel SSP) 的概述

>适用于：Windows Server（半年通道），Windows Server 2016

此主题以供 IT 专业人员功能在 Schannel 安全支持提供商 (SSP)，其中包括传输层安全性 (TLS)、安全套接字层 (SSL) 和数据报传输层安全性 (DTLS) 身份验证协议，对 Windows Server 2012 R2、Windows Server 2012、Windows 8.1 和 Windows 8 中介绍更改。

Schannel 是实现 SSL、 TLS 和 DTLS Internet 标准身份验证协议安全支持提供商 (SSP)。 安全支持提供程序接口 (SSPI) 是 Windows 系统用于执行安全相关的功能，包括身份验证的 API。 SSPI 充当几个安全支持提供商 (Ssp)，包括 Schannel SSP 常见接口

TLS 和 Schannel SSP 中的 SSL Microsoft 实现的详细信息，请参阅[TLS 月 SSL 技术参考 (2003)](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)。


##<a name="tlsssl-schannel-ssp-features"></a>TLS 月 SSL (Schannel SSP) 功能
下面介绍在 Schannel SSP TLS 的功能

### <a name="tls-session-resumption"></a>TLS 会话恢复
传输层安全性 (TLS) 协议、Schannel 安全支持供应商的组件用于保护信任网络跨应用程序之间发送的数据。 身份验证的服务器和客户端计算机和还加密经过身份验证的方之间的消息，则可以使用 TLS 月 SSL。

经常 TLS 连接到服务器的设备需要由于会话到期重新连接。 Windows 8.1 和 Windows Server 2012 R2 现在支持 RFC 5077（TLS 会话恢复不服务器端状态）。 此修改提供了 Windows Phone 和 Windows RT 设备：

-   在服务器上的降低的资源使用状况

-   减少提高效率的客户端连接的带宽

-   由于的连接 resumptions TLS 握手为花费的时间减少。

> [!NOTE]
> 在 Windows 8 中添加了 RFC 5077 的客户端实现。

有关无状态 TLS 会话恢复信息，请参阅 IETF 文档[RFC 5077。](http://www.ietf.org/rfc/rfc5077)

### <a name="application-protocol-negotiation"></a>应用程序协议协商
 Windows Server 2012R2 和 Windows 8.1 支持客户端 TLS 应用程序协议协调以便作为 HTTP 2.0 标准开发部分，应用程序可以利用协议和用户都可以访问联机服务，如 Google 和 Twitter 使用运行 SPDY 协议的应用。

**它的工作原理**

客户端和服务器的应用程序通过提供列出的受支持的应用程序协议 Id，按降序的偏好随意选择启用应用程序协议协商扩展。 TLS 客户端指示其支持应用程序协议协商通过 ClientHello 消息中包含的应用程序层协议协商 (ALPN) 扩展的客户端支持协议的列表。

当 TLS 客户端到服务器发出请求时，TLS 服务器读取它是受支持的协议的客户端还支持的大多数首选应用协议的列表。 如果找到这样一种协议、服务器将使用选定的协议 ID 响应，并继续像往常一样握手。 如果有任何常用应用协议，服务器发送致命握手失败警报。

### <a name="BKMK_TrustedIssuers"></a>受信任的发行商客户端身份验证的管理
需要使用 SSL 或 TLS 身份验证的客户端计算机时，可配置服务器发送受信任的证书颁发的列表。 此列表中包含的证书颁发者，这将信任服务器设置，并是对客户端计算机，并且要选择如果的客户端证书有多个证书存在的提示。 另外，客户端计算机发送到服务器的证书链必须验证配置签发受信任的人列表。

之前 Windows Server 2012 和 Windows 8，应用程序或使用（包括 HTTP.sys 和 IIS）Schannel SSP 的进程可能会提供他们的受支持的客户端身份验证通过证书信任列表 (CTL) 受信任颁发的列表。

在 Windows Server 2012 和 Windows 8，对进行了更改基础身份验证过程，以便：

-   基于 CTL 受信任的发行商列表中管理不再受支持。

-   默认情况下发送受信任的发行商列表中的行为处于关闭状态：SendTrustedIssuerList 注册表项的默认值现已 0（默认情况下关闭）而不是 1。

-   被保留到以前版本的 Windows 操作系统的兼容性。

**这添加哪些值？**

开始与 Windows Server 2012，CTL 使用已更换使用证书的官方商城实现。 这样更熟悉的可管理性通过现有的证书管理 commandlets PowerShell 提供商，以及等 certutil.exe 的命令行工具。

尽管受信任的证书颁发机构列表 Schannel SSP 支持 (以 KB 为 16) 的最大大小保持不变，如下所示 Windows Server 2008 R2、Windows Server 2012 有中新的专用证书为客户端身份验证颁发存储，以便无关的证书不包括在邮件中。

**它如何工作？**

Windows Server 2012，在使用证书官方商城; 配置受信任的发行商列表一个默认全球计算机证书官方商城和一项是可选的每个站点。 列表中的来源取决于如下所示：

-   如果该站点配置特定凭据官方商城，它将用作源

-   如果没有证书存在在应用程序定义商店中，然后检查 Schannel**客户端身份验证颁发**存储在本地计算机上，如果证书出现，将该应用商店用作源。 如果没有证书任一应用商店中找到，则检查根受信任的应用商店。

-   如果全球或本地商店包含证书，将使用 Schannel 提供商**受信任的根 Certifictation 机构**存储为受信任的发行商列表中的来源。 （这是 Windows Server 2008 R2 的行为。）

如果**受信任根 Certifictation 机构**所使用的应用商店中包含多种（自签名）根证书颁发机构颁发证书，默认情况下，仅 CA 发行商证书将发送到服务器。

**如何配置 Schannel 为受信任的发行商使用证书官方商城**

在 Windows Server 2012 的 Schannel SSP 体系结构将默认情况下使用商店如上所述管理受信任的发行商列表。 你仍然可以使用现有的证书管理 commandlets PowerShell 提供商，以及命令行工具，例如 Certutil 管理证书。

有关管理证书使用 PowerShell 提供程序的信息，请参阅[广告 Windows 中的客户服务管理 Cmdlet](https://technet.microsoft.com/library/hh848365(v=wps.620).aspx)。

有关管理证书使用该证书实用程序的信息，请参阅[certutil.exe](https://technet.microsoft.com/library/cc732443.aspx)。

哪些数据，包括应用程序定义的应用商店，定义 Schannel 凭据有关的信息，请参阅[SCHANNEL_CRED 结构 (Windows)](https://msdn.microsoft.com/library/windows/desktop/aa379810(v=vs.85).aspx)。

**默认信任模式**

有三种客户端身份验证信任模式 Schannel 提供支持。 信任模式控件的客户端的证书链的验证如何执行，并由 REG_DWORD"ClientAuthTrustMode"下 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel 控制系统范围的设置.

|值|信任模式|描述|
|-----|-------|--------|
|0|计算机信任（默认）|需要，由受信任的发行商列表中的证书颁发客户端证书。|
|1|专享根信任|客户端证书呼叫指定受信任的发行商存储中包含一个根证书链的要求。 此外必须颁发的发行商受信任的发行商列表中的证书|
|2|专享加利福尼亚信任|要求到中间 CA 证书或中调用方指定根证书的客户端的证书链受信任的发行商应用商店。|

身份验证失败由于受信任的发行商配置问题有关的信息，请参阅知识库文章[280256](https://support.microsoft.com/kb/2802568)。

### <a name="BKMK_SNI"></a>服务器指示器 (SNI) 扩展 TLS 支持
服务器名称指示功能扩展 SSL 和 TLS 协议大量虚拟映像一个服务器上运行时，允许正确服务器的标识。 若要正确安全之间客户端计算机和服务器的通信、客户端计算机从服务器请求数字证书。 服务器请求的响应，并发送证书后，客户端计算机检查它、使用它来加密的通信和继续正常请求响应更换费用。 但是，在虚拟宿主方案，多个域，每一个都有它自己可能不同的证书充当一个服务器上。 在此情况下，服务器有无法事先知道向客户端计算机发送该证书。 SNI 使客户端计算机以通知目标域前面的协议，并允许服务器正确选择正确的证书。

**这添加哪些值？**

此附加功能：

-   允许你拥有多个 SSL 网站上的单个 IP 和端口组合

-   当多个 SSL 网站位于单个 web 服务器减少内存使用情况

-   可使更多的用户，若要同时连接到我 SSL 网站

-   允许你为客户端身份验证过程中选择正确的证书提供给最终用户的计算机界面通过的提示。

**它的工作原理**

Schannel SSP 维护允许的客户端的客户端连接状态的内存中缓存。 这使客户端计算机以重新连接快速 SSL 服务器不完整的 SSL 握手遵循在以后访问。  证书管理此有效地使用允许为投放到以前的操作系统版本相比单个 Windows Server 2012 上的多个站点。

已允许你构建可能证书发行商名称时，最终用户的提示上要选择哪个选项的列表，从而增强证书最终用户的选择。 配置使用组策略，则此列表。

### <a name="BKMK_DTLS"></a>数据报传输层安全性 (DTLS)
已添加到 Schannel 安全支持的提供商 DTLS 1.0 版本协议。 DTLS 协议提供通信的数据报协议的隐私。 协议允许/服务器客户端应用程序进行通信，专门用于阻止窃取、篡改或消息伪造的方式。 DTLS 协议根据传输层安全性 (TLS) 协议，并提供等效的安全保障，减少了需要使用 IPsec 或设计的自定义应用程序的一层安全性协议。

**这添加哪些值？**

报共有流媒体，例如游戏或安全进行视频会议中。 添加 DTLS 协议 Schannel 提供商提供你能够使用在安全的之间的客户端和服务器通信熟悉的 Windows SSPI 模型。 DTLS 有意设计为尽可能相似 TLS 尽可能，使新的安全发明降至最低和，以便尽可能多的代码和基础结构重新使用。

**它的工作原理**

通过 UDP 使用 DTLS 的应用可使用在 Windows Server 2012 和 Windows 8 SSPI 模型。 某些密码套件有可供配置，类似于怎样配置 TLS。 Schannel 继续使用利用 FIPS 140 证书，这在 Windows Vista 中引入了 CNG 加密提供商。

### <a name="BKMK_Deprecated"></a>不建议使用的功能
Windows Server 2012 和 Windows 8 Schannel SSP，没有过时的功能。

## <a name="see-also"></a>请参阅
-   [专用云安全模型-包装功能](https://social.technet.microsoft.com/wiki/contents/articles/6756.private-cloud-security-model-wrapper-functionality.aspx)



