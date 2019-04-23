---
title: 使用针对基于时间的智能 DNS 响应的 DNS 策略
description: 本主题是 DNS 策略方案指南 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33fd9447a79346127714a5e5e73977611eba483c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829468"
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>使用针对基于时间的智能 DNS 响应的 DNS 策略

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题，了解如何通过使用基于一天的时间的 DNS 策略将应用程序流量分配到不同的地理位置分散的应用程序实例。  
  
此方案是在你想要将流量定向到备用的应用程序服务器，如 Web 服务器，并且位于其他时区中的一个时区的情况下很有用。 这使您流量进行负载平衡应用程序实例在高峰时间的段时主服务器超载的流量。   
  
### <a name="bkmk_example1"></a>基于一天的时间智能 DNS 响应的示例  
下面是举例说明如何使用 DNS 策略应用到应用程序传入流量进行均衡基于一天的时间。  
  
此示例使用一个虚构公司 Contoso 礼品服务，从而通过其网站在全球范围内提供联机赠与解决方案，contosogiftservices.com。   
  
在两个数据中心，西雅图 （北美以外） 中的一个，Dublin （欧洲） 中的另一个托管 contosogiftservices.com Web 站点。 DNS 服务器配置为发送注意响应可使用 DNS 策略的地理位置。 使用最新业务激增，contosogiftservices.com 数值还高的访问者的每一天和某些客户报告的服务可用性问题。  
  
Contoso 礼品服务执行的站点分析，并发现每天下午 6 点和晚上 9 点本地时间之间存在到 Web 服务器流量是剧增。 Web 服务器不能缩放以处理增加的流量在这些高峰时间，从而导致拒绝服务的客户。 在这两个在欧洲和美国数据中心中进行相同的高峰小时流量重载。 在一天中的其他情况下，服务器处理远低于其最大能力的通信量。  
  
若要确保 contosogiftservices.com 客户网站上获得一种响应体验，Contoso 礼品服务想要将一些 Dublin 流量重定向到中 Dublin; 下午 6 点和晚上 9 点之间的西雅图应用程序服务器并且他们想要将一些西雅图流量重定向到下午 6 点和在西雅图晚上 9 点之间的 Dublin 应用程序服务器。  
  
下图描绘了此方案。  
  
![一天 DNS 策略示例中的时间](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="bkmk_works1"></a>基于时间的一天的工作原理的智能 DNS 响应  
  
当 DNS 服务器配置与一天 DNS 策略，下午 6 点和每个地理位置，在晚上 9 点之间的时间的 DNS 服务器执行以下操作。  
  
- 回答收到的本地数据中心中的 Web 服务器的 IP 地址的前四个查询。  
- 回答其接收的远程数据中心中的 Web 服务器的 IP 地址的第五个查询。   
  
此基于策略的行为将转移到远程 Web 服务器，从而简化本地应用程序服务器上的压力并提高站点性能的客户的本地 Web 服务器的流量负载的 20%。  
  
在非高峰时间的 DNS 服务器执行正常的地理位置基于流量管理。 此外，将查询发送北美或欧洲地区，以外的位置的 DNS 客户端的 DNS 服务器负载均衡 Seattle 和 Dublin 数据中心分配流量。  
  
当在 DNS 中配置多个 DNS 策略时，它们是一组有序的规则，以及处理这些请求通过 DNS 从最高优先级到最低优先级。 DNS 使用第一个匹配的情况下，其中包括一天的时间的策略。 出于此原因，更具体的策略应具有较高的优先级。 如果创建时间的一天策略，并为它们提供高优先级的策略列表中，DNS 将处理并首次使用这些策略，如果它们匹配的 DNS 客户端查询和策略中定义的条件的参数。 如果它们不匹配，DNS 向下移动的策略列表中进行处理的默认策略，直到它找到的匹配项。  
  
有关策略类型和条件的详细信息，请参阅[DNS 策略概述](../../dns/deploy/DNS-Policies-Overview.md)。  
  
### <a name="bkmk_how1"></a>如何为基于时间的智能 DNS 响应中配置 DNS 策略  
若要配置基于时间的一天的应用程序负载平衡的查询响应的 DNS 策略，必须执行以下步骤。  
  
- [创建 DNS 客户端子网](#bkmk_subnets)  
- [创建区域作用域](#bkmk_zscopes)  
- [将记录添加到区域作用域](#bkmk_records)  
- [创建 DNS 策略](#bkmk_policies)  
  
>[!NOTE]
>你想要配置的区域具有权威的 DNS 服务器上，必须执行这些步骤。 中的成员身份**DnsAdmins**，或等效身份所需执行以下过程。  
  
以下部分提供详细的配置说明。  
  
>[!IMPORTANT]
>以下部分包含示例 Windows PowerShell 命令包含多个参数的示例值。 请确保将这些命令中的示例值替换之前运行这些命令适用于你的部署的值为。  
  
#### <a name="bkmk_subnets"></a>创建 DNS 客户端子网  
第一步是确定的子网或 IP 地址空间为你想要将流量重定向的区域。 例如，如果你想要将流量重定向针对美国和欧洲，需要确定的子网或这些区域的 IP 地址空间。  
  
可以从地理 IP 映射获取此信息。 根据这些地理 IP 发行版，必须创建的"DNS 客户端子网。" DNS 客户端子网是从中查询发送到 DNS 服务器的 IPv4 或 IPv6 子网的逻辑分组。  
  
可以使用以下 Windows PowerShell 命令创建 DNS 客户端子网。  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
有关详细信息，请参阅[添加 DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
#### <a name="bkmk_zscopes"></a>创建区域作用域  
配置客户端子网后，必须分区区域你想要重定向到两个不同的区域作用域，其流量的每个已配置了 DNS 客户端子网一个作用域。  
  
例如，如果你想要将 DNS 名称 www.contosogiftservices.com 的流量重定向，你必须创建两个不同的区域作用域中 contosogiftservices.com 区域、 美国和欧洲。  
  
区域作用域是该区域的唯一实例。 DNS 区域可以有多个区域作用域，包含其自己的 DNS 记录集的每个区域作用域。 同一条记录可出现在多个作用域，使用不同的 IP 地址或相同的 IP 地址。  
  
>[!NOTE]
>默认情况下，区域作用域的 DNS 区域上存在。 此区域作用域为该区域具有相同的名称和旧的 DNS 操作适用于此作用域。  
  
可以使用以下 Windows PowerShell 命令以创建区域作用域。  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
有关详细信息，请参阅[添加 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
#### <a name="bkmk_records"></a>将记录添加到区域作用域  
现在，必须添加到了两个区域范围表示 web 服务器主机的记录。  
  
例如，在**SeattleZoneScope**，记录**www.contosogiftservices.com**添加 IP 地址 192.0.0.1，位于西雅图数据中心。 同样，在**DublinZoneScope**，记录**www.contosogiftservices.com**都柏林的数据中心中的 IP 地址 141.1.0.3 添加  
  
可以使用以下 Windows PowerShell 命令将记录添加到区域作用域。  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
默认作用域中添加一条记录时，则不包含区域范围区域参数。 这是与将记录添加到标准 DNS 区域相同。  
  
有关详细信息，请参阅[添加 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。  
  
#### <a name="bkmk_policies"></a>创建 DNS 策略  
创建子网后，分区 （区域作用域），并且您已将记录添加，则必须创建连接的子网和分区的策略，因此，如果查询来自 DNS 客户端子网之一中的源，从返回的查询响应区域的正确作用域。 没有策略所需的映射的默认区域作用域。  
  
配置这些 DNS 策略后，DNS 服务器行为如下所示：  
  
1. 欧洲的 DNS 客户端收到其 DNS 查询响应中的 Dublin 数据中心中的 Web 服务器的 IP 地址。  
2. 美国 DNS 客户端收到其 DNS 查询响应中的西雅图数据中心中的 Web 服务器的 IP 地址。  
3. 下午 6 点，在都柏林晚上 9 点，20%的来自欧洲客户端的查询在其 DNS 查询响应中的西雅图数据中心中接收 Web 服务器的 IP 的地址。  
4. 下午 6 点，在西雅图晚上 9 点，20%的美国的客户端从查询中其 DNS 查询响应中的 Dublin 数据中心接收 Web 服务器的 IP 的地址。  
5. 从其他国家或地区的查询的下半部分接收西雅图数据中心的 IP 地址和另一半接收都柏林的数据中心的 IP 地址。  
  
  
可以使用以下 Windows PowerShell 命令创建 DNS 客户端子网链接和区域作用域的 DNS 策略。  
  
>[!NOTE]
>在此示例中，DNS 服务器是 GMT 时区，因此在高峰时间必须以等效的 GMT 时间表示时间段。  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
现在已具有所需的 DNS 策略基于地理位置和当天的时间的流量重定向配置 DNS 服务器。  
  
当 DNS 服务器收到名称解析查询时，DNS 服务器的计算结果中针对配置的 DNS 策略对 DNS 请求的字段。 如果名称解析请求中的源 IP 地址与匹配任何策略，关联的区域作用域用于响应查询，并将用户定向到地理上接近它们的资源。  
  
您可以管理要求，根据你的流量中创建数千个 DNS 策略且无需重新启动 DNS 服务器-在传入的查询上动态-应用所有新策略。


