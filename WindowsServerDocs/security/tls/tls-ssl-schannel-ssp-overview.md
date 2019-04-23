---
title: TLS/SSL (Schannel SSP) 概述
description: Windows Server 安全
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
ms.date: 05/16/2018
ms.openlocfilehash: a6571e5e06e07fd62ad4cf39bab322b45c90a9f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848598"
---
# <a name="tlsssl-overview-schannel-ssp"></a>TLS/SSL (Schannel SSP) 概述

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows 10

本主题面向 IT 专业人员介绍了通过描述实际应用程序，使用 Schannel 安全服务提供程序 (SSP) 的 Windows 中的 TLS 和 SSL 实现更改 Microsoft 的实现中和软件要求，以及对于 Windows Server 2012 和 Windows 8 的其他资源。

## <a name="BKMK_OVER"></a>说明
Schannel 是安全支持提供程序 (SSP)，可实现安全套接字层 (SSL) 和传输层安全 (TLS) Internet 标准身份验证协议。

安全支持提供程序接口 (SSPI) 是 Windows 系统用于执行安全相关功能（包括身份验证）的 API。 SSPI 用作几个 Ssp，包括 Schannel SSP 的公共接口

TLS 版本 1.0、 1.1 和 1.2、 SSL 版本 2.0 和 3.0、 以及数据报传输层安全性\(DTLS\)协议版本 1.0 和专用通信传输\(PCT\)上基于协议公钥加密。 Schannel 身份验证协议套件提供这些协议。 所有 Schannel 协议均使用客户端/服务器模型。

## <a name="BKMK_APP"></a>应用程序
管理网络时的一个问题是保护跨未受信任的网络在应用程序之间发送的数据的安全。 可以使用 TLS 和 SSL 进行身份验证服务器和客户端计算机，然后使用该协议来加密经过身份验证的参与方的消息。

例如，你可以将 TLS/SSL 用于：

-   电子商务网站受 SSL 保护的交易
-   对受 SSL 保护的网站的经身份验证的客户端访问
-   远程访问
-   SQL 访问
-   电子邮件

## <a name="BKMK_SOFT"></a>要求
TLS 和 SSL 协议使用客户端/服务器模型和基于证书的身份验证，需要公钥基础结构。

## <a name="BKMK_INSTALL"></a>服务器管理器信息
没有实现 TLS、 SSL 或 Schannel 所需的配置步骤。

## <a name="see-also"></a>请参阅 ##

-   [Schannel 安全数据包](https://docs.microsoft.com/windows/desktop/com/schannel)
-   [安全通道](https://docs.microsoft.com/windows/desktop/SecAuthN/secure-channel)
-   [传输层安全协议](https://docs.microsoft.com/windows/desktop/SecAuthN/transport-layer-security-protocol)
