---
title: 数据报传输层安全协议
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f603dc0c5616619088537ffcbd06f64baece0e23
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402296"
---
# <a name="datagram-transport-layer-security-protocol"></a>数据报传输层安全协议

Windows Server （半年频道），Windows Server 2016，Windows 10

本参考主题面向 IT 专业人员，介绍了数据报传输层安全性（DTLS）协议，该协议是 Schannel 安全支持提供程序（SSP）的一部分。

## <a name="BKMK_DTLS"></a>
DTLS 协议是在 Windows Server 2012 和 Windows 8 中的 Schannel SSP 中引入的，它为数据报协议提供通信隐私。 有关 Windows 版本中受支持的 DTLS 版本的信息，请参阅[TLS/SSL （SCHANNEL SSP）中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx)。 此协议让客户端和服务器应用程序可采用旨在防止窃听、篡改或消息伪造的方式进行通信。 DTLS 协议基于传输层安全 (TLS) 协议并提供等效的安全保证，从而减少使用 IPsec 或设计自定义应用程序层安全协议的需求。

数据报在流媒体（如游戏或安全视频会议）中很常见。 开发人员可以开发应用程序，以便在 Windows 身份验证安全支持提供程序接口（SSPI）模型的上下文中使用 DTLS 协议来保护客户端和服务器之间的通信。 DTLS 协议是基于用户数据报协议（UDP）构建的。 DTLS 旨在尽可能与 TLS 类似，以最大程度地减少新的安全性发明，并最大程度地减少代码和基础结构的重复使用。

可用于配置的密码套件会在可配置 TLS 的密码套件之后进行配置。 不允许使用 RC4。 Schannel 继续使用下一代加密技术（CNG）。 这将利用 Windows Vista 中引入的 FIPS 140 证书。

## <a name="see-also"></a>请参阅

[IETF RFC 4347 数据报传输层安全性](http://tools.ietf.org/html/rfc4347)


