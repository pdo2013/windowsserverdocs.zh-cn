---
title: "传输层安全性协议"
description: "Windows Server 安全"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de510bb0-a9f6-4bbe-8f8a-8dd7473bbae8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 94c81a79e5f3fd8fc22eafde3de8bd3d0bdd73f0
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# 传输层安全性协议

>适用于：Windows Server（半年通道），Windows Server 2016

此主题以供 IT 专业人员介绍了如何传输层安全性 (TLS) 协议的工作原理以及 TLS 1.0、TLS 1.1 和 TLS 1.2 提供 IETF Rfc 的链接。

TLS（和 SSL）协议位于应用协议层和 TCP/IP 层，他们可以在此处安全和应用程序数据发送到传输层之间。 协议工作之间的应用程序层和传输层，因为 TLS 和 SSL 可支持应用程序有多个层协议。

TLS 和 SSL 假定面向连接传输，通常 TCP，正在使用中。 协议允许客户端和服务器检测以下安全风险的应用程序：

-   邮件被篡改

-   消息侦听

-   消息伪造

TLS 和 SSL 协议分为两个层。 应用协议和的三个质询协议的第一层组成：握手协议、更改密码指标协议和通知的协议。 第二层是记录协议。 下图所示的各种层和他们元素。

**TLS 和 SSL 协议层**


Schannel SSP 实现原样 TLS 和 SSL 协议。 SSL 协议专有，但 Internet 工程任务强制产生公共 TLS 规范。 有关哪些 TLS 或 SSL 支持版本的 Windows 版本中的信息，请参阅[TLS 月 SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/en-us/library/windows/desktop/mt808159(v=vs.85).aspx)。 下表列出了每个 TLS 版本规范。 每个规范包含以下信息：

-   TLS 录制协议

-   TLS 质询协议: \-更改密码指标协议 \-提醒协议

-   加密计算

-   强制密码套件

-   应用数据协议

[RFC 5246-传输层安全性 (TLS) 协议版本 1.2](http://tools.ietf.org/html/rfc5246)

[RFC 4346-传输层安全性 (TLS) 协议 1.1 版本](http://tools.ietf.org/html/rfc4346)

[RFC 2246-TLS 协议 1.0 版本](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>TLS 会话恢复
引入 Windows Server 2012 R2、Schannel SSP 实施 TLS 会话恢复的服务器端部分。 在 Windows 8 中添加了 RFC 5077 的客户端实现。

经常 TLS 连接到服务器的设备需要重新连接。 TLS 会话恢复减少 TLS 连接建立因为恢复涉及缩写的 TLS 握手成本。 通过允许的一组 TLS 服务器恢复彼此的 TLS 会议，这有助于更多恢复尝试。 此修改提供任何支持 RFC 5077，包括 Windows Phone 和 Windows RT 的设备的 TLS 客户端以下流量节省类型：

-   在服务器上的降低的资源使用状况

-   减少提高效率的客户端连接的带宽

-   由于的连接 resumptions TLS 握手为花费的时间减少

有关无状态 TLS 会话恢复信息，请参阅 IETF 文档[RFC 5077。](http://www.ietf.org/rfc/rfc5077)

## <a name="BKMK_AppProtocolNego"></a>应用程序协议协商
 Windows Server 2012R2 和 Windows 8.1 引入使客户端 TLS 应用程序协议协商的支持。 作为 HTTP 2.0 标准开发，部分，应用程序可以利用协议和用户可以使用运行 SPDY 协议应用访问联机服务，如 Google 和 Twitter。

有关应用协议协商的工作原理的信息，请参阅[传输层安全性 (TLS) 应用程序层协议协商扩展](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05)。

## <a name="BKMK_SNI"></a>服务器名称指示扩展 TLS 支持
服务器指示 (SNI) 功能扩展 SSL 和 TLS 协议大量虚拟映像一个服务器上运行时，允许正确服务器的标识。 在虚拟宿主方案，几个域（每一个都有它自己可能不同的证书）位于 one 服务器上。 在此情况下，服务器有无法事先知道向客户端发送该证书。 SNI 允许通知目标域前面的协议的客户端和这使得服务器正确选择正确的证书。

此附加功能：

-   允许你拥有多个 SSL 网站上的单个 Internet 协议，端口组合

-   当多个 SSL 网站位于单个 web 服务器减少内存使用情况

-   可使更多的用户，若要同时连接到 SSL 网站



