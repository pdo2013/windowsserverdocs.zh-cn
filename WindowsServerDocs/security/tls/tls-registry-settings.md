---
title: "传输层安全性 (TLS) 注册表设置"
description: "Windows Server 安全"
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
ms.date: 08/07/2017
ms.openlocfilehash: 8ccfacc367a5d32438bebf22798479a07f6cbdfc
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="transport-layer-security-tls-registry-settings"></a>传输层安全性 (TLS) 注册表设置

>适用于：Windows Server（半年通道），Windows Server 2016，Windows Server 2008R2、Windows Server 2008、Windows 10，Windows 8.1、Windows 8、Windows 7、WindowsVista

IT 专业人员的此参考主题包含传输层安全性 (TLS) 协议和安全套接字层 (SSL) 协议通过 Schannel 安全支持提供商 (SSP) 的 Windows 中执行的受支持的注册表设置信息。 注册表子项和条目中本主题帮助你管理并解决相应 Schannel SSP，述专门 TLS 和 SSL 协议。 

>[!Caution]
>作为参照，以使用疑难解答或验证所需的设置将应用时提供的此信息。 我们建议你执行不直接编辑注册表除非没有没有其他选择。
>对注册表进行修改不验证注册表编辑器或由 Windows 操作系统之前它们的应用。 因此，可以存储错误值，，这可能会导致无法恢复错误系统中。 如果可能，而不是通过直接，编辑注册表使用组策略或其他 Windows 工具如 Microsoft 管理控制台 (MMC) 来完成任务。 如果你必须编辑注册表，使用格外小心。 

## <a name="certificatemappingmethods"></a>CertificateMappingMethods 

此项中不存在注册表默认情况。 默认值为受支持，下面列出的所有四个证书地图方法。

当服务器应用程序需要客户端身份验证时、Schannel 会自动尝试地图提供的客户端计算机给用户帐户的证书。 可通过创建映射，它将证书信息对 Windows 的用户帐户相关联的身份客户端证书登录的用户进行身份验证。 创建并启用证书映射后，客户端出示客户端证书，每次你服务器应用程序会自动将关联该用户使用合适的 Windows 的用户帐户。

在大多数情况下，证书映射到一个用户帐户在两种方式之一： 

- 一个证书映射到一个用户帐户（一映射）中。
- 多个证书映射到一个用户帐户（许多到一个映射）。

默认情况下，Schannel 提供商还将使用优先顺序中列出的以下四个证书映射方法：

1. Kerberos 服务的用户 (S4U) 证书地图
2. 用户名主要映射
3. 一映射（也称为主题/发行商映射）
4. 许多到一个地图

适用的版本：当中指定**适用于**开头的本主题介绍的列表。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="ciphers"></a>密码

应通过配置密码套件订单控制 TLS 月 SSL 密码。 有关详细信息，请参阅[配置 TLS 密码 Suite 订单](manage-tls.md#configuring-tls-cipher-suite-order)。

有关 Schannel SSP 所使用的默认密码套件顺序的信息，请参阅[TLS 月 SSL (Schannel SSP) 中的密码套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)。 

##<a name="ciphersuites"></a>密码组

应完成配置 TLS 月 SSL 密码套件使用组策略、MDM 或 PowerShell，请参阅[配置 TLS 密码 Suite 订单](manage-tls.md#configuring-tls-cipher-suite-order)有关详细信息。

有关 Schannel SSP 所使用的默认密码套件顺序的信息，请参阅[TLS 月 SSL (Schannel SSP) 中的密码套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)。 


## <a name="clientcachetime"></a>ClientCacheTime

此项控制的操作系统以毫秒为单位到期客户端的缓存条目的时间。 安全连接缓存关闭值为 0。 此项中不存在注册表默认情况。 

首次连接到通过 Schannel SSP，完整服务器的客户端 TLS 月 SSL 握手执行。 完成后，主要机密、密码套件和证书存储在会话缓存各自的客户端和服务器上。

开始与 Windows Server 2008 和 Windows Vista，默认的客户端缓存时间是 10 小时。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

默认客户端缓存时间

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

此项控制遵从美国联邦信息处理 (FIPS)。 默认设置为 0。

适用的版本：当中指定**适用于**开头的本主题介绍的列表。 

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\LSA

Windows ServerFIPS 密码套件：查看[Schannel SSP 协议和支持密码套件](https://technet.microsoft.com/library/dn786419.aspx)。

## <a name="hashes"></a>哈希

应受 TLS 月 SSL 哈希算法配置密码套件订单。 请参阅[配置 TLS 密码 Suite 订单](manage-tls.md#configuring-tls-cipher-suite-order)有关详细信息。

## <a name="issuercachesize"></a>IssuerCacheSize

此项控制的发行商缓存中，大小，并使用与发行商映射。 尝试映射所有客户端的证书链中颁发 Schannel SSP-不仅直接发行商客户证书。 当颁发不映射到一个帐户，这是典型种情况，服务器可能尝试地图相同发行商重复，命名时间每秒数百。 

为了防止这种情况，服务器有底片缓存中，因此如果发行商名称没有映射到一个帐户，它将添加到缓存，并且 Schannel SSP 不会尝试地图才能再次的发行商名称过期的缓存条目。 此注册表项指定缓存大小。 此项中不存在注册表默认情况。 默认值为 100。 

适用的版本：当中指定**适用于**开头的本主题介绍的列表。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

此项控制缓存超时以毫秒为单位的时间间隔的长度。 尝试映射所有客户端的证书链中颁发 Schannel SSP-不仅直接发行商客户证书。 这种情况，位置颁发不映射到一个帐户，这是典型种情况，服务器可能会试图相同的地图发行商重复，命名时间每秒数百。

为了防止这种情况，服务器有底片缓存中，因此如果发行商名称没有映射到一个帐户，它将添加到缓存，并且 Schannel SSP 不会尝试地图才能再次的发行商名称过期的缓存条目。 此缓存保持为了提高性能，以便系统不会继续尝试相同颁发的地图。 此项中不存在注册表默认情况。 默认值为 10 分钟。

适用的版本：当中指定**适用于**开头的本主题介绍的列表。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>KeyExchangeAlgorithm 的客户端 RSA 键大小

此项控制的客户端 RSA 密钥大小。 

使用的关键 exchange 算法应该受配置密码套件订单。

在 Windows 10 版本 1507 年和 Windows Server 2016 添加。

注册表路径：HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

若要指定 TLS 客户端 RSA 最低受支持的范围关键位长度，创建**ClientMinKeyBitLength**条目。 此项中不存在注册表默认情况。 你已创建条目后，将 DWORD 值更改所需的位长。 如果没有配置，1024 位将最少。 

若要指定 TLS 客户端 RSA 最大受支持的范围关键位长度，创建**ClientMaxKeyBitLength**条目。 此项中不存在注册表默认情况。 你已创建条目后，将 DWORD 值更改所需的位长。 如果没有配置，将不会强制最多。

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>KeyExchangeAlgorithm-Diffie-hellman 关键大小

此项控制 Diffie-hellman 关键大小。 

使用的关键 exchange 算法应该受配置密码套件订单。

在 Windows 10 版本 1507 年和 Windows Server 2016 添加。

注册表路径：HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie Hellman

若要指定 TLS 客户端最低受支持范围的用量-Helman 关键位长度，创建**ClientMinKeyBitLength**条目。 此项中不存在注册表默认情况。 你已创建条目后，将 DWORD 值更改所需的位长。 如果没有配置，1024 位将最少。 
 
若要指定 TLS 客户端最大受支持范围的用量-Helman 关键位长度，创建**ClientMaxKeyBitLength**条目。 此项中不存在注册表默认情况。 你已创建条目后，将 DWORD 值更改所需的位长。 如果没有配置，将不会强制最多。 
 
若要指定 TLS 服务器默认用量 Helman 关键位长度，创建**ServerMinKeyBitLength**条目。 此项中不存在注册表默认情况。 你已创建条目后，将 DWORD 值更改所需的位长。 如果没有配置，2048 位将默认设置。 

## <a name="maximumcachesize"></a>MaximumCacheSize

此项控件的最大的缓存元素数。 设置为 0 MaximumCacheSize 禁用服务器端会话缓存，并阻止重新连接。 上方的默认值增加 MaximumCacheSize 导致 Lsass.exe 消耗更多内存。 每个会话缓存元素通常需要 2 到 4 KB 的内存。 此项中不存在注册表默认情况。 默认值为 20000 元素。 

适用的版本：当中指定**适用于**开头的本主题介绍的列表。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>消息 – 片段分析

________________________________________
此项控制允许大小将接受的零碎 TLS 握手消息的最大。 将不接受允许大小大于的邮件，并且 TLS 握手将失败。 以下项中不存在注册表默认情况。 

当你设置的值为 0x0 时，零碎的消息不处理，并将导致 TLS 握手失败。 这使得 TLS 客户端或服务器当前的计算机上使用 TLS Rfc 不兼容。 

最大大小可以增加最多 2 ^24 1 字节。 允许客户端或服务器阅读和存储大量从网络的未验证数据不是一个好主意，并且会消耗额外的每个安全上下文内存。 

在 Windows 7 和 Windows Server 2008 R2 添加。
有可用更新，可使 Internet Explorer Windows XP、Windows Vista 或 Windows Server 2008 分析零碎的 TLS 月 SSL 握手消息。

注册表路径：HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

若要指定允许最大大小的零碎 TLS 握手邮件接受 TLS 客户端将创建**MessageLimitClient**条目。 你已创建条目后，将 DWORD 值更改所需的位长。 如果没有配置，默认值将 0x8000 字节。 

若要指定允许最大大小的任何客户端身份验证时，将接受 TLS 服务器的零碎 TLS 握手邮件、创建**MessageLimitServer**条目。 你已创建条目后，将 DWORD 值更改所需的位长。 如果没有配置，默认值将 0x4000 字节。 

若要指定允许最大大小的 TLS 服务器会接受客户身份验证时的零碎 TLS 握手邮件、创建**MessageLimitServerClientAuth**条目。 你已创建条目后，将 DWORD 值更改所需的位长。 如果没有配置，默认值将 0x8000 字节。 

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

此项控件时发送列表中签发受信任的人使用的标记。 对于信任的客户端身份验证的证书颁发机构数百的服务器，有太多颁发的服务器，以便能够向他们发送所有向客户端计算机在客户端身份验证的请求。 在此情况下，可以设置此注册表项，并而非发送部分列表，Schannel SSP 将未发送任何列表中向客户端。

不发送的受信任的发行商列表，则可能会影响客户端发送时索要客户端证书。 例如，当 Internet Explorer 收到的客户端身份验证请求时，它仅显示链达之一服务器发送的证书颁发机构的客户端证书。 如果该服务器未发送的列表中，Internet Explorer 显示所有客户端安装的客户端证书。 

此问题可能是所需的。 例如，当 PKI 环境中包括交叉证书的客户端和服务器证书将不能同一根 CA;因此，Internet Explorer 无法选择了某个证书存在链接达服务器 Ca 之一。 通过配置服务器发送一个受信任的发行商列表，Internet Explorer 会发送其所有证书。

此项中不存在注册表默认情况。

默认发送受信任的发行商列表行为

| Windows 版本 | 时间 |
|-----------------|------|
| Windows Server 2012 和 Windows 8 或更高版本 | 假 |
| Windows Server 2008 R2 和 Windows 7 及更早版本 | 如此 |

适用的版本：当中指定**适用于**开头的本主题介绍的列表。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>ServerCacheTime

此项控制的以毫秒为单位过期服务器端缓存条目在操作系统所花费的时间。 值为 0 禁用服务器端会话缓存，并阻止重新连接。 上方的默认值增加 ServerCacheTime 导致 Lsass.exe 消耗更多内存。 每个会话缓存元素通常需要 2 到 4 KB 的内存。 此项中不存在注册表默认情况。 

适用的版本：当中指定**适用于**开头的本主题介绍的列表。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

默认服务器缓存时间：10 小时

## <a name="ssl-20"></a>SSL 2.0

此子项控制 SSL 2.0 的使用。

从 Windows 10 版本 1607 年和 Windows Server 2016 开始，SSL 2.0 已删除，并且不再受支持。
SSL 2.0 默认设置，请参阅[TLS 月 SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。 

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要启用 SSL 2.0 协议，创建**启用**相应子项中的条目。 此项中不存在注册表默认情况。 创建项后，请为 1 更改 DWORD 值。 若要禁用的协议，请将为 0 DWORD 值。

SSL 2.0 子项表

| 子项 | 描述 |
|--------|-------------|
| 客户端 | 控制 SSL 2.0 SSL 客户端上的使用。 |
| 服务器 | 控制 SSL 2.0 SSL 服务器上的使用。 |
| DisabledByDefault | 默认情况下禁用 SSL 2.0 此标记。 |

## <a name="ssl-30"></a>SSL 3.0

此子项控制 SSL 3.0 使用。

从 Windows 10 版本 1607 年和 Windows Server 2016 开始，SSL 3.0 已禁用默认情况。 SSL 3.0 默认设置，请参阅[TLS 月 SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。 

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要启用 SSL 3.0 协议，相应的一个创建启用条目。 此项中不存在注册表默认情况。 创建项后，请为 1 更改 DWORD 值。 若要禁用的协议，请将为 0 DWORD 值。

SSL 3.0 子项表

| 子项 | 描述 |
|--------|-------------|
| 客户端 | 控制 SSL 3.0 SSL 客户端上的使用。 |
| 服务器 | 控制 SSL 3.0 SSL 服务器上的使用。 |
| DisabledByDefault | 默认情况下禁用 SSL 3.0 此标记。 |

## <a name="tls-10"></a>TLS 1.0

此子项控制 TLS 1.0 的使用。

TLS 1.0 默认设置，请参阅看到[TLS 月 SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要禁用 TLS 1.0 协议，创建**启用**相应子项中的条目。 此项中不存在注册表默认情况。 创建项后，将 DWORD 值为 0。 若要启用的协议，请为 1 更改 DWORD 值。

TLS 1.0 子项表

| 子项 | 描述 |
|--------|-------------|
| 客户端 | 控制 TLS 1.0 TLS 客户端上的使用。 |
| 服务器 | 控制 TLS 1.0 TLS 服务器上的使用。 |
| DisabledByDefault | 默认情况下禁用 TLS 1.0 此标记。 |

## <a name="tls-11"></a>1.1 TLS

此子项控制 TLS 1.1 的使用。

第 1.1 TLS 默认设置，请参阅[TLS 月 SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

>[!Note] 
>对于 TLS 1.1 来处于启用状态并协商在运行 Windows Server 2008 R2 的服务器上，你必须创建**DisabledByDefault**相应子项（客户端，服务器）中的项目并将它设置为"0"。 在注册表中不会看到项目，并且默认设置为"1"。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要禁用 TLS 1.1 协议，创建**启用**相应子项中的条目。 此项中不存在注册表默认情况。 创建项后，将 DWORD 值为 0。 若要启用的协议，请为 1 更改 DWORD 值。

第 1.1 TLS 子项表

| 子项 | 描述 |
|--------|-------------|
| 客户端 | 控制 TLS 1.1 TLS 客户端上的使用。 |
| 服务器 | 控制 TLS 1.1 TLS 服务器上的使用。 |
| DisabledByDefault | 默认情况下禁用 TLS 1.1 此标记。 |

## <a name="tls-12"></a>TLS 1.2

此子项控制 TLS 1.2 的使用。

第 1.2 TLS 默认设置，请参阅[TLS 月 SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

>[!Note] 
>对于 TLS 1.2 处于启用状态并协商在运行 Windows Server 2008 R2 的服务器上，你必须创建**DisabledByDefault**相应子项（客户端，服务器）中的项目并将它设置为"0"。 在注册表中不会看到项目，并且默认设置为"1"。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要禁用 TLS 1.2 协议，创建**启用**相应子项中的条目。 此项中不存在注册表默认情况。 创建项后，将 DWORD 值为 0。 若要启用的协议，请为 1 更改 DWORD 值。

第 1.2 TLS 子项表

| 子项 | 描述 |
|--------|-------------|
| 客户端 | 控制 TLS 1.2 TLS 客户端上的使用。 |
| 服务器 | 控制 TLS 1.2 TLS 服务器上的使用。 |
| DisabledByDefault | 默认情况下禁用 TLS 1.2 此标记。 |

## <a name="dtls-10"></a>DTLS 1.0

此子项控制 DTLS 1.0 的使用。

DTLS 1.0 默认设置，请参阅[TLS 月 SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要禁用 DTLS 1.0 协议，创建**启用**相应子项中的条目。 此项中不存在注册表默认情况。 创建项后，将 DWORD 值为 0。 若要启用的协议，请为 1 更改 DWORD 值。

DTLS 1.0 子项表

| 子项 | 描述 |
|--------|-------------|
| 客户端 | 控制 DTLS 1.0 DTLS 客户端上的使用。 |
| 服务器 | 控制 DTLS 1.0 DTLS 服务器上的使用。 |
| DisabledByDefault | 默认情况下禁用 DTLS 1.0 此标记。 |

## <a name="dtls-12"></a>DTLS 1.2

此子项控制 DTLS 1.2 的使用。

第 1.2 DTLS 默认设置，请参阅[TLS 月 SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx)。

注册表路径：HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

若要禁用 DTLS 1.2 协议，创建**启用**相应子项中的条目。 此项中不存在注册表默认情况。 创建项后，将 DWORD 值为 0。 若要启用的协议，请为 1 更改 DWORD 值。

第 1.2 DTLS 子项表

| 子项 | 描述 |
|--------|-------------|
| 客户端 | 控制 DTLS 1.2 DTLS 客户端上的使用。 |
| 服务器 | 控制 DTLS 1.2 DTLS 服务器上的使用。 |
| DisabledByDefault | 默认情况下禁用 DTLS 1.2 此标记。 |

