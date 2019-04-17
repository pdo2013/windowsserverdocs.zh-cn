---
title: TLS (Schannel SSP)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 6e1a229bfc7063597f8994c71bbaec5637d084a4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>在 Windows 10 和 Windows Server 2016 更改 TLS (Schannel SSP)

>适用于：Windows Server 2016 和 Windows 10

## <a name="cipher-suite-changes"></a>密码套件更改

Windows 10 版本 1511 年，Windows Server 2016 添加密码套件订单使用移动设备管理 (MDM) 配置的支持。

密码套件优先级订单更改，请参阅[密码套件中 Schannel](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)。

添加了对以下密码套件的支持：

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) 在 Windows 10 版本 1507 年和 Windows Server 2016
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) 在 Windows 10 版本 1507 年和 Windows Server 2016

更改以下密码套件 DisabledByDefault:

- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) 在 Windows 10 中，1703 年版本
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) 在 Windows 10 中，1703 年版本
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) 在 Windows 10 中，1703 年版本
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) 在 Windows 10 中，1703 年版本
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) 在 Windows 10 中，1703 年版本
- 在 Windows 10 中，版本 1709 TLS_RSA_WITH_RC4_128_SHA
- 在 Windows 10 中，版本 1709 TLS_RSA_WITH_RC4_128_MD5

从 Windows 10 版本 1507 年和 Windows Server 2016 开始，默认支持 SHA 512 证书。

### <a name="rsa-key-changes"></a>RSA 关键更改

Windows 10 版本 1507 年和 Windows Server 2016 添加注册表配置客户端 RSA 密钥大小选项。

有关详细信息，请参阅[KeyExchangeAlgorithm 的客户端 RSA 密钥大小](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes)。

### <a name="diffie-hellman-key-changes"></a>Diffie-hellman 关键更改

Windows 10 版本 1507 年和 Windows Server 2016 添加注册表配置 Diffie-hellman 关键大小选项。

有关详细信息，请参阅[KeyExchangeAlgorithm-Diffie-hellman 关键大小](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes)。

### <a name="schusestrongcrypto-option-changes"></a>SCH_USE_STRONG_CRYPTO 选项更改

使用 Windows 10 版本 1507 年和 Windows Server 2016，[SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx)选项现在禁用空，MD5，DES，并导出密码。

## <a name="elliptical-curve-changes"></a>更改椭圆形曲线

Windows 10 版本 1507 年和 Windows Server 2016 组策略为添加配置计算机配置下椭圆形曲线 > 管理模板 > 网络 > SSL 配置设置。 ECC 曲线订单列表指定椭圆形曲线首选顺序以及使受支持的曲线未启用它。 
 
添加了对下列椭圆形曲线的支持：

- BrainpoolP256r1 (RFC 7027) 在 Windows 10 版本 1507 年和 Windows Server 2016
- BrainpoolP384r1 (RFC 7027) 在 Windows 10 版本 1507 年和 Windows Server 2016 
- BrainpoolP512r1 (RFC 7027) 在 Windows 10 版本 1507 年和 Windows Server 2016
- Curve25519 (RFC 草稿-ietf-tls-curve25519) 在 Windows 10 版本 1607 年和 Windows Server 2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>调度级别 SealMessage 和 UnsealMessage 支持

Windows 10 版本 1507 年和 Windows Server 2016 添加对支持 SealMessage/UnsealMessage 以调度级别。

## <a name="dtls-12"></a>DTLS 1.2

Windows 10 版本 1607 年和 Windows Server 2016 添加 DTLS 1.2 (RFC 6347) 的支持。

## <a name="httpsys-thread-pool"></a>HTTP.SYS 线程池 

Windows 10 版本 1607 年和 Windows Server 2016 添加注册表配置的线程池用于处理 TLS 握手 HTTP.SYS.

注册表路径： 

HKLM\SYSTEM\CurrentControlSet\Control\LSA

若要指定 / CPU 核心，最大的线程池的大小，创建**MaxAsyncWorkerThreadsPerCpu**条目。 此项中不存在注册表默认情况。 你已创建条目后，将 DWORD 值更改所需的大小。 如果没有配置，最大将为 CPU 核心每 2 个线程。

## <a name="next-protocol-negotiation-npn-support"></a>下一步协议协商（npn 型）的支持

从 Windows 10 版本 1703 年开始，npn 下一步协议协商（型）已删除，并且不再受支持。

## <a name="pre-shared-key-psk"></a>预共享的键 (PSK)

Windows 10 版本 1607 年和 Windows Server 2016 添加 PSK 关键 exchange 算法 (RFC 4279) 的支持。

添加了对以下 PSK 密码套件的支持：

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) 在 Windows 10 版本 1607 年和 Windows Server 2016
- 在 Windows 10 版本 1607 年和 Windows Server 2016 TLS_PSK_WITH_AES_256_CBC_SHA384(RFC 5487)
- TLS_PSK_WITH_NULL_SHA256 (RFC 5487) 在 Windows 10 版本 1607 年和 Windows Server 2016
- TLS_PSK_WITH_NULL_SHA384 (RFC 5487) 在 Windows 10 版本 1607 年和 Windows Server 2016
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) 在 Windows 10 版本 1607 年和 Windows Server 2016
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) 在 Windows 10 版本 1607 年和 Windows Server 2016

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>不服务器端状态服务器端性能改进的会话恢复

Windows 10 版本 1507 年和 Windows Server 2016 提供了 30%会话 resumptions 每秒会话门票与 Windows Server 2012。

## <a name="session-hash-and-extended-master-secret-extension"></a>会话希和扩展母版幽静扩展

Windows 10 版本 1507 年和 Windows Server 2016 添加对 RFC 7627 支持：传输层安全性 (TLS) 会话希代码以及扩展母版幽静扩展。

由于此更改后，Windows 10 和 Windows Server 2016 要求第三方[CNG SSL 提供商](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx)更新，以便支持 NCRYPT_SSL_INTERFACE_VERSION_3，并描述此新界面。


## <a name="ssl-support"></a>SSL 支持

从 Windows 10 版本 1607 年和 Windows Server 2016 开始，TLS 客户端和服务器 SSL 3.0 默认处于禁用状态。 这意味着只有应用程序或服务专门请求 SSL 3.0 通过 SSPI，客户端将永远不会提供或接受 SSL 3.0 并且服务器永远不会选择 SSL 3.0。

从 Windows 10 版本 1607 年和 Windows Server 2016 开始，SSL 2.0 已删除，并且不再受支持。

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>更改 Windows TLS 遵守 TLS 1.2 要求与非兼容 TLS 客户端进行连接

在 TLS 1.2 客户端使用["signature_algorithms"扩展](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1)指示服务器签名/哈希算法配对可能使用的（即，服务器证书和 server 的关键 exchange）的数字签名。 TLS 1.2 RFC 还需要服务器证书邮件接受"signature_algorithms"扩展：

"如果客户端提供"signature_algorithms"扩展，然后由服务器提供的所有证书必须由都签名显示在该扩展希/签名算法配对。"

在练习中，某些第三方 TLS 客户端不要遵守 TLS 1.2 RFC 和失效哈希算法对它们是在"signature_algorithms"扩展，接受，包括所有签名或完全忽略扩展（后者指示服务器客户端仅支持使用 RSA、DSA 或 ECDSA SHA1）。

TLS 服务器通常仅有一个证书配置每端点，这意味着服务器始终不能提供的证书，以满足客户需求。

在 Windows 10 和 Windows Server 2016，Windows TLS 堆栈之前严格遵守 TLS 1.2 连接失败 RFC 非符合 TLS 客户端和互操作性问题所导致的 RFC 要求。 在 Windows 10 和 Windows Server 2016 中，限制将保持放松状态，并服务器可以发送某个证书存在不符合 TLS 1.2 RFC，如果该服务器的唯一的选项。 然后，的客户端可能继续或终止握手。

验证时服务器和客户端证书，Windows TLS 堆栈严格符合 TLS 1.2 RFC 和只允许协商的签名和哈希算法服务器和客户端的证书。


