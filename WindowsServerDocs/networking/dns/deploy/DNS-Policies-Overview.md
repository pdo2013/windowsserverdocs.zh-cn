---
title: DNS 策略概述
description: 本主题是 Windows Server 2016 DNS 策略方案指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 613bb7f43b382389dc0db953a48668147cfaee88
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356053"
---
# <a name="dns-policies-overview"></a>DNS 策略概述

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题来了解 Windows Server 2016 中的新策略。 可以将 DNS 策略用于基于地理位置的流量管理、基于一天中的时间的智能 DNS 响应、管理为 split @ no__t-0brain 部署配置的单个 DNS 服务器、对 DNS 查询应用筛选器等。 以下各项提供了有关这些功能的更多详细信息。

-   **应用程序负载平衡。** 在不同位置部署了多个应用程序实例后，可以使用 DNS 策略来平衡不同应用程序实例之间的流量负载，从而动态分配应用程序的流量负载。

-   **基于 Geo @ no__t-1Location 的流量管理。** 你可以使用 DNS 策略来允许主 DNS 服务器和辅助 DNS 服务器根据客户端尝试连接到的客户端和资源的地理位置来响应 DNS 客户端查询，并为客户端提供最接近的 IP 地址资源. 

-   **裂脑 DNS。** 使用 split @ no__t-0brain DNS 时，DNS 记录会拆分为同一 DNS 服务器上的不同区域作用域，并且 DNS 客户端将基于客户端是内部客户端还是外部客户端接收响应。 可以为 Active Directory 集成的区域或独立 DNS 服务器上的区域配置 split @ no__t-0brain DNS。

-   **滤除.** 你可以配置 DNS 策略来创建基于你提供的条件的查询筛选器。 DNS 策略中的查询筛选器允许你将 DNS 服务器配置为基于发送 DNS 查询的 DNS 查询和 DNS 客户端以自定义方式进行响应。 
-   **取证.** 你可以使用 DNS 策略将恶意 DNS 客户端重定向到非 @ no__t-0existent IP 地址，而不是将它们定向到他们尝试访问的计算机。

-   **基于时间的重定向的时间。** 你可以使用 DNS 策略，通过基于一天中的时间将应用程序流量分布到应用程序的不同地理位置。

## <a name="new-concepts"></a>新概念  
若要创建策略以支持上述方案，必须能够确定区域中的记录组、网络上的客户端组以及其他元素。 以下新的 DNS 对象表示这些元素：  

- **客户端子网：** 客户端子网对象表示一个 IPv4 或 IPv6 子网，其中的查询将查询提交到 DNS 服务器。 你可以创建子网，以便在以后根据请求所源自的子网定义要应用的策略。 例如，在 "裂脑 DNS" 方案中，可使用内部子网中的客户端的内部 IP 地址和外部子网中的客户端的 IP 地址，对名称（如<em>www.microsoft.com</em> ）的解析请求进行应答。

- **递归作用域：** 递归作用域是一组设置的唯一实例，这些设置控制 DNS 服务器上的递归。 递归作用域包含转发器列表，并指定是否启用了递归。 DNS 服务器可以有多个递归作用域。 DNS 服务器递归策略允许您为一组查询选择递归作用域。 如果 DNS 服务器对某些查询没有权威，则可以通过 DNS 服务器递归策略来控制如何解析这些查询。 可以指定要使用的转发器以及是否使用递归。

- **区域作用域：** DNS 区域可以有多个区域作用域，每个区域作用域包含其自己的一组 DNS 记录。 同一条记录可以出现在多个作用域中，具有不同的 IP 地址。 此外，区域传输是在区域范围级别进行的。 这意味着，主要区域中的区域作用域内的记录将传输到辅助区域中的同一区域作用域。

## <a name="types-of-policy"></a>策略类型

DNS 策略按级别和类型划分。 您可以使用查询解析策略来定义处理查询的方式，并使用区域传送策略来定义区域传输的发生方式。 可以在服务器级别或区域级别应用每个策略类型。

### <a name="query-resolution-policies"></a>查询解决策略

你可以使用 DNS 查询解析策略来指定 DNS 服务器处理传入解析查询的方式。 每个 DNS 查询解析策略都包含以下元素：  

|字段|描述|可能值|  
|---------|---------------|-------------------|  
|**名称**|策略名称|-最多256个字符<br />-可以包含对文件名有效的任何字符|  
|**状态**|策略状态|-Enable （默认值）<br />-已禁用|  
|**调配**|策略级别|-服务器<br />-Zone|  
|**处理顺序**|按级别对查询进行分类并将其应用于后，服务器将找到查询与条件匹配的第一个策略，并将其应用于 query|-数值<br />-每个策略的唯一值，包含同一级别并应用于值|  
|**Action**|要由 DNS 服务器执行的操作|-Allow （区域级别的默认值）<br />-Deny （服务器级别上的默认值）<br />-Ignore|  
|**据**|策略条件（和/或）以及要应用策略的条件的列表|-Condition 运算符（和/或）<br />-条件列表（请参阅下面的条件表格）|  
|**范围**|区域作用域和每个作用域的加权值的列表。 加权值用于负载平衡分布。 例如，如果此列表包含权重为3且 datacenter2 的权重为5的 datacenter1，则服务器将使用 datacentre1 三次发出的记录（8个请求中的一个记录）响应|-区域作用域列表（按名称）和权重|  

> [!NOTE]
> 服务器级别策略只能将值**Deny**或**Ignore**作为操作。

DNS 策略条件字段由以下两个元素组成：


|              名称               |                                         描述                                          |                                                                                                                               示例值                                                                                                                               |
|---------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        **客户端子网**        | 预定义的客户端子网的名称。 用于验证从中发送查询的子网。 |                             -   **EQ、西班牙、法国**-如果子网被标识为西班牙或法国，则解析为 true<br />-   **NE、加拿大、墨西哥**-如果客户端子网为除加拿大和墨西哥以外的任何子网，则解析为 true                             |
|     **传输协议**      |        查询中使用的传输协议。 可能的条目为**UDP**和**TCP**        |                                                                                                                    -   **EQ，TCP**<br />-   **EQ、UDP**                                                                                                                     |
|      **Internet 协议**      |        查询中使用的网络协议。 可能的条目是**IPv4**和**IPv6**        |                                                                                                                   -   **EQ、IPv4**<br />-   **EQ、IPv6**                                                                                                                    |
| **服务器接口 IP 地址** |                   传入 DNS 服务器网络接口的 IP 地址                   |                                                                                                              -   **EQ，10.0.0.1**<br />-   **EQ、192.168.1.1**                                                                                                              |
|            **FQDN**             |            查询中记录的 FQDN，可能使用通配符            | -   **EQ、www .com** -仅当查询尝试解析<em>www.contoso.com</em> FQDN 时，解析为 true<br />-   **EQ、\*.contoso.com、\*.woodgrove.com** -如果查询用于在*contoso.com***或***woodgrove.com*结束的任何记录，则解析为 true。 |
|         **查询类型**          |                          正在查询的记录类型（A、SRV、TXT）                          |                                                  如果查询正在请求 TXT**或**SRV 记录，-   **EQ，TXT，SRV**解析为 true<br />如果查询正在请求 MX 记录，则为 -   **EQ、MX** -解析为 true                                                   |
|         **当天的时间**         |                              接收查询的时间                               |                                                                    -   **EQ，10： 00-12：00，22： 00-23： 00** -如果在上午10点到中午之间，**或**在10PM 和晚上11点之间接收查询，则解析为 true                                                                    |

使用上述表格作为起点，下表可用于定义用于匹配任何类型的记录的查询的条件，但 contoso.com 域中的 SRV 记录是从 10.0.0.0/24 子网中的客户端通过 TCP 从接口10.0.0.3：  

|名称|ReplTest1|  
|--------|---------|  
|客户端子网|EQ、10.0.0.0/24|  
|传输协议|EQ，TCP|  
|服务器接口 IP 地址|EQ，10.0.0。3|  
|FQDN|EQ，* .com|  
|查询类型|NE，SRV|  
|当天的时间|EQ，20： 00-22：00|  

您可以创建同一级别的多个查询解析策略，只要它们具有不同的处理顺序值。 当有多个策略可用时，DNS 服务器将按以下方式处理传入的查询：  

![DNS 策略处理](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  

### <a name="recursion-policies"></a>递归策略  
递归策略是一**种特殊类型**的服务器级别策略。 递归策略控制 DNS 服务器对查询执行递归的方式。 仅当查询处理达到递归路径时，递归策略才适用。 对于一组查询，可以选择 "拒绝" 或 "忽略" 值。 或者，可以为一组查询选择一组转发器。  

可以使用递归策略来实现裂脑 DNS 配置。 在此配置中，DNS 服务器为查询的一组客户端执行递归，而 DNS 服务器不为该查询的其他客户端执行递归。  

递归策略包含与下表中的元素相同的常规 DNS 查询解析策略所包含的元素：  

|名称|描述|  
|--------|---------------|  
|**应用于递归**|指定仅应将此策略用于递归。|  
|**递归范围**|递归作用域的名称。|  

> [!NOTE]  
> 只能在服务器级别创建递归策略。  

### <a name="zone-transfer-policies"></a>区域传输策略  
区域传输策略控制 DNS 服务器是否允许区域传输。 可以在服务器级别或区域级别为区域复制创建策略。 服务器级别策略适用于在 DNS 服务器上发生的每个区域传输查询。 区域级别策略仅适用于在 DNS 服务器上托管的区域上的查询。 区域级别策略的最常见用途是实现阻止或安全列表。  

> [!NOTE]  
> 区域传输策略只能使用 DENY 或 IGNORE 作为操作。  

你可以使用以下服务器级别区域传输策略拒绝给定子网中的 contoso.com 域的区域传送：  

```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfContosoToFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  

您可以创建同一级别的多个区域传输策略，只要它们具有不同的处理顺序值。 当有多个策略可用时，DNS 服务器将按以下方式处理传入的查询：  

![多个区域传输策略的 DNS 进程](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  

## <a name="managing-dns-policies"></a>管理 DNS 策略  
可以使用 PowerShell 来创建和管理 DNS 策略。 以下示例介绍了可通过 DNS 策略配置的不同示例方案：  

### <a name="traffic-management"></a>流量管理  
你可以根据 DNS 客户端的位置，将基于 FQDN 的流量定向到不同的服务器。 下面的示例演示如何创建流量管理策略，以将特定子网中的客户定向到北美数据中心，并将其他子网定向到欧洲数据中心。  

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

脚本的前两行为北美和欧洲创建客户端子网对象。 后面的两行在 contoso.com 域中创建区域作用域，每个区域各有一个区域。 后面的两行在每个区域中创建一条记录，将 ww.contoso.com 关联到不同的 IP 地址，一个用于欧洲，另一个用于北美。 最后，该脚本的最后几行创建两个 DNS 查询解析策略，一个策略应用于北美子网，另一个应用于欧洲子网。  

### <a name="block-queries-for-a-domain"></a>阻止域查询  
你可以使用 DNS 查询解析策略来阻止查询到域。 下面的示例将阻止所有查询 treyresearch.net：  

```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  

### <a name="block-queries-from-a-subnet"></a>阻止子网中的查询  
还可以阻止来自特定子网的查询。 下面的脚本将创建 172.0.33.0/24 的子网，然后创建一个策略，以忽略来自该子网的所有查询：  

```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  

### <a name="allow-recursion-for-internal-clients"></a>允许内部客户端的递归  
可以通过使用 DNS 查询解析策略来控制递归。 下面的示例可用于为内部客户端启用递归，同时为裂脑方案中的外部客户端禁用递归。  

```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  

脚本中的第一行更改默认递归作用域，简单命名为 "."。禁用递归。 第二行创建一个名为*InternalClients*的递归作用域，其中启用了递归。 第三行创建一个策略，以将新创建的递归范围应用于通过将10.0.0.34 作为 IP 地址的服务器接口传入的任何查询。  

### <a name="create-a-server-level-zone-transfer-policy"></a>创建服务器级别区域传输策略  
你可以使用 DNS 区域传输策略以更精细的方式控制区域传输。 下面的示例脚本可用于允许对给定子网上的任何服务器进行区域传输：  

```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  

脚本中的第一行将创建一个名为*AllowedSubnet*的子网对象，其中包含 IP 块 172.21.33.0/24。 第二行创建区域传输策略，以允许将区域传输到之前创建的子网上的任何 DNS 服务器。  

### <a name="create-a-zone-level-zone-transfer-policy"></a>创建区域级别区域传输策略  
你还可以创建区域级别区域传输策略。 下面的示例忽略来自 IP 地址为本例为10.0.0.33 的服务器接口的 contoso.com 的区域传输请求：  

```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "eq,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  

## <a name="dns-policy-scenarios"></a>DNS 策略方案

有关如何在特定情况下使用 DNS 策略的信息，请参阅本指南中的以下主题。

- [将 DNS 策略用于基于地理位置的流量管理和主服务器](primary-geo-location.md)  
- [将 DNS 策略用于基于地理位置的流量管理和主要辅助部署](primary-secondary-geo-location.md)  
- [根据一天中的时间将 DNS 策略用于智能 DNS 响应](dns-tod-intelligent.md)
- [基于当天使用 Azure 云应用服务器的时间的 DNS 响应](dns-tod-azure-cloud-app-server.md)
- [将 DNS 策略用于裂脑 DNS 部署](split-brain-DNS-deployment.md)
- [在 Active Directory 中对裂脑 DNS 使用 DNS 策略](dns-sb-with-ad.md)
- [使用 DNS 策略对 DNS 查询应用筛选器](apply-filters-on-dns-queries.md)
- [使用 DNS 策略实现应用程序负载平衡](app-lb.md)
- [将 DNS 策略用于应用程序负载平衡和地理位置识别](app-lb-geo.md)

## <a name="using-dns-policy-on-read-only-domain-controllers"></a>在只读域控制器上使用 DNS 策略

DNS 策略兼容只读域控制器。 请注意，需要重新启动 DNS 服务器服务，才能在只读域控制器上加载新的 DNS 策略。 这不是可写域控制器所必需的。
