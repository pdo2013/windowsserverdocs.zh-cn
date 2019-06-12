---
title: 传输层安全性 (TLS) 注册表设置
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 02/28/2019
ms.openlocfilehash: 32068319aae7545675e126eed6e1ab4c914bcbcf
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812642"
---
# <a name="transport-layer-security-tls-registry-settings"></a>传输层安全性 (TLS) 注册表设置

>适用于：Windows Server （半年频道） 2019，Windows Server 2016 中，Windows 10 的 Windows 服务器

本参考主题面向 IT 专业人员包含传输层安全性 (TLS) 协议和通过 Schannel 安全支持安全套接字层 (SSL) 协议的 Windows 实现的受支持的注册表设置的信息提供程序 (SSP)。 注册表子项和此主题帮助你管理和疑难解答 Schannel SSP 中涉及的项专门的 TLS 和 SSL 协议。 

> [!CAUTION]
> 在排除故障或验证是否应用了必需的设置时，可参考本信息。
> 除非万不得已，否则建议不要直接编辑注册表。
> 在应用对注册表的修改之前，注册表编辑器或 Windows 操作系统不会对这些修改进行验证。
> 因此，可能会存储不正确的值，从而可能导致系统出现不可恢复的错误。
> 如有可能，请使用组策略或其他 Windows 工具（如 Microsoft 管理控制台 (MMC)）来完成任务，而不是直接编辑注册表。
> 如果必须编辑注册表，请格外小心。

## <a name="certificatemappingmethods"></a>CertificateMappingMethods 

默认情况下，注册表中不存在此项。 其默认值为支持下面列出的四种证书映射方法。

当服务器应用程序需要客户端身份验证时，Schannel 会自动尝试将客户端计算机提供的证书映射到用户帐户。 通过创建将证书信息与 Windows 用户帐户关联的映射，你可以对使用客户端证书登录的用户进行身份验证。 创建并启用证书映射后，客户端每次出示客户端证书时，服务器应用程序都会自动将该用户与相应的 Windows 用户帐户关联。

在大多数情况下，证书通过以下两种方式之一映射到用户帐户： 

- 单个证书映射到单个用户帐户（一对一映射）。
- 多个证书映射到一个用户帐户（多对一映射）。

默认情况下，Schannel 提供程序将使用以下四种证书映射方法（按优先顺序列出）：

1. Kerberos service-for-user (S4U) 证书映射
2. 用户主体名称映射
3. 一对一映射关系（也称为使用者/颁发者映射）
4. 多对一映射

适用的版本：本主题开头的“适用于”  列表中指定的版本。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="ciphers"></a>Ciphers

TLS/SSL 密码应由配置密码套件顺序控制。 有关详细信息，请参阅[配置 TLS 密码套件顺序](manage-tls.md#configuring-tls-cipher-suite-order)。

有关 Schannel SSP 所用的默认密码套件顺序的信息，请参阅[TLS/SSL (Schannel SSP) 中的密码套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)。 

## <a name="ciphersuites"></a>CipherSuites

应完成的配置 TLS/SSL 密码套件使用组策略、 MDM 或 PowerShell，请参阅[配置 TLS 密码套件顺序](manage-tls.md#configuring-tls-cipher-suite-order)有关详细信息。

有关 Schannel SSP 所用的默认密码套件顺序的信息，请参阅[TLS/SSL (Schannel SSP) 中的密码套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)。 


## <a name="clientcachetime"></a>ClientCacheTime

此项控制操作系统终止客户端缓存条目所需的时间（以毫秒为单位）。 值 0 表示关闭安全连接缓存。 默认情况下，注册表中不存在此项。 

客户端第一次通过 Schannel SSP 连接到服务器时，将执行完整的 TLS/SSL 握手。 完成握手后，主密钥、密码套件和证书将存储在各自的客户端和服务器上的会话缓存中。

从 Windows Server 2008 和 Windows Vista 开始，默认客户端缓存时间为 10 小时。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

默认客户端缓存时间

## <a name="enableocspstaplingforsni"></a>EnableOcspStaplingForSni

联机证书状态协议 (OCSP) 装订使 web 服务器，如 Internet 信息服务 (IIS)，以在 TLS 握手过程向客户端发送的服务器证书时提供当前服务器证书的吊销状态。 此功能可减少 OCSP 服务器上的负载，因为 web 服务器可以缓存服务器证书的当前 OCSP 状态并将其发送到多个 web 客户端。 如果没有此功能，每个 web 客户端会尝试从 OCSP 服务器检索服务器证书的当前 OCSP 状态。 这将生成该 OCSP 服务器上的高负载。 

除了 IIS 中，通过 http.sys 的 web 服务可以是有益的此设置，包括 Active Directory 联合身份验证服务 (AD FS) 和 Web 应用程序代理 (WAP)。 

默认情况下，对于具有简单的安全 (SSL/TLS) 绑定的 IIS 网站启用 OCSP 支持。 但是，默认情况下如果 IIS 网站使用一个或两个以下类型的安全 (SSL/TLS) 绑定未启用此支持：
- 需要服务器名称指示
- 使用集中式证书存储

在这种情况下，服务器你好响应在 TLS 握手期间将不在默认情况下包含 OCSP 装订状态。 此行为可以提高性能：Windows OCSP 装订实现可以扩展到数百个服务器证书。 因为 SNI 和 CCS 使 IIS 能够扩展到数千个可能有成千上万个服务器证书的网站，设置默认情况下启用此行为可能导致性能问题。

适用的版本：从 Windows Server 2012 和 Windows 8 的所有版本。 

注册表路径: [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL]

添加以下项：

"EnableOcspStaplingForSni"=dword:00000001

若要禁用，设置为 0 的 DWORD 值：

"EnableOcspStaplingForSni"=dword:00000000

> [!NOTE] 
> 启用此注册表项会产生潜在的性能影响。

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

此项控制美国联邦信息处理 (FIPS) 合规性。 默认值为 0。

适用的版本：从 Windows Server 2012 和 Windows 8 的所有版本。 

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\LSA

Windows Server FIPS 密码套件：请参阅[Schannel SSP 中支持的密码套件和协议](https://technet.microsoft.com/library/dn786419.aspx)。

## <a name="hashes"></a>Hashes

TLS/SSL 哈希算法应由配置密码套件顺序控制。 请参阅[配置 TLS 密码套件顺序](manage-tls.md#configuring-tls-cipher-suite-order)有关详细信息。

## <a name="issuercachesize"></a>IssuerCacheSize

此项控制颁发者缓存的大小，它与颁发者映射一起使用。 Schannel SSP 将尝试映射客户端证书链中的所有颁发者 — 而不只是客户端证书的直接颁发者。 如果颁发者未映射到帐户（这种情况很常见），服务器可能会尝试以每秒数百次的频率重复映射相同的颁发者名称。 

为防止这种情况，服务器有一个负缓存，因此，如果某个颁发者名称未映射到帐户，会将它添加到负缓存中，在该缓存条目到期之前，Schannel SSP 不会尝试再次映射该颁发者名称。 此注册表项指定缓存大小。 默认情况下，注册表中不存在此项。 默认值为 100。 

适用的版本：从 Windows Server 2008 和 Windows Vista 开始的所有版本。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

此项控制缓存超时间隔时长（以毫秒为单位）。 Schannel SSP 将尝试映射客户端证书链中的所有颁发者 — 而不只是客户端证书的直接颁发者。 如果颁发者未映射到帐户（这种情况很常见），服务器可能会尝试以每秒数百次的频率重复映射相同的颁发者名称。

为防止这种情况，服务器有一个负缓存，因此，如果某个颁发者名称未映射到帐户，会将它添加到负缓存中，在该缓存条目到期之前，Schannel SSP 不会尝试再次映射该颁发者名称。 出于性能方面的考虑，将保留此缓存，以便系统不继续尝试映射相同的颁发者。 默认情况下，注册表中不存在此项。 默认值为 10 分钟。

适用的版本：从 Windows Server 2008 和 Windows Vista 开始的所有版本。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>KeyExchangeAlgorithm-客户端 RSA 密钥大小

此项控制客户端的 RSA 密钥大小。 

应由配置密码套件顺序控制使用的密钥交换算法。

添加 Windows 10 版本 1507年和 Windows Server 2016。

注册表路径：HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

若要指定 RSA 最小支持的范围密钥位长度的 TLS 客户端，创建**ClientMinKeyBitLength**条目。 默认情况下，注册表中不存在此项。 创建该项后，请将 DWORD 值更改为所需的位长度。 如果未配置，1024 位将最小值。 

若要指定 RSA 最大受支持的范围密钥位长度的 TLS 客户端，创建**ClientMaxKeyBitLength**条目。 默认情况下，注册表中不存在此项。 创建该项后，请将 DWORD 值更改为所需的位长度。 如果未配置，将不会强制最大值。

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>KeyExchangeAlgorithm-Diffie-hellman 密钥大小

此项控制的 Diffie-hellman 密钥大小。 

应由配置密码套件顺序控制使用的密钥交换算法。

添加 Windows 10 版本 1507年和 Windows Server 2016。

注册表路径：HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman

若要指定最小支持范围的 Diffie-Helman 密钥位长度的 TLS 客户端，创建**ClientMinKeyBitLength**条目。 默认情况下，注册表中不存在此项。 创建该项后，请将 DWORD 值更改为所需的位长度。 如果未配置，1024 位将最小值。 
 
若要指定最大受支持范围的 Diffie-Helman 密钥位长度的 TLS 客户端，创建**ClientMaxKeyBitLength**条目。 默认情况下，注册表中不存在此项。 创建该项后，请将 DWORD 值更改为所需的位长度。 如果未配置，将不会强制最大值。 
 
若要指定 TLS 服务器默认值的 Diffie Helman 密钥位长度，创建**ServerMinKeyBitLength**条目。 默认情况下，注册表中不存在此项。 创建该项后，请将 DWORD 值更改为所需的位长度。 如果未配置，则将默认为 2048 位。 

## <a name="maximumcachesize"></a>MaximumCacheSize

此项控制缓存元素的最大数目。 如果将 MaximumCacheSize 设置为 0，将禁用服务器端会话缓存并阻止重新连接。 将 MaximumCacheSize 增大到超过默认值后，会导致 Lsass.exe 占用额外的内存。 每个会话缓存元素通常需要 2 到 4 KB 的内存。 默认情况下，注册表中不存在此项。 默认值为 20,000 个元素。 

适用的版本：从 Windows Server 2008 和 Windows Vista 开始的所有版本。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>消息传送 – 片段分析

________________________________________
此项控制允许的分片将接受的 TLS 握手消息大小的最大。 将不接受大于允许的大小的消息，并且在 TLS 握手将失败。 这些条目不存在在注册表中默认情况下。 

当您将值设置为 0x0 时，分片的消息不会处理，将导致在 TLS 握手失败。 这使得 TLS 客户端或服务器在当前计算机上符合 TLS Rfc 不符合。 

允许的最大大小可以增大最大 2 ^24-1 个字节。 允许读取和存储大量的网络中的未验证数据的客户端或服务器不是一个不错的主意，将会占用额外内存为每个安全上下文。 

添加到 Windows 7 和 Windows Server 2008 R2 中。
有可用的更新，使 Internet 资源管理器在 Windows XP、 Windows Vista 或 Windows Server 2008 分析分片的 TLS/SSL 握手消息。

注册表路径：HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

若要指定允许的 TLS 客户端将接受的碎片化 TLS 握手消息大小最大值，创建**MessageLimitClient**条目。 创建该项后，请将 DWORD 值更改为所需的位长度。 如果未配置，默认值将是 0x8000 字节。 

若要指定允许的 TLS 服务器将接受任何客户端身份验证时的分片的 TLS 握手消息大小最大值，创建**MessageLimitServer**条目。 创建该项后，请将 DWORD 值更改为所需的位长度。 如果未配置，默认值将是 0x4000 字节。 

若要指定允许的 TLS 服务器将接受客户端身份验证时的分片的 TLS 握手消息大小最大值，创建**MessageLimitServerClientAuth**条目。 创建该项后，请将 DWORD 值更改为所需的位长度。 如果未配置，默认值将是 0x8000 字节。 

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

此项控制发送可信颁发者列表时所用的标志。 如果服务器针对客户端身份验证信任数百个证书颁发机构，那么，对服务器而言，颁发者太多，无法在客户端计算机请求客户端身份验证时将这些颁发者全部发送到客户端计算机。 在这种情况下，可以设置此注册表项，Schannel SSP 将不向客户端发送任何列表，而不是发送部分列表。

不发送可信颁发者列表可能会影响要求客户端提供客户端证书时客户端所发送的内容。 例如，当 Internet Explorer 收到客户端身份验证请求时，它仅显示与服务器发送的最多一个证书颁发机构链接的客户端证书。 如果服务器未发送列表，Internet Explorer 将显示客户端上安装的所有客户端证书。 

某些情况下，此行为是可取的。 例如，当 PKI 环境包含交叉证书时，客户端证书和服务器证书不会有相同的根 CA；因此，Internet Explorer 不能选择与服务器的最多一个 CA 链接的证书。 通过将服务器配置为不发送可信颁发者列表，Internet Explorer 将发送其所有证书。

默认情况下，注册表中不存在此项。

默认发送受信任的颁发者列表行为

| Windows 版本 | Time |
|-----------------|------|
| Windows Server 2012 和 Windows 8 及更高版本 | FALSE |
| Windows Server 2008 R2 和 Windows 7 及更早版本 | TRUE |

适用的版本：从 Windows Server 2008 和 Windows Vista 开始的所有版本。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>ServerCacheTime

此项控制操作系统终止服务器端缓存条目所需的时间（以毫秒为单位）。 值 0 表示禁用服务器端会话缓存并阻止重新连接。 将 ServerCacheTime 增大到超过默认值后，会导致 Lsass.exe 占用额外的内存。 每个会话缓存元素通常需要 2 到 4 KB 的内存。 默认情况下，注册表中不存在此项。 

适用的版本：从 Windows Server 2008 和 Windows Vista 开始的所有版本。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

默认服务器缓存时间：10 小时

## <a name="ssl-20"></a>SSL 2.0

此子项控制使用 SSL 2.0。

从 Windows 10，版本 1607年和 Windows Server 2016 开始，SSL 2.0 已删除，不再受支持。
SSL 2.0 默认设置，请参阅[TLS/SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。 

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要启用 SSL 2.0 协议，创建**已启用**中的客户端或服务器子项下, 表中所述的条目。 默认情况下，注册表中不存在此项。 创建该项后，请将 DWORD 值更改为 1。 

SSL 2.0 子项表

| 子项 | 描述 |
|--------|-------------|
| 客户端 | 控制 SSL 客户端上的使用 SSL 2.0。 |
| Server | 控制 SSL 服务器上的使用 SSL 2.0。 |

若要为客户端或服务器禁用 SSL 2.0，请将 DWORD 值更改为 0。 如果 SSPI 应用请求时使用 SSL 2.0，它将被拒绝。 

若要禁用 SSL 2.0 默认情况下，创建**DisabledByDefault**条目和更改 DWORD 值为 1。 如果 SSPI 应用显式请求时使用 SSL 2.0，它可能进行协商。 

下面的示例演示在注册表中禁用 SSL 2.0:

![禁用 SSL 2.0](images/ssl-2-registry-setting.png)


## <a name="ssl-30"></a>SSL 3.0

此子项控制使用 SSL 3.0。

从 Windows 10，版本 1607年和 Windows Server 2016 开始，SSL 3.0 已禁用默认情况下。 SSL 3.0 默认设置，请参阅[TLS/SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。 

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要启用 SSL 3.0 协议，创建**已启用**中的客户端或服务器子项下, 表中所述的条目。  
默认情况下，注册表中不存在此项。 创建该项后，请将 DWORD 值更改为 1。 

SSL 3.0 子项表

| 子项 | 描述 |
|--------|-------------|
| 客户端 | 控制 SSL 客户端上的使用 SSL 3.0。 |
| Server | 控制 SSL 服务器上的使用 SSL 3.0。 |

若要为客户端或服务器禁用 SSL 3.0，请将 DWORD 值更改为 0。
如果 SSPI 应用请求时使用的 SSL 3.0，它将被拒绝。 

若要禁用 SSL 3.0，默认情况下，创建**DisabledByDefault**条目和更改 DWORD 值为 1。 如果 SSPI 应用显式请求时要使用 SSL 3.0，它可能进行协商。 

下面的示例演示在注册表中禁用 SSL 3.0:

![禁用 SSL 3.0](images/ssl-3-registry-setting.png)

## <a name="tls-10"></a>TLS 1.0

此子项控制使用 TLS 1.0。

TLS 1.0 默认设置，请参阅[TLS/SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要启用 TLS 1.0 协议，创建**已启用**下表中所述的客户端或服务器子项中的条目。 默认情况下，注册表中不存在此项。 创建该项后，请将 DWORD 值更改为 1。 

TLS 1.0 子项表

| 子项 | 描述 |
|--------|-------------|
| 客户端 | 控制 TLS 客户端上的使用 TLS 1.0。 |
| Server | 控制 TLS 服务器上的使用 TLS 1.0。 |

若要禁用 TLS 1.0 客户端或服务器，请将 DWORD 值更改为 0。
如果 SSPI 应用请求时使用 TLS 1.0，它将被拒绝。 

若要禁用 TLS 1.0，默认情况下，创建**DisabledByDefault**条目和更改 DWORD 值为 1。 如果 SSPI 应用显式请求时要使用 TLS 1.0，它可能进行协商。 

下面的示例演示在注册表中禁用 TLS 1.0:

![禁用 TLS 1.0](images/tls-registry-setting.png)

## <a name="tls-11"></a>TLS 1.1

此子项控制使用 TLS 1.1。

TLS 1.1 默认设置，请参阅[TLS/SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要启用 TLS 1.1 协议，创建**已启用**下表中所述的客户端或服务器子项中的条目。 默认情况下，注册表中不存在此项。 创建该项后，请将 DWORD 值更改为 1。 

TLS 1.1 子项表

| 子项 | 描述 |
|--------|-------------|
| 客户端 | 控制 TLS 客户端上的使用 TLS 1.1。 |
| Server | 控制 TLS 服务器上的使用 TLS 1.1。 |

若要禁用 TLS 1.1 的客户端或服务器，请将 DWORD 值更改为 0。
如果 SSPI 应用请求为使用 TLS 1.1，它将被拒绝。 

若要禁用 TLS 1.1，默认情况下，创建**DisabledByDefault**条目和更改 DWORD 值为 1。 如果 SSPI 应用显式请求为使用 TLS 1.1，它可能进行协商。 

下面的示例演示在注册表中禁用 TLS 1.1:

![禁用 TLS 1.1](images/tls-11-registry-setting.png)

## <a name="tls-12"></a>TLS 1.2

此子项控制使用 TLS 1.2。

TLS 1.2 默认设置，请参阅[TLS/SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要启用 TLS 1.2 协议，创建**已启用**下表中所述的客户端或服务器子项中的条目。 默认情况下，注册表中不存在此项。 创建该项后，请将 DWORD 值更改为 1。 

TLS 1.2 子项表

| 子项 | 描述 |
|--------|-------------|
| 客户端 | 控制 TLS 客户端上的使用 TLS 1.2。 |
| Server | 控制 TLS 服务器上的使用 TLS 1.2。 |

若要禁用 TLS 1.2 客户端或服务器，请将 DWORD 值更改为 0。
如果 SSPI 应用请求为使用 TLS 1.2，它将被拒绝。 

若要禁用 TLS 1.2，默认情况下，创建**DisabledByDefault**条目和更改 DWORD 值为 1。 如果显式请求为使用 TLS 1.2 的 SSPI 应用，它可能进行协商。 

下面的示例演示在注册表中禁用 TLS 1.2:

![禁用 TLS 1.2](images/tls-12-registry-setting.png)

## <a name="dtls-10"></a>DTLS 1.0

此子项控制使用 DTLS 1.0。

DTLS 1.0 默认设置，请参阅[TLS/SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要启用 DTLS 1.0 协议，创建**已启用**下表中所述的客户端或服务器子项中的条目。 默认情况下，注册表中不存在此项。 创建该项后，请将 DWORD 值更改为 1。 

DTLS 1.0 子项表

| 子项 | 描述 |
|--------|-------------|
| 客户端 | 控制使用 DTLS 1.0 DTLS 客户端上。 |
| Server | 控制使用 DTLS 1.0 DTLS 服务器上。 |

若要禁用 DTLS 1.0 客户端或服务器，请将 DWORD 值更改为 0。
如果 SSPI 应用请求使用 DTLS 1.0 时，它将被拒绝。 

若要禁用 DTLS 1.0，默认情况下，创建**DisabledByDefault**条目和更改 DWORD 值为 1。 如果 SSPI 应用显式请求时要使用 DTLS 1.0，它可能进行协商。 

下面的示例演示在注册表中禁用 DTLS 1.0:

![DTLS 1.0 已禁用](images/dtls-10-registry-setting.png)

## <a name="dtls-12"></a>DTLS 1.2

此子项控制使用 DTLS 1.2。

DTLS 1.2 默认设置，请参阅[TLS/SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要启用 DTLS 1.2 协议，创建**已启用**下表中所述的客户端或服务器子项中的条目。 默认情况下，注册表中不存在此项。 创建该项后，请将 DWORD 值更改为 1。 

DTLS 1.2 子项表

| 子项 | 描述 |
|--------|-------------|
| 客户端 | 控制 DTLS 客户端上的使用 DTLS 1.2。 |
| Server | 控制使用 DTLS 1.2 DTLS 服务器上。 |


若要禁用 DTLS 1.2 客户端或服务器，请将 DWORD 值更改为 0。
如果 SSPI 应用请求使用 DTLS 1.0 时，它将被拒绝。 

若要禁用 DTLS 1.2，默认情况下，创建**DisabledByDefault**条目和更改 DWORD 值为 1。 如果 SSPI 应用显式请求使用 DTLS 1.2 时，它可能进行协商。 

下面的示例演示在注册表中禁用 DTLS 1.1:

![DTLS 1.1 已禁用](images/dtls-11-registry-setting.png)


