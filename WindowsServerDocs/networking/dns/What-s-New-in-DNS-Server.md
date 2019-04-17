---
title: 什么是 Windows Server 中的 DNS 服务器中的新增功能
description: 本主题提供新功能，在 Windows Server 2016 及更高版本中的 DNS 服务器的概述
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3ddb8920c045f231dcf5286283d9895ef6ffff47
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>什么是 Windows Server 中的 DNS 服务器中的新增功能

>适用于：Windows Server（半年通道），Windows Server 2016

本主题介绍了域名系统 (DNS) 服务器新增或已更改在 Windows Server 2016 的功能。  
  
在 Windows Server 2016、 DNS 服务器提供增强以下方面的支持。  
  
|功能|新的或改进|描述|  
|-----------------|-------------------|---------------|  
|DNS 策略|新增功能|您可以配置 DNS 策略，若要指定 DNS 服务器响应 DNS 查询的方式。 客户端 IP 地址（的位置），可以基于 DNS 响应时间天，以及一些其他参数。 DNS 策略启用位置感知 DNS、交通管理、负载平衡、裂 DNS 和其他情况。|  
|响应速率限制 (RRL)|新增功能|你可以启用响应 DNS 服务器上的速率限制。 通过执行此操作，你避免恶意通过以下 DNS 服务器启动拒绝 DNS 客户端上服务攻击的系统。|  
|基于 DNS 命名的实体 (DANE) 的身份验证|新增功能|你可以使用 TLSA （传输层安全性身份验证） 记录以信息提供给 DNS 客户端，指出他们很有可能会从你的域名证书哪些 CA。 这可以防止某人可能会在此处损坏 DNS 缓存指向其自己的网站，并提供他们颁发的不同 CA 证书拦截中攻击。|  
|未知的记录支持|新增功能|你可以添加明确不支持使用未知的记录功能的 Windows DNS 服务器的记录。|  
|IPv6 根提示|新增功能|你可以使用原始 IPV6 根提示支持执行 Internet 名称分辨率使用 IPV6 根服务器。|  
|Windows PowerShell 支持|改进了|新的 Windows PowerShell cmdlet 是适用于 DNS 服务器。|  
  
## <a name="dns-policies"></a>DNS 策略

你可以使用 DNS 策略地理位置基于交通管理、 智能 DNS 响应基于一天时间，管理单个 DNS 服务器 split\ 脑部署中配置了在 DNS 查询等应用筛选器。 以下各项提供有关这些功能的详细信息。

-   **应用程序负载平衡。** 在部署的不同位置的应用的多个实例后，你可以使用 DNS 策略平衡不同的应用程序的情况下，动态分配的应用程序的交通加载之间的交通负载。

-   **Geo\ 位置基于交通管理。** 你可以使用 DNS 策略允许响应 DNS 客户端查询基于地理位置的客户端和资源的客户端尝试连接时，主要和辅助 DNS 服务器客户端提供最近资源的 IP 地址。 

-   **拆分脑 DNS。** Split\ 脑 DNS DNS 记录分为相同 DNS 服务器，在不同的区域范围内，DNS 客户端接收基于这些客户端是内部或外部的客户端的是否响应。 独立 DNS 服务器上，您可以配置 split\ 脑 DNS 集成的 Active Directory 区域或区域。

-   **筛选。** 您可以配置 DNS 策略创建查询均基于你提供的条件的筛选器。 在 DNS 策略查询筛选器允许你配置 DNS 服务器以自定义根据 DNS 查询和 DNS 客户端发送 DNS 查询的方式进行响应。 
-   **讨论。** 你可以使用 DNS 策略将恶意 DNS 客户端重定向到 non\ 存在而不是将它们定向到它们尝试访问的计算机的 IP 地址。

-   **基于重定向的一天的时间。** 你可以使用 DNS 策略使用的基于当天的时间 DNS 策略应用程序交通分散不同的应用程序的地理位置分布式实例。 
  
你还可以使用 DNS 策略 Active Directory 集成 DNS 的区域。

有关详细信息，请参阅[DNS 策略方案指南](deploy/DNS-Policies-Overview.md)。

## <a name="response-rate-limiting"></a>响应速率限制

您可以配置 RRL 设置来控制如何时进行应对请求 DNS 客户端到服务器接收定位相同的客户端的多个请求。 通过执行此操作，你可以防止其他人发送使用 DNS 服务器拒绝服务 (Dos) 攻击。 例如，使用自动程序网络可以发送给你的请求使用第三个计算机的 IP 地址的 DNS 服务器的请求。 不 RRL，所有请求洪水第三个计算机可能都响应 DNS 服务器。 当你使用 RRL 时，您可以配置以下设置：  
  
-   **每秒响应**。 这是最大的大量相同的响应，将为客户端一秒钟内的时间。  
  
-   **每秒错误**。 这是最大的大量错误响应到同一个客户端发送一个秒钟内的时间。  
  
-   **窗口**。 这是的秒如果进行了太多请求响应客户端将暂停为其数。  
  
-   **泄漏率**。 这是频率 DNS 服务器将回复查询响应暂停期间。 例如，如果服务器挂起 10 秒钟的客户端的响应，并且泄漏率 5，服务器仍将响应的每个 5 查询发送一个查询而异。 这允许合法的客户端，才能响应，即使他们子网或 FQDN 限制响应率应用 DNS 服务器。  
  
-   **TC 率**。 这用于告诉客户端尝试 TCP 时挂起响应客户端进行连接。 例如，如果 TC 率 3 平板电脑，并且服务器挂起响应给定的客户端，服务器将颁发收到每 3 查询的 TCP 连接的请求。 请确保的值为 TC 率低于泄漏速率，若要为客户提供泄漏响应前通过 TCP 连接的选项。  
  
-   **最大的响应**。 这是最大的数响应服务器将在发布给客户端时挂起的响应。  
  
-   **白色列出域**。 这是从 RRL 设置中排除的域的列表。  
  
-   **白色列表子网**。 这是子网要排除从 RRL 设置列表。  
  
-   **白色列表服务器接口**。 这是 DNS 服务器接口要排除从 RRL 设置列表。  
  
## <a name="dane-support"></a>DANE 支持

你可以使用 DANE 支持 \ （RFC 6394 和 6698\） 若要指定 DNS 客户端 CA 它们预见证书颁发从域名称的托管了 DNS 服务器。 这将阻止形式的人将能够破坏 DNS 缓存并 DNS 名称指向其自己的 IP 地址的男子在中间攻击。  
  
例如，假设主机由使用证书颁发机构已知名为 CA1 www.contoso.com 在使用 SSL 安全网站。 其他人仍可能能够从不同，不以便-有名，证书颁发机构名为 CA2 获取 www.contoso.com 证书。 然后，实体举办 www.contoso.com 伪网站可能能够损坏 DNS 客户端或服务器以 www.contoto.com 指向其假站点的缓存。 最终用户将会从 CA2，所出示的证书可能只是确认其和连接到假的站点。 与 DANE，客户将向要求提供 TLSA 记录 contoso.com DNS 服务器发出请求，并了解有关 www.contoso.com 证书已由 CA1 的问题。 如果从另一台 CA 出示的证书，则中止连接。  
  
## <a name="unknown-record-support"></a>未知的记录支持

"未知"是记录其 RDATA 格式不知道到 DNS 服务器。 未知的记录 (RFC 3597) 类型新添加的支持意味着你可以到二进制-线格式中的 Windows DNS 服务器区域中添加的不受支持的记录类型。 Windows 缓存解析程序已处理未知的记录类型的能力。 Windows DNS 服务器不执行任何记录的特定处理未知记录，但将它发送响应中重新如果查询收到它。  
  
## <a name="ipv6-root-hints"></a>IPv6 根提示

IPV6 根提示，通过 IANA，发布已添加到 windows DNS 服务器。 Internet 名称查询现在可以使用 IPv6 根服务器执行名称分辨率。

## <a name="windows-powershell-support"></a>Windows PowerShell 支持

Windows Server 2016 引入的以下新的 Windows PowerShell cmdlet 和参数。
  
-   **添加 DnsServerRecursionScope**。 此 cmdlet DNS 服务器上创建新的递归范围。 DNS 策略使用递归范围指定转发器用于 DNS 查询的列表。  
  
-   **删除 DnsServerRecursionScope**。 此 cmdlet 中删除现有递归范围。  
  
-   **设置 DnsServerRecursionScope**。 此 cmdlet 更改现有递归范围的设置。  
  
-   **获取 DnsServerRecursionScope**。 此 cmdlet 检索有关现有递归范围的信息。  
  
-   **添加 DnsServerClientSubnet**。 此 cmdlet 创建新 DNS 客户端子网。 DNS 策略使用子网找出 DNS 客户端所在的位置。  
  
-   **删除 DnsServerClientSubnet**。 此 cmdlet 中删除现有 DNS 客户端子网。  
  
-   **设置 DnsServerClientSubnet**。 此 cmdlet 更改的设置的现有的 DNS 客户端子网。  
  
-   **获取 DnsServerClientSubnet**。 此 cmdlet 检索有关现有 DNS 客户端子网信息。  
  
-   **添加 DnsServerQueryResolutionPolicy**。 此 cmdlet 创建一个新的 DNS 查询分辨率策略。 用于指定 DNS 查询分辨率策略如何操作，或如果查询响应根据不同的条件。  
  
-   **删除 DnsServerQueryResolutionPolicy**。 此 cmdlet 中删除现有 DNS 策略。  
  
-   **设置 DnsServerQueryResolutionPolicy**。 此 cmdlet 更改现有 DNS 策略的设置。  
  
-   **获取 DnsServerQueryResolutionPolicy**。 此 cmdlet 检索有关现有 DNS 策略的信息。  
  
-   **启用 DnsServerPolicy**。 此 cmdlet 使现有 DNS 策略。  
  
-   **禁用 DnsServerPolicy**。 此 cmdlet 禁用现有 DNS 策略。  
  
-   **添加 DnsServerZoneTransferPolicy**。 此 cmdlet 创建一个新的 DNS 服务器区域传输策略。 DNS 区域传输策略指定拒绝还是忽略区域传输根据不同的条件。  
  
-   **删除 DnsServerZoneTransferPolicy**。 此 cmdlet 中删除现有 DNS 服务器区域传输策略。  
  
-   **设置 DnsServerZoneTransferPolicy**。 此 cmdlet 更改现有 DNS 服务器区域传输策略的设置。  
  
-   **获取 DnsServerResponseRateLimiting**。 此 cmdlet 检索 RRL 设置。  
  
-   **设置 DnsServerResponseRateLimiting**。 此 cmdlet 更改 RRL settigns。  
  
-   **添加 DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet DNS 服务器上创建一个 RRL 例外列表。  
  
-   **获取 DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet 检索 RRL excception 列表。  
  
-   **删除 DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet 中删除现有 RRL 异常列表。  
  
-   **设置 DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet 更改 RRL 异常列表。  
  
-   **添加 DnsServerResourceRecord**。 更新此 cmdlet 支持未知的记录类型。  
  
-   **获取 DnsServerResourceRecord**。 更新此 cmdlet 支持未知的记录类型。  
  
-   **删除 DnsServerResourceRecord**。 更新此 cmdlet 支持未知的记录类型。  
  
-   **设置 DnsServerResourceRecord**。 更新此 cmdlet 支持未知的记录类型

有关详细信息，请参阅以下 Windows Server 2016 Windows PowerShell 命令参考主题。

- [DnsServer 模块](https://technet.microsoft.com/itpro/powershell/windows/dns-server/index)
- [DnsClient 模块](https://technet.microsoft.com/itpro/powershell/windows/dns-client/index)

## <a name="see-also"></a>请参阅  
  
-   [什么是 DNS 客户端中的新增功能](What-s-New-in-DNS-Client.md)  
  

  

