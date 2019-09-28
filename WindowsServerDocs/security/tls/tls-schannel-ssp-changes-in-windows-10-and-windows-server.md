---
title: TLS （Schannel SSP）
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 05/16/2018
ms.openlocfilehash: e103e985592e6aed150ccd3e1a87e56f19621dbe
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403387"
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>TLS （Schannel SSP）在 Windows 10 和 Windows Server 2016 中的更改

>适用于：Windows Server （半年频道）、Windows Server 2016 和 Windows 10

## <a name="cipher-suite-changes"></a>密码套件更改

Windows 10 版本1511和 Windows Server 2016 添加了对使用移动设备管理（MDM）配置密码套件顺序的支持。

有关密码套件优先级顺序的更改，请参阅[Schannel 中的密码套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)。

添加了对以下密码套件的支持：

- Windows 10 版本1507和 Windows Server 2016 中的 TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 （RFC 5289）
- Windows 10 版本1507和 Windows Server 2016 中的 TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 （RFC 5289）

以下密码套件的 DisabledByDefault 更改：

- Windows 10 版本1703中的 TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 （RFC 5246）
- Windows 10 版本1703中的 TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 （RFC 5246）
- Windows 10 版本1703中的 TLS_DHE_DSS_WITH_AES_256_CBC_SHA （RFC 5246）
- Windows 10 版本1703中的 TLS_DHE_DSS_WITH_AES_128_CBC_SHA （RFC 5246）
- Windows 10 版本1703中的 TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA （RFC 5246）
- Windows 10 版本1709中的 TLS_RSA_WITH_RC4_128_SHA
- Windows 10 版本1709中的 TLS_RSA_WITH_RC4_128_MD5

从 Windows 10 版本1507和 Windows Server 2016 开始，默认情况下支持 SHA 512 证书。

### <a name="rsa-key-changes"></a>RSA 密钥更改

Windows 10 版本1507和 Windows Server 2016 添加了客户端 RSA 密钥大小的注册表配置选项。

有关详细信息，请参阅[KeyExchangeAlgorithm-客户端 RSA 密钥大小](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes)。

### <a name="diffie-hellman-key-changes"></a>Diffie-hellman 关键更改

Windows 10 版本1507和 Windows Server 2016 添加了 Diffie-hellman 密钥大小的注册表配置选项。

有关详细信息，请参阅[KeyExchangeAlgorithm-diffie-hellman 密钥大小](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes)。

### <a name="sch_use_strong_crypto-option-changes"></a>SCH_USE_STRONG_CRYPTO 选项更改

对于 Windows 10，版本1507和 Windows Server 2016， [SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx)选项现在禁用 NULL、MD5、DES 和导出密码。

## <a name="elliptical-curve-changes"></a>椭圆曲线更改

Windows 10 版本1507和 Windows Server 2016 在 "计算机配置" > 管理模板 > 网络 > SSL "配置设置" 下为椭圆曲线添加组策略配置。 ECC 曲线顺序列表指定椭圆曲线的首选顺序，并启用未启用的支持曲线。 
 
添加了对以下椭圆曲线的支持：

- Windows 10 版本1507和 Windows Server 2016 中的 BrainpoolP256r1 （RFC 7027）
- Windows 10 版本1507和 Windows Server 2016 中的 BrainpoolP384r1 （RFC 7027） 
- Windows 10 版本1507和 Windows Server 2016 中的 BrainpoolP512r1 （RFC 7027）
- Windows 10 版本1607和 Windows Server 2016 中的 Curve25519 （RFC 草稿-ietf-Curve25519）

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>SealMessage & UnsealMessage 的派单级别支持

Windows 10 版本1507和 Windows Server 2016 在调度级别添加了对 SealMessage/UnsealMessage 的支持。

## <a name="dtls-12"></a>DTLS 1。2

Windows 10 版本1607和 Windows Server 2016 添加了对 DTLS 1.2 （RFC 6347）的支持。

## <a name="httpsys-thread-pool"></a>HTTP.SYS 线程池 

Windows 10 版本1607和 Windows Server 2016 添加用于处理 HTTP TLS 握手的线程池大小的注册表配置。系统.

注册表路径： 

HKLM\SYSTEM\CurrentControlSet\Control\LSA

若要指定每个 CPU 核心的最大线程池大小，请创建一个**MaxAsyncWorkerThreadsPerCpu**条目。 默认情况下，注册表中不存在此项。 创建项后，将 DWORD 值更改为所需的大小。 如果未配置，则每个 CPU 核心的最大线程数为2个。

## <a name="next-protocol-negotiation-npn-support"></a>下一个协议协商（NPN）支持

从 Windows 10 版本1703开始，下一个协议协商（NPN）已删除，不再受支持。

## <a name="pre-shared-key-psk"></a>预共享密钥（PSK）

Windows 10 版本1607和 Windows Server 2016 添加了对 PSK 密钥交换算法的支持（RFC 4279）。

添加了对以下 PSK 密码套件的支持：

- Windows 10 版本1607和 Windows Server 2016 中的 TLS_PSK_WITH_AES_128_CBC_SHA256 （RFC 5487）
- Windows 10 版本1607和 Windows Server 2016 中的 TLS_PSK_WITH_AES_256_CBC_SHA384 （RFC 5487）
- Windows 10 版本1607和 Windows Server 2016 中的 TLS_PSK_WITH_NULL_SHA256 （RFC 5487）
- Windows 10 版本1607和 Windows Server 2016 中的 TLS_PSK_WITH_NULL_SHA384 （RFC 5487）
- Windows 10 版本1607和 Windows Server 2016 中的 TLS_PSK_WITH_AES_128_GCM_SHA256 （RFC 5487）
- Windows 10 版本1607和 Windows Server 2016 中的 TLS_PSK_WITH_AES_256_GCM_SHA384 （RFC 5487）

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>无服务器端状态服务器端性能改进的会话恢复

与 Windows Server 2012 相比，windows 10 版本1507和 Windows Server 2016 提供每秒更多 30% 的会话摘要。

## <a name="session-hash-and-extended-master-secret-extension"></a>会话哈希和扩展的主密钥扩展

Windows 10 版本1507和 Windows Server 2016 添加了 RFC 7627 支持：传输层安全性（TLS）会话哈希和扩展的主密钥扩展。

由于此更改，Windows 10 和 Windows Server 2016 需要第三方[CNG SSL 提供程序](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx)更新以支持 NCRYPT_SSL_INTERFACE_VERSION_3，并描述此新接口。


## <a name="ssl-support"></a>SSL 支持

从 Windows 10 版本1607和 Windows Server 2016 开始，默认情况下，TLS 客户端和服务器 SSL 3.0 处于禁用状态。 这意味着，除非应用程序或服务专门通过 SSPI 请求 SSL 3.0，否则客户端将不会提供或接受 SSL 3.0，并且服务器绝不会选择 SSL 3.0。

从 Windows 10 版本1607和 Windows Server 2016 开始，SSL 2.0 已被移除，不再受支持。

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>对 Windows TLS 的更改遵循与不符合 TLS 客户端的连接的 TLS 1.2 要求

在 TLS 1.2 中，客户端使用["signature_algorithms" 扩展](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1)向服务器指示可以在数字签名中使用的签名/哈希算法对（即服务器证书和服务器密钥交换）。 TLS 1.2 RFC 还要求服务器证书消息 "signature_algorithms" 扩展：

"如果客户端提供了" signature_algorithms "扩展，则服务器提供的所有证书必须由该扩展中显示的哈希/签名算法对签名。"

在实际操作中，某些第三方 TLS 客户端不符合 TLS 1.2 RFC，并且不能包含它们愿意接受到 "signature_algorithms" 扩展中的所有签名和哈希算法对，也不能完全省略扩展（后者指示该客户端仅支持具有 RSA、DSA 或 ECDSA 的 SHA1 的服务器。

TLS 服务器的每个终结点通常只配置一个证书，这意味着服务器无法始终提供满足客户端要求的证书。

在 Windows 10 和 Windows Server 2016 之前，Windows TLS 堆栈严格遵循 TLS 1.2 RFC 要求，导致与 RFC 不相容 TLS 客户端和互操作性问题的连接失败。 在 Windows 10 和 Windows Server 2016 中，约束是宽松的，服务器可以发送不符合 TLS 1.2 RFC 的证书（如果这是服务器的唯一选项）。 然后，客户端可以继续或终止握手。

验证服务器和客户端证书时，Windows TLS 堆栈严格符合 TLS 1.2 RFC，并且仅允许在服务器和客户端证书中协商的签名和哈希算法。


