---
title: "引入令牌绑定"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 2a0bb8c75fe3ca7f7befe0bd67eb3d363a5ad7a9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="introducing-token-binding"></a>引入令牌绑定

>适用于：Windows Server 2016 和 Windows 10

加密将其安全令牌绑定到 TLS 层减少令牌被盗和重播攻击的应用程序和服务，可使令牌绑定的协议。 长、唯一可识别 TLS [RFC5246] 绑定可以跨多个 TLS 会话和连接。

版本支持：

- Windows 10 版本 1507，默认情况下关闭
    - 标记添加的绑定协议[[草稿-ietf-tokbind-协议-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet 和 HTTP.SYS 令牌绑定的支持 http [[草稿-ietf-tokbind-https-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10 版本 1511 年以及 1607，，Windows Server 2016 – 在默认情况下
    - 标记有约束力的协议更新[[草稿-ietf-tokbind-协议-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - 为令牌绑定适应添加 TLS 扩展[[草稿-popov-tokbind-协商-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet 和 HTTP.SYS 令牌绑定的支持通过更新 HTTP [[草稿-ietf-tokbind-https-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10 版本 1507 servicing 更新具有[KB4034668](https://support.microsoft.com/kb/KB4034668)，Windows 10 版本 1511 年的服务更新[KB4034660](https://support.microsoft.com/kb/KB4034660)，Windows 10 版本 1607 年，Windows Server 2016 服务更新与[KB4034658](https://support.microsoft.com/kb/KB4034658)支持令牌绑定协议版本 0.10 – 在默认情况下
    - 标记有约束力的协议更新[[草稿-ietf-tokbind-协议-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - 为令牌绑定适应添加 TLS 扩展[[草稿-ietf-tokbind-协商-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet 和 HTTP.SYS 令牌绑定的支持通过更新 HTTP [[草稿-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows 10 版本 1703 年支持令牌绑定协议版本 0.10 – 在默认情况下
    - 标记有约束力的协议更新[[草稿-ietf-tokbind-协议-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - 为令牌绑定适应添加 TLS 扩展[[草稿-ietf-tokbind-协商-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet 和 HTTP.SYS 令牌绑定的支持通过更新 HTTP [[草稿-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - 启用虚拟化基于安全的 Windows 设备将继续令牌绑定键独立于正在运行的操作系统的受保护的环境中

可以在找到有关 ASP.NET 支持信息[.NET Framework 参考源](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170)。 

有关.NET Framework 的信息，请参阅以下主题：

- [网络增强功能](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [.NET TokenBinding 类](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
