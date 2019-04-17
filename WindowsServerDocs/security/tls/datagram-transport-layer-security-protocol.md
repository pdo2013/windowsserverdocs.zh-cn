---
title: "数据报传输层安全性协议"
description: "Windows Server 安全"
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
ms.date: 10/12/2016
ms.openlocfilehash: 31c1cf1f3218c0511a4407b560be30d0c6f86233
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# 数据报传输层安全性协议

>适用于：Windows Server（半年通道），Windows Server 2016

IT 专业人员的此参考主题介绍了数据报传输层安全性 (DTLS) 协议，它是一部分 Schannel 安全支持提供商 (SSP)。

## <a name="BKMK_DTLS"></a>
DTLS 协议 Schannel SSP 在 Windows Server 2012 和 Windows 8 中引入了，提供通信的数据报协议的隐私。 有关哪些 DTLS 支持版本的 Windows 版本中的信息，请参阅[TLS 月 SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/en-us/library/windows/desktop/mt808159(v=vs.85).aspx)。 协议允许客户端和服务器应用程序进行通信，专门用于阻止窃取、被篡改、或消息伪造的方式。 DTLS 协议基于传输层安全性 (TLS) 协议，并提供等效的安全保障，减少了需要使用 IPsec 或设计的自定义应用程序的一层安全性协议。

报共有流媒体，例如游戏或安全进行视频会议中。 开发人员可以开发应用程序使用 Windows 身份验证的安全支持提供程序接口 (SSPI) 模型上下文中的 DTLS 协议安全之间客户端和服务器的通信。 在顶部用户数据报协议 (UDP) 中内置 DTLS 协议。 DTLS 设计为尽可能相似 TLS 尽可能，以最小化新的安全发明和最大的代码和基础结构重新使用。

可供配置的密码套件被模仿这些可以为 TLS 配置。 不允许 RC4。 Schannel 继续使用加密技术下一代 (CNG)。 这利用 FIPS 140 认证、Windows Vista 中引入了。

## 请参阅

[IETF RFC 4347 数据报传输层安全性](http://tools.ietf.org/html/rfc4347)


