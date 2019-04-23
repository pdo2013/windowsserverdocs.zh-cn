---
title: 数据报传输层安全协议
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57b8873a-ad9c-4f2c-93e0-a2af352c6965
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: b32ebafff5e41d5c3140f008f6f391852f2f3474
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870788"
---
# <a name="datagram-transport-layer-security-protocol"></a>数据报传输层安全协议

Windows Server （半年频道），Windows Server 2016 中，Windows 10

本参考主题面向 IT 专业人员介绍数据报传输层安全性 (DTLS) 协议，它是 Schannel 安全支持提供程序 (SSP) 的一部分。

## <a name="BKMK_DTLS"></a>
DTLS 协议在 Schannel SSP 在 Windows Server 2012 和 Windows 8 中引入，提供有关数据报协议的通信隐私。 有关哪些 DTLS 版本支持 Windows 版本中的信息，请参阅[TLS/SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/en-us/library/windows/desktop/mt808159(v=vs.85).aspx)。 此协议让客户端和服务器应用程序可采用旨在防止窃听、篡改或消息伪造的方式进行通信。 DTLS 协议基于传输层安全 (TLS) 协议并提供等效的安全保证，从而减少使用 IPsec 或设计自定义应用程序层安全协议的需求。

数据报是常见的流媒体，如游戏或受保护视频会议。 开发人员可以开发应用程序可以使用 Windows 身份验证安全支持提供程序接口 (SSPI) 模型的上下文中的 DTLS 协议，保护客户端和服务器之间的通信。 DTLS 协议是构建的基于用户数据报协议 (UDP)。 DTLS 旨在尽可能新安全发明降至最低并最大限度地重用代码和基础结构量是类似于 TLS。

可用于配置的密码套件可以实现模式化后的可以为 TLS 配置。 不允许使用 RC4。 Schannel 继续使用 Cryptography Next Generation (CNG)。 这可利用的在 Windows Vista 中引入的 FIPS 140 认证。

## <a name="see-also"></a>请参阅

[IETF RFC 4347 数据报传输层安全性](http://tools.ietf.org/html/rfc4347)


