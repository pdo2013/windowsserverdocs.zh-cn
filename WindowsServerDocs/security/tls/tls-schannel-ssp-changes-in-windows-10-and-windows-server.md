---
title: TLS (Schannel SSP)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 05/16/2018
ms.openlocfilehash: 030fd81e0c6ba0423f1fa73e680006766cf2b180
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890768"
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>Windows 10 和 Windows Server 2016 中的 TLS (Schannel SSP) 更改

>适用于：Windows Server （半年频道）、 Windows Server 2016 和 Windows 10

## <a name="cipher-suite-changes"></a>密码套件的更改

Windows 10，版本 1511年和 Windows Server 2016 将添加对配置的密码套件顺序使用移动设备管理 (MDM) 的支持。

有关密码套件优先级顺序更改，请参阅[Schannel 中的密码套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)。

添加了的对以下密码套件：

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) 在 Windows 10 版本 1507年和 Windows Server 2016
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) 在 Windows 10 版本 1507年和 Windows Server 2016

DisabledByDefault 更改以下密码套件：

- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) 在 Windows 10，版本 1703年
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) 在 Windows 10，版本 1703年
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) 在 Windows 10，版本 1703年
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) 在 Windows 10，版本 1703年
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) 在 Windows 10，版本 1703年
- 在 Windows 10 版本 1709 TLS_RSA_WITH_RC4_128_SHA
- 在 Windows 10 版本 1709 TLS_RSA_WITH_RC4_128_MD5

从 Windows 10，版本 1507年和 Windows Server 2016 开始，默认情况下支持 SHA 512 证书。

### <a name="rsa-key-changes"></a>RSA 密钥更改

Windows 10 版本 1507年和 Windows Server 2016 将添加客户端 RSA 密钥大小的注册表配置选项。

有关详细信息，请参阅[KeyExchangeAlgorithm-客户端 RSA 密钥大小](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes)。

### <a name="diffie-hellman-key-changes"></a>Diffie-hellman 密钥更改

Windows 10 版本 1507年和 Windows Server 2016 将添加的 Diffie-hellman 密钥大小的注册表配置选项。

有关详细信息，请参阅[KeyExchangeAlgorithm-Diffie-hellman 密钥大小](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes)。

### <a name="schusestrongcrypto-option-changes"></a>SCH_USE_STRONG_CRYPTO 选项进行的更改

使用 Windows 10 版本 1507年和 Windows Server 2016 [SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx)选项现在禁用 NULL，MD5，DES，并导出密码。

## <a name="elliptical-curve-changes"></a>椭圆曲线更改

Windows 10 版本 1507年和 Windows Server 2016 将添加的计算机配置下的椭圆曲线组策略配置 > 管理模板 > 网络 > SSL 配置设置。 ECC 曲线订单列表指定椭圆曲线首选的顺序以及实现支持的曲线不启用。 
 
添加了的对下列椭圆曲线：

- BrainpoolP256r1 (RFC 7027) 在 Windows 10 版本 1507年和 Windows Server 2016
- BrainpoolP384r1 (RFC 7027) 在 Windows 10 版本 1507年和 Windows Server 2016 
- BrainpoolP512r1 (RFC 7027) 在 Windows 10 版本 1507年和 Windows Server 2016
- Curve25519 (RFC 草稿-ietf-tls-curve25519) 在 Windows 10，版本 1607年和 Windows Server 2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>针对 SealMessage & UnsealMessage 调度级支持

Windows 10 版本 1507年和 Windows Server 2016 在调度级别添加对 SealMessage/UnsealMessage 的支持。

## <a name="dtls-12"></a>DTLS 1.2

Windows 10，版本 1607年和 Windows Server 2016 将添加对 DTLS 1.2 (RFC 6347) 的支持。

## <a name="httpsys-thread-pool"></a>HTTP。SYS 线程池 

Windows 10，版本 1607年和 Windows Server 2016 将添加用于处理 http 的 TLS 握手的线程池大小的注册表配置。SYS。

注册表路径： 

HKLM\SYSTEM\CurrentControlSet\Control\LSA

若要指定每个 CPU 内核的最大线程池大小，创建**MaxAsyncWorkerThreadsPerCpu**条目。 默认情况下，注册表中不存在此项。 创建该项后，请将 DWORD 值更改为所需大小。 如果未配置，最大值是每个 CPU 内核 2 个线程。

## <a name="next-protocol-negotiation-npn-support"></a>下一步协议协商 （npn 型） 支持

从 Windows 10 版本 1703年开始，npn 下一步协议协商 （型） 已被删除，不再受支持。

## <a name="pre-shared-key-psk"></a>预共享的密钥 (PSK)

Windows 10，版本 1607年和 Windows Server 2016 将添加对 PSK 密钥交换算法 (RFC 4279) 的支持。

添加了的对以下 PSK 密码套件：

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) 在 Windows 10，版本 1607年和 Windows Server 2016
- 在 Windows 10，版本 1607年和 Windows Server 2016 TLS_PSK_WITH_AES_256_CBC_SHA384(RFC 5487)
- TLS_PSK_WITH_NULL_SHA256 (RFC 5487) 在 Windows 10，版本 1607年和 Windows Server 2016
- TLS_PSK_WITH_NULL_SHA384 (RFC 5487) 在 Windows 10，版本 1607年和 Windows Server 2016
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) 在 Windows 10，版本 1607年和 Windows Server 2016
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) 在 Windows 10，版本 1607年和 Windows Server 2016

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>会话恢复，而无需服务器端状态的服务器端的性能改进

Windows 10 版本 1507年和 Windows Server 2016 提供的会话票证与 Windows Server 2012 相比每秒 30%的会话上下文。

## <a name="session-hash-and-extended-master-secret-extension"></a>会话哈希和扩展的主密码扩展

Windows 10 版本 1507年和 Windows Server 2016 将添加对 RFC 7627 的支持：传输层安全性 (TLS) 会话哈希和扩展主机机密扩展。

由于此更改，导致 Windows 10 和 Windows Server 2016 需要第三方[CNG SSL 提供程序](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx)更新以支持 NCRYPT_SSL_INTERFACE_VERSION_3，并介绍了此新接口。


## <a name="ssl-support"></a>SSL 支持

从 Windows 10，版本 1607年和 Windows Server 2016 开始，TLS 客户端和服务器 SSL 3.0 默认情况下禁用。 这意味着，除非应用程序或服务专门请求通过 SSPI SSL 3.0，否则客户端将永远不会提供或接受 SSL 3.0，然后服务器将永远不会选择 SSL 3.0。

从 Windows 10 版本 1607年和 Windows Server 2016 开始，SSL 2.0 已删除，不再受支持。

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>对 Windows TLS 遵守其不符合标准的 TLS 客户端的连接的 TLS 1.2 要求的更改

TLS 1.2，在客户端使用["signature_algorithms"扩展](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1)以向服务器表明哪些签名/哈希算法对可能使用的数字签名 （即，服务器证书和服务器密钥交换）。 TLS 1.2 RFC 还需要服务器证书消息遵循"signature_algorithms"扩展：

"如果客户端提供"signature_algorithms"扩展，然后由服务器提供的所有证书必须进行都签名通过该扩展中将显示一个哈希/签名算法对。"

在实践中，某些第三方 TLS 客户端不符合要包括所有的签名和哈希算法对它们是愿意接受"signature_algorithms"扩展中的 TLS 1.2 RFC 和失败或完全省略分机号 （后者表示到在服务器上的客户端仅支持 SHA1 与 RSA、 DSA 或 ECDSA）。

TLS 服务器通常仅有一个配置每个终结点，这意味着服务器不能始终提供满足客户端的要求的证书的证书。

之前的 Windows 10 和 Windows Server 2016 中，Windows TLS 堆栈严格遵守 TLS 1.2 RFC 要求，不符合 RFC 的 TLS 客户端和互操作性问题，导致连接失败。 在 Windows 10 和 Windows Server 2016 中，被放松约束和该服务器可发送不符合 TLS 1.2 RFC 的证书，如果是服务器的唯一选项。 然后，客户端可能继续或终止在握手。

在验证服务器和客户端证书时，Windows TLS 堆栈严格符合 TLS 1.2 RFC，并在服务器和客户端证书中仅允许协商的签名和哈希算法。


