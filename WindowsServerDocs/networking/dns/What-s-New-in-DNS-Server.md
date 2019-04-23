---
title: 什么是 Windows Server 中的 DNS 服务器中的新增功能
description: 本主题概述在 Windows Server 2016 和更高版本中的 DNS 服务器中的新增功能
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 665e411eda834a59c6dbe3581611b9b58bd006f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833568"
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>什么是 Windows Server 中的 DNS 服务器中的新增功能

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍了新增或更改 Windows Server 2016 中的域名系统 (DNS) 服务器功能。  
  
Windows Server 2016 中 DNS 服务器提供以下几个方面的增强的支持。  
  
|功能|新功能或改进功能|描述|  
|-----------------|-------------------|---------------|  
|DNS 策略|新增|可以配置 DNS 策略来指定 DNS 服务器响应 DNS 查询的方式。 DNS 响应可以在客户端 IP 地址 （位置） 上基于时间的天和几个其他参数。 DNS 策略启用位置感知型 DNS、 流量管理、 负载平衡、 拆分式 DNS 和其他方案。|  
|响应速率限制 (RRL)|新增|可以让你的 DNS 服务器上的响应速率限制。 通过执行此操作，可以避免恶意使用你的 DNS 服务器来启动拒绝服务攻击，DNS 客户端上的系统中的可能性。|  
|命名实体 （窗格会） 的基于 DNS 的身份验证|新增|可以使用 TLSA （传输层安全身份验证） 记录向状态哪些它们应该会从用于你的域名的证书的 CA 的 DNS 客户端提供的信息。 这样可以防止拦截的攻击，有人可能会损坏 DNS 缓存，以指向其自己的网站，并提供它们从不同的 CA 颁发的证书。|  
|未知的记录支持|新增|您可以添加记录不显式支持的使用未知的记录功能的 Windows DNS 服务器。|  
|IPv6 根提示|新增|可以使用根提示支持执行使用 IPV6 根服务器的 internet 名称解析的本机 IPV6。|  
|Windows PowerShell 支持|改进功能|为 DNS 服务器提供了新的 Windows PowerShell cmdlet。|  
  
## <a name="dns-policies"></a>DNS 策略

对于地理位置基于流量管理，基于一天时间，以管理单个 DNS 服务器配置为拆分的智能 DNS 响应，可以使用 DNS 策略\-大脑部署，DNS 查询和的详细信息上应用筛选器。 以下各项提供有关这些功能的更多详细信息。

-   **应用程序负载均衡。** 当已部署应用程序在不同位置的多个实例时，可以使用 DNS 策略来均衡动态分配的流量负载的应用程序在不同的应用程序实例之间的流量负载。

-   **异地\-基于位置的流量管理。** 可以使用 DNS 策略以允许主要和辅助 DNS 服务器响应 DNS 客户端查询基于的客户端并向其客户端尝试连接，该资源的地理位置提供客户端最接近的 IP 地址资源。 

-   **拆分式 DNS。** 使用拆分\-澍 DNS，DNS 记录拆分为相同的 DNS 服务器上的不同区域作用域和 DNS 客户端接收基于客户端是内部或外部客户端的响应。 你可以配置拆分式\-澍 DNS Active Directory 集成区域或独立的 DNS 服务器上的区域。

-   **筛选。** 可以配置 DNS 策略来创建查询筛选器的基于你提供的条件。 DNS 策略中的查询筛选器，可以配置 DNS 服务器根据 DNS 查询和发送 DNS 查询的 DNS 客户端的方式自定义响应。 
-   **取证。** 可以使用 DNS 策略重定向到非恶意的 DNS 客户端\-存在的 IP 地址，而不是将它们定向到他们尝试访问的计算机。

-   **一天的时间基于重定向。** 可以使用 DNS 策略以使用基于一天的时间的 DNS 策略将应用程序流量分配到不同的地理位置分散的应用程序实例。 
  
您还可以使用 DNS 策略，为 Active Directory 集成 DNS 区域。

有关详细信息，请参阅[DNS 策略方案指南](deploy/DNS-Policies-Overview.md)。

## <a name="response-rate-limiting"></a>响应速率限制

可以配置 RRL 设置来控制如何响应对 DNS 客户端的请求时服务器接收多个针对同一个客户端的请求。 通过执行此操作，您可以防止有人发送使用你的 DNS 服务器的拒绝服务 (Dos) 攻击。 例如，net 的智能机器人可以将请求发送到使用第三个计算机的 IP 地址作为请求者在 DNS 服务器。 而无需 RRL，你的 DNS 服务器可能会响应所有请求，流量激增的第三台计算机。 当使用 RRL 时，可以配置以下设置：  
  
-   **每秒响应**。 这是在同一响应将在一秒内提供给客户端最大次数。  
  
-   **每秒错误**。 这是最多的错误响应将在一秒内发送到同一个客户端的次数。  
  
-   **窗口**。 这是如果进行了过多的请求将在为其挂起对客户端的响应的秒数。  
  
-   **泄漏速率**。 这是频率的 DNS 服务器将响应的查询响应将被挂起的时间内。 例如，如果服务器将向客户端的响应挂起 10 秒，并且泄漏速率为 5，则服务器将仍响应一个查询用于发送的每个 5 个查询。 这允许合法客户端收到的响应，即使在 DNS 服务器正在将应用的响应速率限制其子网或 FQDN。  
  
-   **TC 速率**。 这用来告诉客户端尝试使用 TCP 进行连接时对客户端的响应将被挂起。 例如，如果 TC 速率为 3，并在服务器挂起对给定的客户端的响应，服务器将发出接收每 3 个查询的 TCP 连接的请求。 请确保 TC 速率的值低于泄漏速率，为客户端提供的选项，可以在泄漏响应之前通过 TCP 连接。  
  
-   **最大响应**。 这是服务器将向客户端颁发，响应将被挂起时的响应的最大数目。  
  
-   **列入允许列表域**。 这是要从 RRL 设置中排除的域列表。  
  
-   **列入允许列表的子网**。 这是要从 RRL 设置中排除的子网的列表。  
  
-   **列入允许列表服务器接口**。 这是要从 RRL 设置中排除的 DNS 服务器接口的列表。  
  
## <a name="dane-support"></a>窗格会支持

可以使用窗格会支持\(RFC 6394 和 6698\)若要向 DNS 客户端指定哪些 CA 他们应期望从域名称要颁发的证书托管在你的 DNS 服务器。 这可以防止某人是能够破坏 DNS 缓存并指向其自己的 IP 地址的 DNS 名称的拦截的攻击的形式。  
  
例如，假设主机通过使用从名为 CA1 的已知颁发机构的证书在 www.contoso.com 中使用 SSL 安全网站。 有人可能仍将能够从不同，因此也-未知的证书颁发机构名为 CA2 获得 www.contoso.com 的证书。 然后，托管 www.contoso.com 假网站的实体可能能够破坏 DNS 缓存，使其 www.contoto.com 指向其伪造的站点的客户端或服务器。 最终用户将从 CA2，提供了证书和可能只需同意它并连接到该伪造站点。 与窗格会，客户端会向 contoso.com 询问 TLSA 记录的 DNS 服务器发出请求，并了解 www.contoso.com 的证书是由 CA1 的问题。 如果从另一个 CA 提供的证书，则中止连接。  
  
## <a name="unknown-record-support"></a>未知的记录支持

"未知"是记录到 DNS 服务器不知道其 RDATA 格式。 对未知的记录 (RFC 3597) 类型的新添加的支持意味着您可以到二进制的在线格式中的 Windows DNS 服务器区域中添加不受支持的记录类型。 Windows 缓存解析程序已有能力处理未知的记录类型。 Windows DNS 服务器不会执行任何记录的特定处理未知的记录，但将其发送回在响应中如果它未收到查询。  
  
## <a name="ipv6-root-hints"></a>IPv6 根提示

IPV6 根提示，由 IANA，发布已加入 windows DNS 服务器。 Internet 名称查询现在可以用于执行名称解析使用 IPv6 根服务器。

## <a name="windows-powershell-support"></a>Windows PowerShell 支持

Windows Server 2016 中引入的以下新的 Windows PowerShell cmdlet 和参数。
  
-   **添加 DnsServerRecursionScope**。 此 cmdlet 在 DNS 服务器上创建新的递归作用域。 递归作用域的 DNS 策略中用于指定一系列转发器 DNS 查询中使用。  
  
-   **删除 DnsServerRecursionScope**。 此 cmdlet 将删除现有的递归作用域。  
  
-   **集 DnsServerRecursionScope**。 此 cmdlet 更改现有递归作用域的设置。  
  
-   **Get DnsServerRecursionScope**。 此 cmdlet 检索有关现有递归作用域的信息。  
  
-   **添加 DnsServerClientSubnet**。 此 cmdlet 将创建一个新的 DNS 客户端的子网。 子网的 DNS 策略中用于标识 DNS 客户端所在的位置。  
  
-   **Remove-DnsServerClientSubnet**. 此 cmdlet 将删除现有的 DNS 客户端子网。  
  
-   **Set-DnsServerClientSubnet**. 此 cmdlet 可更改现有的 DNS 客户端子网的设置。  
  
-   **Get-DnsServerClientSubnet**. 此 cmdlet 检索有关现有 DNS 客户端的子网信息。  
  
-   **添加 DnsServerQueryResolutionPolicy**。 此 cmdlet 创建新的 DNS 查询解析策略。 DNS 查询解析策略用来指定如何，或如果对其进行响应查询，基于不同条件。  
  
-   **删除 DnsServerQueryResolutionPolicy**。 此 cmdlet 会删除现有的 DNS 策略。  
  
-   **集 DnsServerQueryResolutionPolicy**。 此 cmdlet 可更改现有的 DNS 策略的设置。  
  
-   **Get DnsServerQueryResolutionPolicy**。 此 cmdlet 检索有关现有 DNS 策略的信息。  
  
-   **启用 DnsServerPolicy**。 此 cmdlet 可启用现有的 DNS 策略。  
  
-   **禁用 DnsServerPolicy**。 此 cmdlet 将禁用现有的 DNS 策略。  
  
-   **添加 DnsServerZoneTransferPolicy**。 此 cmdlet 创建新的 DNS 服务器区域传输策略。 DNS 区域传输策略指定是否拒绝或忽略基于不同条件的区域复制。  
  
-   **删除 DnsServerZoneTransferPolicy**。 此 cmdlet 会删除现有的 DNS 服务器区域传输策略。  
  
-   **集 DnsServerZoneTransferPolicy**。 此 cmdlet 更改现有 DNS 服务器区域传输策略的设置。  
  
-   **Get DnsServerResponseRateLimiting**。 此 cmdlet 检索 RRL 设置。  
  
-   **集 DnsServerResponseRateLimiting**。 此 cmdlet 可更改 RRL 设置。  
  
-   **添加 DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet 在 DNS 服务器上创建一个 RRL 异常列表。  
  
-   **Get DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet 检索 RRL excception 列表。  
  
-   **删除 DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet 将删除现有 RRL 异常列表。  
  
-   **集 DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet 可更改 RRL 异常列表。  
  
-   **添加 DnsServerResourceRecord**。 此 cmdlet 已更新为支持未知的记录类型。  
  
-   **Get DnsServerResourceRecord**。 此 cmdlet 已更新为支持未知的记录类型。  
  
-   **删除 DnsServerResourceRecord**。 此 cmdlet 已更新为支持未知的记录类型。  
  
-   **集 DnsServerResourceRecord**。 此 cmdlet 进行更新，以支持未知的记录类型

有关详细信息，请参阅以下 Windows Server 2016 的 Windows PowerShell 命令参考主题。

- [DnsServer 模块](https://docs.microsoft.com/powershell/module/dnsserver/?view=win10-ps)
- [DnsClient 模块](https://docs.microsoft.com/powershell/module/dnsclient/?view=win10-ps)

## <a name="see-also"></a>请参阅  
  
-   [什么是 DNS 客户端中的新增功能](What-s-New-in-DNS-Client.md)  
  

  

