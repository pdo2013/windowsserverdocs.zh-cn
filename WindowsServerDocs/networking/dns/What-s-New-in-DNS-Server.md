---
title: Windows Server 中 DNS 服务器的新增功能
description: 本主题概述了 Windows Server 2016 及更高版本中 DNS 服务器的新增功能
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: de502d7be023d12e3350063e467a60356b2472c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406238"
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>Windows Server 中 DNS 服务器的新增功能

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍 Windows Server 2016 中新增或更改的域名系统（DNS）服务器功能。  
  
在 Windows Server 2016 中，DNS 服务器提供以下几个方面的增强支持。  
  
|功能|新功能或改进功能|描述|  
|-----------------|-------------------|---------------|  
|DNS 策略|新增|你可以配置 DNS 策略来指定 DNS 服务器如何响应 DNS 查询。 DNS 响应可以基于客户端 IP 地址（位置）、当天的时间和多个其他参数。 DNS 策略启用位置感知的 DNS、流量管理、负载平衡、裂脑 DNS 和其他方案。|  
|响应速率限制（RRL）|新增|可以在 DNS 服务器上启用响应速率限制。 这样做可以避免恶意系统在 DNS 客户端上使用 DNS 服务器启动拒绝服务攻击的可能性。|  
|命名实体的基于 DNS 的身份验证（窗格会）|新增|你可以使用 TLSA （传输层安全身份验证）记录向 DNS 客户端提供信息，这些客户端会向你的域名提供证书所需的 CA。 这可以防止中间人攻击，有人可能会损坏 DNS 缓存以指向自己的网站，并提供从其他 CA 颁发的证书。|  
|未知记录支持|新增|你可以使用未知的记录功能添加 Windows DNS 服务器未显式支持的记录。|  
|IPv6 根提示|新增|可以使用本地 IPV6 根提示支持通过 IPV6 根服务器执行 internet 名称解析。|  
|Windows PowerShell 支持|改进功能|新的 Windows PowerShell cmdlet 可用于 DNS 服务器。|  
  
## <a name="dns-policies"></a>DNS 策略

可以将 DNS 策略用于基于地理位置的流量管理、基于一天中的时间的智能 DNS 响应、管理为 split @ no__t-0brain 部署配置的单个 DNS 服务器、对 DNS 查询应用筛选器等。 以下各项提供了有关这些功能的更多详细信息。

-   **应用程序负载平衡。** 在不同位置部署了多个应用程序实例后，可以使用 DNS 策略来平衡不同应用程序实例之间的流量负载，从而动态分配应用程序的流量负载。

-   **基于 Geo @ no__t-1Location 的流量管理。** 你可以使用 DNS 策略来允许主 DNS 服务器和辅助 DNS 服务器根据客户端尝试连接到的客户端和资源的地理位置来响应 DNS 客户端查询，并为客户端提供最接近的 IP 地址资源. 

-   **裂脑 DNS。** 使用 split @ no__t-0brain DNS 时，DNS 记录会拆分为同一 DNS 服务器上的不同区域作用域，并且 DNS 客户端将基于客户端是内部客户端还是外部客户端接收响应。 可以为 Active Directory 集成的区域或独立 DNS 服务器上的区域配置 split @ no__t-0brain DNS。

-   **滤除.** 你可以配置 DNS 策略来创建基于你提供的条件的查询筛选器。 DNS 策略中的查询筛选器允许你将 DNS 服务器配置为基于发送 DNS 查询的 DNS 查询和 DNS 客户端以自定义方式进行响应。 
-   **取证.** 你可以使用 DNS 策略将恶意 DNS 客户端重定向到非 @ no__t-0existent IP 地址，而不是将它们定向到他们尝试访问的计算机。

-   **基于时间的重定向的时间。** 你可以使用 DNS 策略，通过基于一天中的时间将应用程序流量分布到应用程序的不同地理位置。 
  
你还可以将 DNS 策略用于 Active Directory 集成的 DNS 区域。

有关详细信息，请参阅[DNS 策略方案指南](deploy/DNS-Policies-Overview.md)。

## <a name="response-rate-limiting"></a>响应速率限制

你可以配置 RRL 设置，以控制当服务器收到针对同一客户端的多个请求时如何响应 DNS 客户端的请求。 这样做可以防止某人使用您的 DNS 服务器发送拒绝服务（Dos）攻击。 例如，bot 网络可以使用第三台计算机的 IP 地址作为请求者来向 DNS 服务器发送请求。 如果没有 RRL，你的 DNS 服务器可能会响应所有请求，从而淹没第三台计算机。 使用 RRL 时，可以配置以下设置：  
  
-   **每秒响应数**。 这是在一秒内对客户端提供相同响应的最大次数。  
  
-   **每秒的错误数**。 这是一秒内将错误响应发送到同一客户端的最大次数。  
  
-   **窗口**。 这是对客户端发出的请求数过多的情况下将暂停对客户端的响应的秒数。  
  
-   **泄漏速率**。 这是在挂起响应期间 DNS 服务器响应查询的频率。 例如，如果服务器挂起了10秒的对客户端的响应，并且泄漏速率为5，则服务器仍将对每个发送的5个查询响应一个查询。 这样，即使 DNS 服务器在其子网或 FQDN 上应用了响应速率限制，合法的客户端也可以获得响应。  
  
-   **TC 费率**。 这用于告知客户端在对客户端的响应挂起时尝试与 TCP 连接。 例如，如果 TC 速率为3，并且服务器暂停对给定客户端的响应，则服务器将对每3个接收的查询发出 TCP 连接请求。 确保 "TC 速率" 的值低于 "泄漏速率"，以便为客户端提供在泄漏响应之前通过 TCP 进行连接的选项。  
  
-   **最大响应**。 这是挂起响应时服务器将向客户端发出的最大响应数。  
  
-   **白色列表域**。 这是要从 RRL 设置中排除的域的列表。  
  
-   **白色列表子网**。 这是要从 RRL 设置中排除的子网的列表。  
  
-   **白色列表服务器接口**。 这是要从 RRL 设置中排除的 DNS 服务器接口的列表。  
  
## <a name="dane-support"></a>窗格会支持

你可以使用窗格会支持 \(RFC 6394 和 6698 @ no__t-1，指定 DNS 客户端应将证书颁发给 dns 服务器中的域名称。 这可防止攻击者利用中间人攻击，使得用户能够破坏 DNS 缓存并将 DNS 名称指向其自己的 IP 地址。  
  
例如，假设你在 www.contoso.com 通过使用名为 CA1 的知名颁发机构颁发的证书来托管使用 SSL 的安全网站。 有人可能仍然能够从名为 CA2 的不同、不是众所周知的证书颁发机构获取 www.contoso.com 的证书。 然后，托管虚设 www.contoso.com 网站的实体可能会损坏客户端或服务器的 DNS 缓存，以将 www.contoto.com 指向其虚假站点。 最终用户将从 CA2 提供证书，并可直接确认并连接到虚假站点。 使用窗格会，客户端将向 DNS 服务器发出请求，以便 contoso.com 请求 TLSA 记录，并了解 www.contoso.com 的证书是由 CA1 颁发的。 如果使用其他 CA 颁发的证书，则会中止连接。  
  
## <a name="unknown-record-support"></a>未知记录支持

"未知记录" 是指其 RDATA 格式对 DNS 服务器未知的 RR。 新添加的对未知记录（RFC 3597）类型的支持意味着可以将不受支持的记录类型添加到采用二进制实时格式的 Windows DNS 服务器区域。 Windows 缓存解析程序已具有处理未知记录类型的能力。 Windows DNS 服务器不会对未知记录执行任何特定于记录的处理，但会在收到查询的情况下将其发送回响应。  
  
## <a name="ipv6-root-hints"></a>IPv6 根提示

由 IANA 发布的 IPV6 根提示已添加到 windows DNS 服务器。 Internet 名称查询现在可以使用 IPv6 根服务器来执行名称解析。

## <a name="windows-powershell-support"></a>Windows PowerShell 支持

Windows Server 2016 中引入了以下新的 Windows PowerShell cmdlet 和参数。
  
-   **DnsServerRecursionScope**。 此 cmdlet 将在 DNS 服务器上创建一个新的递归作用域。 DNS 策略使用递归作用域来指定要在 DNS 查询中使用的转发器列表。  
  
-   **DnsServerRecursionScope**。 此 cmdlet 将删除现有的递归作用域。  
  
-   **DnsServerRecursionScope**。 此 cmdlet 更改现有递归作用域的设置。  
  
-   **DnsServerRecursionScope**。 此 cmdlet 检索有关现有递归作用域的信息。  
  
-   **DnsServerClientSubnet**。 此 cmdlet 将创建新的 DNS 客户端子网。 DNS 策略使用子网来识别 DNS 客户端所在的位置。  
  
-   **DnsServerClientSubnet**。 此 cmdlet 将删除现有的 DNS 客户端子网。  
  
-   **DnsServerClientSubnet**。 此 cmdlet 更改现有 DNS 客户端子网的设置。  
  
-   **DnsServerClientSubnet**。 此 cmdlet 检索有关现有 DNS 客户端子网的信息。  
  
-   **DnsServerQueryResolutionPolicy**。 此 cmdlet 将创建新的 DNS 查询解析策略。 DNS 查询解析策略用于根据不同条件指定查询的响应方式（或查询的响应方式）。  
  
-   **DnsServerQueryResolutionPolicy**。 此 cmdlet 将删除现有的 DNS 策略。  
  
-   **DnsServerQueryResolutionPolicy**。 此 cmdlet 更改现有 DNS 策略的设置。  
  
-   **DnsServerQueryResolutionPolicy**。 此 cmdlet 检索有关现有 DNS 策略的信息。  
  
-   **DnsServerPolicy**。 此 cmdlet 将启用现有的 DNS 策略。  
  
-   **DnsServerPolicy**。 此 cmdlet 将禁用现有的 DNS 策略。  
  
-   **DnsServerZoneTransferPolicy**。 此 cmdlet 将创建新的 DNS 服务器区域传输策略。 DNS 区域传送策略指定是根据不同条件拒绝还是忽略区域传输。  
  
-   **DnsServerZoneTransferPolicy**。 此 cmdlet 将删除现有的 DNS 服务器区域传输策略。  
  
-   **DnsServerZoneTransferPolicy**。 此 cmdlet 更改现有 DNS 服务器区域传输策略的设置。  
  
-   **DnsServerResponseRateLimiting**。 此 cmdlet 检索 RRL 设置。  
  
-   **DnsServerResponseRateLimiting**。 此 cmdlet 更改 RRL 设置也。  
  
-   **DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet 在 DNS 服务器上创建 RRL 例外列表。  
  
-   **DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet 检索 RRL excception 列表。  
  
-   **DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet 将删除现有的 RRL 异常列表。  
  
-   **DnsServerResponseRateLimitingExceptionlist**。 此 cmdlet 更改 RRL 例外列表。  
  
-   **DnsServerResourceRecord**。 此 cmdlet 已更新为支持未知记录类型。  
  
-   **DnsServerResourceRecord**。 此 cmdlet 已更新为支持未知记录类型。  
  
-   **DnsServerResourceRecord**。 此 cmdlet 已更新为支持未知记录类型。  
  
-   **DnsServerResourceRecord**。 此 cmdlet 已更新为支持未知记录类型

有关详细信息，请参阅以下 Windows Server 2016 Windows PowerShell 命令参考主题。

- [DnsServer 模块](https://docs.microsoft.com/powershell/module/dnsserver/?view=win10-ps)
- [Set-dnsclient 模块](https://docs.microsoft.com/powershell/module/dnsclient/?view=win10-ps)

## <a name="see-also"></a>请参阅  
  
-   [DNS 客户端中的新增功能](What-s-New-in-DNS-Client.md)  
  

  

