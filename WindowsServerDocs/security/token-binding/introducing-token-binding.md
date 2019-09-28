---
title: 标记绑定简介
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 4623a48c-cefd-4a27-9173-2af58ac212f2
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 52ba35808b34eb07ecd6ac92819e9dc7a693b15b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403323"
---
# <a name="introducing-token-binding"></a>标记绑定简介

>适用于：Windows Server 2016 和 Windows 10

令牌绑定协议允许应用程序和服务以加密方式将其安全令牌绑定到 TLS 层，以减轻令牌被盗和重播攻击。 长时间生存期、唯一可识别的 TLS [RFC5246] 绑定可以跨多个 TLS 会话和连接。

版本支持：

- Windows 10 版本1507–默认情况下关闭
    - 添加了令牌绑定协议[[草稿-tokbind]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - WinInet & HTTP。SYS 支持通过 HTTP 进行的令牌绑定[[草稿-ietf-tokbind]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/01/)
- 默认情况下，windows 10、版本1511和1607以及 Windows Server 2016 –打开
    - 令牌绑定协议已更新[[草稿-tokbind]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/01/)
    - 添加了用于令牌绑定协商的 TLS 扩展[[popov-tokbind]](https://tools.ietf.org/html/draft-popov-tokbind-negotiation-00)
    - WinInet & HTTP。SYS 支持通过 HTTP 更新的令牌绑定[[草稿-ietf-tokbind]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/02/)
- Windows 10 版本1507，其中包含服务更新[KB4034668](https://support.microsoft.com/kb/KB4034668)，windows 10，版本1511，含服务更新[KB4034660](https://support.microsoft.com/kb/KB4034660)，windows 10，版本1607，windows Server 2016 with 服务更新[KB4034658](https://support.microsoft.com/kb/KB4034658)支持令牌绑定协议版本0.10 –默认启用
    - 令牌绑定协议已更新[[草稿-tokbind]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - 添加了用于令牌绑定协商的 TLS 扩展[[tokbind-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP。SYS 支持通过 HTTP 更新的令牌绑定[[草稿-ietf-tokbind]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
- 默认情况下，Windows 10 版本1703支持令牌绑定协议版本0.10 –启用
    - 令牌绑定协议已更新[[草稿-tokbind]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-protocol/10/)
    - 添加了用于令牌绑定协商的 TLS 扩展[[tokbind-05]](https://tools.ietf.org/html/draft-ietf-tokbind-negotiation-05)
    - WinInet & HTTP。SYS 支持通过 HTTP 更新的令牌绑定[[草稿-ietf-tokbind]](https://datatracker.ietf.org/doc/draft-ietf-tokbind-https/06/)
    - 启用了基于虚拟化的安全性的 Windows 设备会将令牌绑定密钥保存在与正在运行的操作系统隔离的受保护环境中

有关 ASP .NET 支持的信息，请参阅[.NET Framework 参考源](https://referencesource.microsoft.com/#System.Web/ITlsTokenBindingInfo.cs,4a5e5668f5c31170)。 

有关 .NET Framework 的信息，请参阅以下主题：

- [网络增强功能](https://blogs.msdn.microsoft.com/dotnet/2015/11/30/net-framework-4-6-1-is-now-available/#networking)

- [.NET TokenBinding 类](https://msdn.microsoft.com/library/system.security.authentication.extendedprotection.tokenbinding.aspx)
