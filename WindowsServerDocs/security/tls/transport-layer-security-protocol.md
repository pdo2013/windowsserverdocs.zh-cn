---
title: 传输层安全协议
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de510bb0-a9f6-4bbe-8f8a-8dd7473bbae8
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 05/16/2018
ms.openlocfilehash: 0a3b241fe0d2a61361d551b7f515507ad55d71cd
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284231"
---
# <a name="transport-layer-security-protocol"></a>传输层安全协议

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows 10

面向 IT 专业人员的本主题介绍了如何传输层安全性 (TLS) 协议的工作并提供指向 IETF Rfc 的 TLS 1.0、 1.1 和 TLS 1.2。

TLS （和 SSL） 协议是位于应用程序协议层和 TCP/IP 层，它们可以保护，并将应用程序数据发送到传输层之间。 由于应用程序层和传输层之间工作协议，TLS 和 SSL 可以支持多个应用程序层协议。

TLS 和 SSL 假定一个面向连接的传输，通常是 TCP，正在使用中。 此协议让客户端和服务器应用程序以检测以下安全风险：

-   消息被篡改

-   消息侦听

-   消息伪造

TLS 和 SSL 协议可以分为两个层。 应用程序协议和三个握手协议的第一层组成： 握手协议、 更改密码 spec 协议和警报的协议。 第二层是记录协议。 下图阐释各层，且其元素。

**TLS 和 SSL 协议层**


Schannel SSP 实现 TLS 和 SSL 协议而无需修改。 SSL 协议是专有的但 Internet 工程任务组会产生公共 TLS 规范。 有关哪些 TLS 或 SSL 版本支持 Windows 版本中的信息，请参阅[TLS/SSL (Schannel SSP) 中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx)。 下表列出了为每个 TLS 版本规范。 每个规范包含以下信息：

-   TLS 记录协议

-   TLS 握手协议：\- 更改密码 spec 协议\-警报协议

-   加密计算

-   必需的密码套件

-   应用程序数据协议

[RFC 5246-传输层安全性 (TLS) 协议版本 1.2](http://tools.ietf.org/html/rfc5246)

[RFC 4346-传输层安全性 (TLS) 协议版本 1.1](http://tools.ietf.org/html/rfc4346)

[RFC 2246-TLS 协议版本 1.0](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>TLS 会话恢复
在 Windows Server 2012 R2 中引入，Schannel SSP 实现 TLS 会话恢复的服务器端部分。 Windows 8 中新增了 RFC 5077 的客户端实现。

将 TLS 连接到服务器的设备经常需要重新连接。 TLS 会话恢复可减少建立 TLS 连接，因为恢复涉及缩写的 TLS 握手的成本。 这样一组 TLS 服务器恢复彼此的 TLS 会话，从而便于更多恢复尝试。 此修改为支持 RFC 5077，包括 Windows Phone 和 Windows RT 设备的任何 TLS 客户端提供了以下节省：

-   在服务器上减少了资源使用率

-   减少了带宽，这可提高客户端连接的效率

-   减少了由于连接恢复而在 TLS 握手所花费的时间

有关无状态 TLS 会话恢复的信息，请参阅 IETF 文档 [RFC 5077](http://www.ietf.org/rfc/rfc5077)。

## <a name="BKMK_AppProtocolNego"></a>应用程序协议协商
 Windows Server 2012 R2 和 Windows 8.1 引入了支持，允许客户端 TLS 应用程序协议协商。 应用程序可以利用协议的 HTTP 2.0 标准开发，并且用户可以使用运行 SPDY 协议的应用访问联机服务，如 Google 和 Twitter。

有关应用程序协议协商的工作原理的信息，请参阅[传输层安全性 (TLS) 应用程序层协议协商扩展](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05)。

## <a name="BKMK_SNI"></a>对服务器名称指示扩展的 TLS 支持
服务器名称指示符 (SNI) 功能扩展了 SSL 和 TLS 协议，以便在单台服务器上运行大量虚拟映像时正确标识服务器。 在虚拟主机托管方案中，一台服务器上托管多个域 （各有其自己的可能不同证书）。 在这种情况下，服务器有没有办法事先知道其证书发送到客户端。 SNI 让客户端通知目标域之前在协议中，且这让服务器可以正确选择正确的证书。

此附加功能：

-   可用于承载在单个 Internet 协议和端口组合的多个 SSL 网站

-   在单台 Web 服务器上托管多个 SSL 网站时，减少内存使用

-   允许多个用户同时连接到 SSL 网站



