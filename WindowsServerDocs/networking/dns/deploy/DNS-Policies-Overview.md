---
title: DNS 策略概述
description: 本主题是 DNS 策略方案指南 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6c4d8f9bb6c56e8f90a90cd4e77565a39211f719
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446434"
---
# <a name="dns-policies-overview"></a>DNS 策略概述

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题以了解有关 DNS 策略，这是 Windows Server 2016 中的新增功能。 对于地理位置基于流量管理，基于一天时间，以管理单个 DNS 服务器配置为拆分的智能 DNS 响应，可以使用 DNS 策略\-大脑部署，DNS 查询和的详细信息上应用筛选器。 以下各项提供有关这些功能的更多详细信息。

-   **应用程序负载均衡。** 当已部署应用程序在不同位置的多个实例时，可以使用 DNS 策略来均衡动态分配的流量负载的应用程序在不同的应用程序实例之间的流量负载。

-   **异地\-基于位置的流量管理。** 可以使用 DNS 策略以允许主要和辅助 DNS 服务器响应 DNS 客户端查询基于的客户端并向其客户端尝试连接，该资源的地理位置提供客户端最接近的 IP 地址资源。 

-   **拆分式 DNS。** 使用拆分\-澍 DNS，DNS 记录拆分为相同的 DNS 服务器上的不同区域作用域和 DNS 客户端接收基于客户端是内部或外部客户端的响应。 你可以配置拆分式\-澍 DNS Active Directory 集成区域或独立的 DNS 服务器上的区域。

-   **筛选。** 可以配置 DNS 策略来创建查询筛选器的基于你提供的条件。 DNS 策略中的查询筛选器，可以配置 DNS 服务器根据 DNS 查询和发送 DNS 查询的 DNS 客户端的方式自定义响应。 
-   **取证。** 可以使用 DNS 策略重定向到非恶意的 DNS 客户端\-存在的 IP 地址，而不是将它们定向到他们尝试访问的计算机。

-   **一天的时间基于重定向。** 可以使用 DNS 策略以使用基于一天的时间的 DNS 策略将应用程序流量分配到不同的地理位置分散的应用程序实例。

## <a name="new-concepts"></a>新的概念  
若要创建策略，以支持上面列出的方案，有必要将无法确定组的区域，在其他元素之间的网络上的客户端组中的记录。 这些元素可以通过以下新的 DNS 对象表示：  

- **客户端子网：** 客户端子网对象表示从其查询提交到 DNS 服务器的 IPv4 或 IPv6 子网。 您可以创建要更高版本定义策略，以应用根据请求来自的子网的子网。 例如，在裂脑 DNS 情况，如名称解析的请求<em>www.microsoft.com</em>可以找到相应的内部 IP 地址向客户端从内部子网，并向外部客户端不同的 IP 地址子网。

- **递归作用域：** 递归作用域是控制递归 DNS 服务器上的设置的组的唯一实例。 递归作用域包含的转发器列表，并指定递归处于启用状态。 DNS 服务器可以有多个递归作用域。 DNS 服务器递归策略允许您选择一组查询的递归作用域。 如果 DNS 服务器不是权威对于某些查询，DNS 服务器递归策略使您得以控制如何解决这些查询。 您可以指定使用以及是否使用递归的转发器。

- **区域作用域：** DNS 区域可以有多个区域作用域，包含其自己的 DNS 记录集的每个区域作用域。 同一条记录可出现在多个作用域，使用不同的 IP 地址。 此外，在区域作用域级别完成区域传输。 这意味着在主要区域的区域作用域中的记录会被传输到辅助区域中的相同区域作用域。

## <a name="types-of-policy"></a>类型的策略

DNS 策略分为按级别和类型。 可以使用查询解析策略，以定义如何处理查询和区域传送策略，以定义如何发生区域传送。 可以在服务器级别或区域级别每个策略类型进行应用。

### <a name="query-resolution-policies"></a>查询解析策略

DNS 查询解析策略可用于指定如何传入解析由 DNS 服务器来处理查询。 每个 DNS 查询解析策略包含以下元素：  

|字段|描述|可能值|  
|---------|---------------|-------------------|  
|**名称**|策略名称|-最多 256 个字符<br />-可以包含有效输入文件的名称的任何字符|  
|**状态**|策略状态|-启用 （默认值）<br />-已禁用|  
|**Level**|策略级别|-   Server<br />区域|  
|**处理顺序**|该服务器后查询级别进行分类，并对应用，一旦发现为其查询与条件匹配，并将其查询应用的第一个策略|-数字值<br />每个策略包含相同级别和值对应用的唯一值|  
|**操作**|要由 DNS 服务器执行的操作|-允许 （默认设置区域级别）<br />拒绝 （服务器级别上的默认值）<br />-   Ignore|  
|**条件**|策略条件 （和/或） 和条件，以便满足要应用的策略列表|条件运算符 （和/或）<br />-条件 （请参阅下面的条件表） 的列表|  
|**范围**|区域作用域和每个作用域的加权的值的列表。 加权的值用于负载均衡分发。 例如，如果此列表包括 datacenter1 使用权重为 3 和 datacenter2 使用权重为 5 的服务器将响应一条记录从 datacentre1 三次超出 8 个请求|-列表中的区域 （按名称） 的作用域和权重|  

> [!NOTE]
> 服务器级策略只能具有值**拒绝**或**忽略**用作一项操作。

DNS 策略条件字段组成的两个元素：


|              名称               |                                         描述                                          |                                                                                                                               示例值                                                                                                                               |
|---------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        **客户端子网**        | 预定义的客户端子网的名称。 用于验证从其发送查询的子网。 |                             -   **EQ、 西班牙、 法国**-解析为 true，如果子网被标识为西班牙或法国<br />-   **NE、 加拿大、 墨西哥**-解析为 true，如果客户端子网是加拿大和墨西哥以外的任何子网                             |
|     **传输协议**      |        传输协议在查询中使用。 可能的项都**UDP**和**TCP**        |                                                                                                                    -   **EQ,TCP**<br />-   **EQ,UDP**                                                                                                                     |
|      **Internet 协议**      |        在查询中使用的网络协议。 可能的项都**IPv4**和**IPv6**        |                                                                                                                   -   **EQ,IPv4**<br />-   **EQ,IPv6**                                                                                                                    |
| **服务器接口 IP 地址** |                   传入的 DNS 服务器网络接口的 IP 地址                   |                                                                                                              -   **EQ,10.0.0.1**<br />-   **EQ,192.168.1.1**                                                                                                              |
|            **FQDN**             |            在查询中，记录的因为它使用通配符的 FQDN            | -   **EQ、 www.contoso.com** -解析总数 rue 仅当查询在尝试解析<em>www.contoso.com</em> FQDN<br />-   **EQ、\*。 contoso.com\*。 woodgrove.com** -解析为 true，如果查询的任何记录以结尾*contoso.com***或***woodgrove.com* |
|         **查询类型**          |                          正在查询的记录 （A、 SVR、 TXT） 的类型                          |                                                  -   **EQ、 TXT、 SRV** -解析为 true，如果查询正在请求 TXT**或**SRV 记录<br />-   **EQ、 MX** -解析为 true，如果查询正在请求 MX 记录                                                   |
|         **当天的时间**         |                              接收查询的时间                               |                                                                    -   **EQ、 10:00-12:00，22:00-23:00** -解析为 true，如果查询收到之间上午 10 点和中午**或**晚上 10 点和晚上 11 点之间                                                                    |

使用上的表作为起始点下, 表可用来定义用于匹配任何类型的记录，但来自 10.0.0.0/24 子网中的客户端通过 TCP 8 和通过晚上 10 点之间的 contoso.com 域中的 SRV 记录的查询的判据我接口 10.0.0.3:  

|名称|值|  
|--------|---------|  
|客户端子网|EQ,10.0.0.0/24|  
|传输协议|EQ,TCP|  
|服务器接口 IP 地址|EQ,10.0.0.3|  
|FQDN|EQ,*.contoso.com|  
|查询类型|NE,SRV|  
|当天的时间|EQ,20:00-22:00|  

可以创建多个查询的相同级别的解决策略，只要它们具有不同的值的处理顺序。 在多个策略都不可用，DNS 服务器将按以下方式处理传入的查询：  

![DNS 策略处理](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  

### <a name="recursion-policies"></a>递归策略  
递归策略是一种特殊**类型**服务器级别策略。 递归策略控制如何在 DNS 服务器执行递归查询。 递归策略应用仅在查询处理到达的递归路径。 可以选择一组查询的递归的拒绝或忽略的值。 或者，可以选择一系列转发器的一组查询。  

递归策略可用于实现 Split-brain DNS 配置。 在此配置中，DNS 服务器执行递归查询，客户端的一组时的 DNS 服务器不会执行该查询的其他客户端递归。  

递归策略包含相同的元素包含常规的 DNS 查询解析策略，以及下表中的元素：  

|名称|描述|  
|--------|---------------|  
|**在递归应用**|指定此策略应仅使用递归。|  
|**递归作用域**|递归作用域的名称。|  

> [!NOTE]  
> 仅可以在服务器级别创建递归策略。  

### <a name="zone-transfer-policies"></a>区域传输策略  
区域传输策略控制是否允许区域传送或不是由你的 DNS 服务器。 可以在服务器级别或区域级别创建区域传送的策略。 服务器级策略适用于 DNS 服务器发生的每个区域传输查询。 区域级别策略仅适用于托管 DNS 服务器上的区域的查询。 区域级别策略的最常见用途是实现已阻止或安全列表。  

> [!NOTE]  
> 区域传输策略仅能使用拒绝或忽略作为操作。  

下面的服务器级别区域传输策略可用于从给定子网拒绝 contoso.com 域的区域复制：  

```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfContosoToFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  

可以创建多个区域传送的同一级别的策略，只要它们具有不同的值的处理顺序。 在多个策略都不可用，DNS 服务器将按以下方式处理传入的查询：  

![DNS 区域传输的多个策略的过程](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  

## <a name="managing-dns-policies"></a>管理 DNS 策略  
您可以创建和使用 PowerShell 管理 DNS 策略。 下面的示例经历不同的示例方案，你可以通过 DNS 策略配置：  

### <a name="traffic-management"></a>流量管理  
可以根据不同的服务器，具体取决于 DNS 客户端位置到 FQDN 的流量进行定向。 下面的示例演示如何创建管理策略，以引导客户从到北美的数据中心的特定子网和欧洲数据中心到另一个子网的流量。  

```  
Add-DnsServerClientSubnet -Name "NorthAmericaSubnet" -IPv4Subnet "172.21.33.0/24"  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "172.17.44.0/24"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "NorthAmericaZoneScope"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.17.97.97" -ZoneScope "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.21.21.21" -ZoneScope "NorthAmericaZoneScope"  
Add-DnsServerQueryResolutionPolicy -Name "NorthAmericaPolicy" -Action ALLOW -ClientSubnet "eq,NorthAmericaSubnet" -ZoneScope "NorthAmericaZoneScope,1" -ZoneName "Contoso.com"  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName contoso.com  
```  

该脚本的前两行创建客户端位于北美和欧洲的子网对象。 在此之后的两个行创建 contoso.com 域，一个用于每个区域内的区域范围。 在此之后的两行在将关联到不同的 IP 地址、 一个用于欧洲、 北美的另一个 ww.contoso.com 每个区域中创建一条记录。 最后，该脚本的最后一个行创建两个 DNS 查询解析策略，一个要应用于北美子网，另一个到欧洲子网。  

### <a name="block-queries-for-a-domain"></a>块查询的域  
您可以使用 DNS 查询解析策略来阻止查询到域。 下面的示例将阻止对 treyresearch.net 的所有查询：  

```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  

### <a name="block-queries-from-a-subnet"></a>从子网块查询  
您还可以阻止来自特定子网的查询。 下面的脚本创建 172.0.33.0/24 的子网，然后创建一个策略，以忽略来自该子网的所有查询：  

```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  

### <a name="allow-recursion-for-internal-clients"></a>允许内部客户端进行递归  
可以通过使用 DNS 查询解析策略来控制递归。 下面的示例可用于禁用裂脑情况中的外部客户端时的内部客户端，启用递归。  

```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  

在脚本中的第一个行更改默认递归作用域，只需命名为"。"（点） 若要禁用递归。 第二行创建名为的递归作用*InternalClients*与递归已启用。 第三行创建要应用的策略和新创建递归到经过具有 10.0.0.34 作为 IP 地址的服务器接口的任何查询的作用域。  

### <a name="create-a-server-level-zone-transfer-policy"></a>创建服务器级别区域传输策略  
可以通过使用 DNS 区域传送策略来控制在更精细的窗体中的区域传送。 下面的示例脚本可用于为给定的子网上任何服务器允许区域复制：  

```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  

在脚本中的第一行创建一个名为的子网对象*AllowedSubnet*用的 IP 阻止 172.21.33.0/24。 第二行创建区域传输策略以允许区域复制到任何 DNS 服务器上以前创建的子网。  

### <a name="create-a-zone-level-zone-transfer-policy"></a>创建区域级别区域传输策略  
此外可以创建区域级别区域传输策略。 下面的示例将忽略来自具有 10.0.0.33 的 IP 地址的服务器接口的 contoso.com 区域传送的任何请求：  

```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "ne,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  

## <a name="dns-policy-scenarios"></a>DNS 策略方案

有关如何使用 DNS 策略执行特定方案的信息，请参阅本指南中的以下主题。

- [使用基于 DNS 策略的地理位置和主服务器的流量管理](primary-geo-location.md)  
- [对于地理位置基于流量管理和主要-辅助部署使用 DNS 策略](primary-secondary-geo-location.md)  
- [使用 DNS 策略执行基于一天的时间智能 DNS 响应](dns-tod-intelligent.md)
- [基于 Azure 的一天的时间的 DNS 响应云应用程序服务器](dns-tod-azure-cloud-app-server.md)
- [使用针对拆分式 DNS 部署 DNS 策略](split-brain-DNS-deployment.md)
- [使用 Active Directory 中的拆分式 DNS 的 DNS 策略](dns-sb-with-ad.md)
- [在 DNS 查询上应用筛选器使用 DNS 策略](apply-filters-on-dns-queries.md)
- [使用 DNS 策略执行应用程序负载均衡](app-lb.md)
- [使用 DNS 策略执行应用程序负载平衡和地理位置感知](app-lb-geo.md)

## <a name="using-dns-policy-on-read-only-domain-controllers"></a>在只读域控制器上使用 DNS 策略

DNS 策略适用于只读域控制器。 请注意，重新启动 DNS 服务器服务是需要新的 DNS 策略要加载在只读域控制器上。 这不是可写域控制器上必需的。
