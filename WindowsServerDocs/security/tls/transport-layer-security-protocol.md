---
title: 传输层安全协议
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: aca2db3ae5bf424dd0f855d24c1ef771039c8b14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403376"
---
# <a name="transport-layer-security-protocol"></a>传输层安全协议

>适用于：Windows Server （半年频道），Windows Server 2016，Windows 10

适用于 IT 专业人员的本主题介绍了传输层安全性（TLS）协议的工作原理，并提供了指向适用于 TLS 1.0、TLS 1.1 和 TLS 1.2 的 IETF Rfc 的链接。

TLS （和 SSL）协议位于应用程序协议层和 TCP/IP 层之间，在此层中，可以保护应用程序数据并将其发送到传输层。 由于协议在应用程序层和传输层之间工作，TLS 和 SSL 可以支持多个应用程序层协议。

TLS 和 SSL 假设正在使用面向连接的传输（通常是 TCP）。 协议允许客户端和服务器应用程序检测以下安全风险：

-   消息篡改

-   消息拦截

-   消息伪造

TLS 和 SSL 协议可以分为两层。 第一层包含应用程序协议和三个握手协议：握手协议、更改密码规范协议和警报协议。 第二层是记录协议。 下图说明了各种层及其元素。

**TLS 和 SSL 协议层**


Schannel SSP 无需修改即可实现 TLS 和 SSL 协议。 SSL 协议是专有的，但 Internet 工程任务组会生成公共 TLS 规范。 有关 Windows 版本支持的 TLS 或 SSL 版本的信息，请参阅[tls/ssl （SCHANNEL SSP）中的协议](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx)。 下表列出了每个 TLS 版本的规格。 每个规范都包含以下信息：

-   TLS 记录协议

-   TLS 握手协议：\- 更改密码规范协议 \- 警报协议

-   加密计算

-   强制密码套件

-   应用程序数据协议

[RFC 5246-传输层安全性（TLS）协议版本1。2](http://tools.ietf.org/html/rfc5246)

[RFC 4346-传输层安全性（TLS）协议版本1。1](http://tools.ietf.org/html/rfc4346)

[RFC 2246-TLS 协议版本1。0](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>TLS 会话恢复
Schannel SSP 在 Windows Server 2012 R2 中引入，它实现了 TLS 会话恢复的服务器端部分。 Windows 8 中添加了 RFC 5077 的客户端实现。

将 TLS 连接到服务器的设备经常需要重新连接。 TLS 会话恢复降低了建立 TLS 连接的成本，因为恢复涉及到一种简化的 TLS 握手。 这会允许一组 TLS 服务器恢复彼此的 TLS 会话，从而促进更多恢复尝试。 此修改为支持 RFC 5077 的任何 TLS 客户端（包括 Windows Phone 和 Windows RT 设备）提供了以下节约：

-   在服务器上减少了资源使用率

-   减少了带宽，这可提高客户端连接的效率

-   由于连接摘要，减少了 TLS 握手所花费的时间

有关无状态 TLS 会话恢复的信息，请参阅 IETF 文档 [RFC 5077](http://www.ietf.org/rfc/rfc5077)。

## <a name="BKMK_AppProtocolNego"></a>应用程序协议协商
 Windows Server 2012 R2 和 Windows 8.1 引入了支持客户端 TLS 应用程序协议协商的支持。 应用程序可以在 HTTP 2.0 标准开发中利用协议，并且用户可以使用运行 SPDY 协议的应用来访问联机服务如 Google 和 Twitter。

有关应用程序协议协商的工作原理的信息，请参阅[传输层安全性 (TLS) 应用程序层协议协商扩展](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05)。

## <a name="BKMK_SNI"></a>TLS 支持服务器名称指示扩展
服务器名称指示符 (SNI) 功能扩展了 SSL 和 TLS 协议，以便在单台服务器上运行大量虚拟映像时正确标识服务器。 在虚拟托管方案中，多个域（每个域都有其自己的可能不同证书）托管在一个服务器上。 在这种情况下，服务器无法事先知道要将哪个证书发送到客户端。 SNI 允许客户端在协议前面通知目标域，这允许服务器正确选择正确的证书。

此附加功能：

-   允许你在单个 Internet 协议和端口组合上托管多个 SSL 网站

-   在单台 Web 服务器上托管多个 SSL 网站时，减少内存使用

-   允许多个用户同时连接到 SSL 网站



