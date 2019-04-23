---
title: 引入了令牌绑定
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 2a0bb8c75fe3ca7f7befe0bd67eb3d363a5ad7a9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884228"
---
# <a name="introducing-token-binding"></a>引入了令牌绑定

>适用于：Windows Server 2016 和 Windows 10

令牌绑定协议允许应用程序和服务以密码学角度上将其安全令牌绑定到 TLS 层以缓解令牌被盗和重播攻击。 生存期较长，唯一地标识 TLS [RFC5246] 绑定可以跨多个 TLS 会话和连接。

版本支持：

- Windows 10 版本 1507 – 默认情况下关闭
    - 令牌绑定协议添加[[草稿-ietf-tokbind-协议-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet 和 HTTP。令牌绑定通过 HTTP 的 SYS 支持[[草稿-ietf-tokbind-https-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- Windows 10、 版本 1511年、 1607 和 Windows Server 2016-在默认情况下
    - 令牌绑定协议更新[[草稿-ietf-tokbind-协议-01]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - 添加的标记绑定协商的 TLS 扩展[[草稿-popov-tokbind-协商-00]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet 和 HTTP。通过 HTTP 更新令牌绑定的 SYS 支持[[草稿-ietf-tokbind-https-02]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10 版本 1507年与服务更新[KB4034668](https://support.microsoft.com/kb/KB4034668)，Windows 10 版本 1511年的服务更新[KB4034660](https://support.microsoft.com/kb/KB4034660)，Windows 10，版本 1607年和 Windows Server 2016 服务更新[KB4034658](https://support.microsoft.com/kb/KB4034658)支持令牌绑定协议版本 0.10 – 在默认情况下
    - 令牌绑定协议更新[[草稿-ietf-tokbind-协议-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - 添加的标记绑定协商的 TLS 扩展[[草稿-ietf-tokbind-协商-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet 和 HTTP。通过 HTTP 更新令牌绑定的 SYS 支持[[草稿-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- Windows 10 版本 1703年令牌绑定协议版本 0.10 – 在默认情况下支持
    - 令牌绑定协议更新[[草稿-ietf-tokbind-协议-10]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - 添加的标记绑定协商的 TLS 扩展[[草稿-ietf-tokbind-协商-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet 和 HTTP。通过 HTTP 更新令牌绑定的 SYS 支持[[草稿-ietf-tokbind-https-06]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - 已启用基于虚拟化的安全性的 Windows 设备将标记绑定密钥保留在独立于正在运行的操作系统的受保护环境中

ASP.NET 支持的信息可从[.NET Framework 参考源](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170)。 

有关.NET Framework 的信息，请参阅以下主题：

- [联网增强功能](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [.NET TokenBinding 类](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
