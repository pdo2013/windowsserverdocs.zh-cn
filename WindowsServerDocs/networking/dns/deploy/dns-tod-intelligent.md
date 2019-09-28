---
title: 使用针对基于时间的智能 DNS 响应的 DNS 策略
description: 本主题是 Windows Server 2016 DNS 策略方案指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e497b0d73c816f0295588aa77a21c49d376c0dcf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406187"
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>使用针对基于时间的智能 DNS 响应的 DNS 策略

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题来了解如何使用基于当天的时间的 DNS 策略在应用程序的不同地理位置分发应用程序流量。  
  
如果希望将一个时区中的流量定向到位于另一个时区的备用应用程序服务器（如 Web 服务器），则此方案非常有用。 这使你可以在高峰时间段内通过流量对主服务器进行负载平衡，在高峰时段内对流量进行负载均衡。   
  
### <a name="bkmk_example1"></a>基于一天中的时间的智能 DNS 响应示例  
下面的示例演示了如何使用 DNS 策略根据一天中的时间来平衡应用程序流量。  
  
此示例使用了一家虚构的公司 Contoso 礼品服务，该公司通过其网站 contosogiftservices.com 提供了全球范围内的在线赠与政策解决方案。   
  
Contosogiftservices.com 网站托管在两个数据中心中，一个位于西雅图（北美），另一个位于都柏林（欧洲）。 DNS 服务器配置为使用 DNS 策略发送地理位置感知响应。 使用最新的业务冲击，contosogiftservices.com 每天有更多的访问者，一些客户报告了服务可用性问题。  
  
Contoso 礼券执行站点分析，发现每个晚上晚上6点到晚上9点的本地时间，Web 服务器的流量会出现冲击。 Web 服务器无法缩放以在这些高峰时段处理增加的流量，导致拒绝向客户发送服务。 欧洲和美国数据中心发生相同的高峰工时流量过载。 在一天的其他时间，这些服务器处理的流量量要低于其最大容量。  
  
为了确保 contosogiftservices.com 客户获得网站的响应体验，Contoso 礼品服务希望将一些都柏林流量重定向到北京的西雅图应用程序服务器，然后再到都柏林的晚上9点;他们希望在西雅图的 6 PM 和 9 PM 之间将一些西雅图流量重定向到都柏林应用程序服务器。  
  
下图描述了此方案。  
  
![当天的时间 DNS 策略示例](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="bkmk_works1"></a>基于一天时间的智能 DNS 响应的工作方式  
  
如果 DNS 服务器配置为 "一天中的时间 DNS 策略"，则在每个地理位置晚上6：晚上6：晚上9： DNS 服务器将执行以下设置。  
  
- 用本地数据中心内的 Web 服务器的 IP 地址回答其接收到的前四个查询。  
- 在远程数据中心中，用 Web 服务器的 IP 地址回答收到的第五个查询。   
  
此基于策略的行为将本地 Web 服务器的流量负载的20个降低到远程 Web 服务器，从而减轻了本地应用程序服务器的压力并改善了客户的站点性能。  
  
在非高峰时段，DNS 服务器会执行基于地理位置的正常流量管理。 此外，如果 DNS 客户端从北美或欧洲以外的位置发送查询，则 DNS 服务器会在西雅图和都柏林数据中心对流量进行负载均衡。  
  
在 DNS 中配置了多个 DNS 策略时，它们是一组有序规则，并由 DNS 从最高优先级到最低优先级进行处理。 DNS 使用与环境匹配的第一个策略，包括当天的时间。 出于此原因，更具体的策略应具有较高的优先级。 如果在策略列表中创建了时间策略，并在策略列表中给予了高优先级，则 DNS 会先处理这些策略，并在策略中定义的条件与 DNS 客户端查询的参数匹配。 如果二者不匹配，则 DNS 在策略列表中向下移动以处理默认策略，直到找到匹配项为止。  
  
有关策略类型和条件的详细信息，请参阅[DNS 策略概述](../../dns/deploy/DNS-Policies-Overview.md)。  
  
### <a name="bkmk_how1"></a>如何根据一天的时间为智能 DNS 响应配置 DNS 策略  
若要为基于应用程序负载平衡的查询响应配置 DNS 策略，必须执行以下步骤。  
  
- [创建 DNS 客户端子网](#bkmk_subnets)  
- [创建区域作用域](#bkmk_zscopes)  
- [将记录添加到区域作用域](#bkmk_records)  
- [创建 DNS 策略](#bkmk_policies)  
  
>[!NOTE]
>您必须在对要配置的区域具有权威的 DNS 服务器上执行这些步骤。 若要执行以下过程，需要**DnsAdmins**中的成员身份或同等身份。  
  
以下各节提供了详细的配置说明。  
  
>[!IMPORTANT]
>以下各节包含示例 Windows PowerShell 命令，其中包含许多参数的示例值。 在运行这些命令之前，请确保将这些命令中的示例值替换为适用于你的部署的值。  
  
#### <a name="bkmk_subnets"></a>创建 DNS 客户端子网  
第一步是确定要将流量重定向到的区域的子网或 IP 地址空间。 例如，如果要重定向美国和欧洲的流量，则需要确定这些区域的子网或 IP 地址空间。  
  
你可以从地理 IP 映射获取此信息。 根据这些地域 IP 分发，必须创建 "DNS 客户端子网"。 DNS 客户端子网是将查询发送到 DNS 服务器的 IPv4 或 IPv6 子网的逻辑分组。  
  
你可以使用以下 Windows PowerShell 命令来创建 DNS 客户端子网。  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
有关详细信息，请参阅[DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
#### <a name="bkmk_zscopes"></a>创建区域作用域  
配置了客户端子网之后，必须将要重定向到的流量分区为两个不同的区域作用域，其中每个已配置的 DNS 客户端子网的作用域。  
  
例如，如果要重定向 DNS 名称 www.contosogiftservices.com 的流量，则必须在 contosogiftservices.com 区域中创建两个不同的区域作用域，一个用于美国，一个用于欧洲。  
  
区域作用域是区域的唯一实例。 DNS 区域可以有多个区域作用域，每个区域作用域包含其自己的一组 DNS 记录。 同一条记录可以出现在多个作用域中，具有不同的 IP 地址或相同的 IP 地址。  
  
>[!NOTE]
>默认情况下，在 DNS 区域中存在区域作用域。 此区域作用域具有与区域相同的名称，并且旧式 DNS 操作在此作用域上起作用。  
  
你可以使用以下 Windows PowerShell 命令创建区域作用域。  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
有关详细信息，请参阅[DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
#### <a name="bkmk_records"></a>将记录添加到区域作用域  
现在，必须将表示 web 服务器主机的记录添加到这两个区域作用域中。  
  
例如，在**SeattleZoneScope**中，将使用 IP 地址192.0.0.1 添加记录<strong>www.contosogiftservices.com</strong> ，该地址位于西雅图数据中心。 同样，在**DublinZoneScope**中，记录<strong>Www.contosogiftservices.com</strong>添加到都柏林 datacenter 中的 IP 地址141.1.0。3  
  
你可以使用以下 Windows PowerShell 命令将记录添加到区域作用域。  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
在默认范围内添加记录时，不包含 ZoneScope 参数。 这与将记录添加到标准 DNS 区域相同。  
  
有关详细信息，请参阅[DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。  
  
#### <a name="bkmk_policies"></a>创建 DNS 策略  
创建子网、分区（区域作用域）并添加记录后，你必须创建用于连接子网和分区的策略，以便在查询来自某个 DNS 客户端子网中的源时，从区域的正确范围。 映射默认区域作用域不需要策略。  
  
配置这些 DNS 策略后，DNS 服务器行为如下所示：  
  
1. 欧洲的 DNS 客户端在其 DNS 查询响应中接收到都柏林 datacenter 中 Web 服务器的 IP 地址。  
2. 美洲 DNS 客户端在其 DNS 查询响应中接收西雅图 datacenter 中 Web 服务器的 IP 地址。  
3. 都柏林的 6 PM 和 9 PM 之间，欧洲客户的 20% 的查询将在其 DNS 查询响应中的西雅图 datacenter 内接收 Web 服务器的 IP 地址。  
4. 在西雅图的 6 PM 和 9 PM 之间，来自美国客户的 20% 的查询在其 DNS 查询响应中接收到都柏林 datacenter 中 Web 服务器的 IP 地址。  
5. 来自世界各地的一半查询会接收西雅图数据中心的 IP 地址，而另一半接收到都柏林数据中心的 IP 地址。  
  
  
你可以使用以下 Windows PowerShell 命令创建一个 DNS 策略，用于链接 DNS 客户端子网和区域作用域。  
  
>[!NOTE]
>在此示例中，DNS 服务器处于 GMT 时区，因此高峰时间段必须以等效的 GMT 时间表示。  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
有关详细信息，请参阅[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
现在 DNS 服务器配置了所需的 DNS 策略，以根据地理位置和当天的时间重定向流量。  
  
当 DNS 服务器收到名称解析查询时，DNS 服务器会根据配置的 DNS 策略评估 DNS 请求中的字段。 如果名称解析请求中的源 IP 地址与任何策略相匹配，则会使用关联的区域作用域来响应查询，并将用户定向到离它们最近的资源。  
  
你可以根据流量管理要求创建数千个 DNS 策略，并动态应用所有新策略，而无需重新启动 DNS 服务器的传入查询。


