---
title: "TLS-SSL (Schannel SSP) 概述"
description: "Windows Server 安全"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b7b0432-1bef-4912-8c9a-8989d47a4da9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: afd0b70264dba1e720f95e40d3d201c2c5bf1c64
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="tls---ssl-schannel-ssp-overview"></a>TLS-SSL (Schannel SSP) 概述

>适用于：Windows Server（半年通道），Windows Server 2016

此主题以供 IT 专业人员引入了使用 Schannel 安全服务提供商 (SSP) 通过描述实际应用，在 Microsoft 实现和软件要求以及其他资源的更改为 Windows Server 2012 和 Windows 8 的 Windows 中的 TLS 月 SSL 实现。

**你意味着：**

-   [Schannel 安全包](https://msdn.microsoft.com/library/ms678421.aspx)

-   [安全通道](https://msdn.microsoft.com/library/windows/desktop/aa380123.aspx)

-   [传输层安全性协议](https://msdn.microsoft.com/library/windows/desktop/aa380516.aspx)

## <a name="BKMK_OVER"></a>TLS\SSL \(Schannel\) 描述
Schannel 是实现安全套接字层 \(SSL\) 和传输层安全性 \(TLS\) 安全支持的提供商 \(SSP\) Internet 标准身份验证的协议。

安全支持的提供商接口 \(SSPI\) 是 Windows 系统用于执行 security\ 相关功能，包括身份验证的 API。 作为几个安全支持的提供商 \(SSPs\)，包括 Schannel SSP 常见接口 SSPI 功能

1.0，1.1、1.2 安全套接字层 \(SSL\) 协议，版本 2.0 和 3.0，数据报传输层安全性 \(DTLS\) 版本 1.0 而言，并专用通信传输 \(PCT\) 协议传输层安全性 \(TLS\) 协议版本基于公用加密。 安全通道 \(Schannel\) 身份验证协议套件提供以下协议。 所有 Schannel 协议都使用服务器 client\/型号。

## <a name="BKMK_APP"></a>实际应用
管理网络时一个问题保证正在不受信任的网络跨应用程序之间发送的数据安全。 你可以使用 TLS\SSL 进行身份验证的服务器和客户端计算机，然后使用协议加密经过身份验证的方之间的消息。

例如，你可以使用有关 TLS\SSL:

-   与 e\ 商务网站 SSL\ 安全的交易

-   身份验证的客户端到 SSL\ 安全网站的访问权限

-   远程访问

-   SQL 访问

-   E\ 邮件

## <a name="BKMK_SOFT"></a>软件要求
TLS\SSL 协议使用客户端 \ 服务器模型，并根据需要公钥基础结构的证书身份验证。

## <a name="BKMK_INSTALL"></a>服务器管理器信息
不有实现 TLS、SSL 或 Schannel 所需的任何配置步骤。

