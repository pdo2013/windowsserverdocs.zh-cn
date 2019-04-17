---
title: 使用基于当天的时间的智能 DNS 响应 DNS 策略
description: 本主题介绍 DNS 策略方案指南为 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 46821daff4a0046bf78d7f56dc7c5deabcc437e4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>使用基于当天的时间的智能 DNS 响应 DNS 策略

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解如何使用的基于当天的时间 DNS 策略应用程序交通分散不同的应用程序的地理位置分布式实例。  
  
此项 scenario 非常有用的情况下在你想要一个时区备用应用程序服务器，如 Web 服务器，位于其他时区的区域中直接通信。 这允许你以高峰跨应用程序实例加载余额通信时间的段交通时过你的主服务器。   
  
### <a name="bkmk_example1"></a>将基于当天的时间的智能 DNS 响应它的示例  
以下是一种方式，你可以使用 DNS 策略于余额应用程序流量基于当天的时间。  
  
此示例虚构公司，提供联机赠送解决方案通过他们的网站 contosogiftservices.com 全球范围内的 Contoso 礼品服务。   
  
在两个数据中心、在西雅图（美国）的一和 Dublin（欧洲）中的另一个托管 contosogiftservices.com 网站。 发送地理位置感知响应使用 DNS 策略配置 DNS 服务器。 在企业中最近电涌，contosogiftservices.com 有更多的访客每天，并且某些客户报告服务可用性问题。  
  
Contoso 礼品服务执行站点分析，，将会发现之间 6 下午与 9 本地时间，每个晚上电涌中的 Web 服务器的交通。 Web 服务器无法扩展以在高峰时段，处理增加的交通向客户导致拒绝服务。 相同的山峰小时交通重载发生欧洲并美国中心。 在一天的其他时间，服务器处理远他们最大的功能有多个。  
  
若要确保 contosogiftservices.com 客户能够从网站响应体验，Contoso 礼品服务想要将某些 Dublin 交通重定向到西雅图应用程序中的服务器 6 点之间的晚上 9 Dublin;并且，他们想要将某些西雅图交通重定向到 Dublin 应用程序服务器，在西雅图晚上 9 6 点之间。  
  
下图显示了这种情况。  
  
![一天 DNS 策略示例的时间](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="bkmk_works1"></a>如何智能 DNS 响应基于时间的一天的工作原理  
  
使用时间的一天 DNS 策略，在每个地理位置 6 下午与 9 之间配置 DNS 服务器时将执行以下 DNS 服务器。  
  
- 解答前四个查询收到本地数据中心中的 Web 服务器的 IP 地址，它。  
- 回答五个查询收到远程数据中心中的 Web 服务器的 IP 地址，它。   
  
此策略基于行为减轻二十 %到远程的 Web 服务器，减轻压力本地应用服务器改进站点的性能的客户的本地的 Web 服务器交通加载。  
  
高峰、DNS 服务器执行正常地理-位置基于交通管理。 此外，从北美或欧洲、之外的地点发送查询的 DNS 客户端 DNS 服务器加载跨西雅图和 Dublin 数据中心余额交通。  
  
当多个 DNS 策略配置 DNS 中时，它们是一组有序的规则，并且他们进行处理 dns 高优先级从到最低的优先级。 DNS 使用的第一个策略相匹配的情况下，包括当天的时间。 出于此原因，更多具体策略应具有较高的优先级。 如果你创建的一天策略的时间，并使它们高优先级策略的列表中，DNS 处理，并首次使用这些策略，如果他们匹配的策略中定义的条件以及 DNS 客户端查询参数。 如果它们不匹配，DNS 向下移动策略的列表进行处理默认策略，直至找到匹配。  
  
有关策略类型和条件的详细信息，请参阅[DNS 策略概述](../../dns/deploy/DNS-Policies-Overview.md)。  
  
### <a name="bkmk_how1"></a>如何为智能 DNS 响应将基于当天的时间配置 DNS 策略  
若要配置基于一天应用程序负载平衡时间查询响应 DNS 策略，你必须执行以下步骤。  
  
- [创建 DNS 客户端子网](#bkmk_subnets)  
- [创建区域范围](#bkmk_zscopes)  
- [添加到区域范围记录](#bkmk_records)  
- [创建 DNS 策略](#bkmk_policies)  
  
>[!NOTE]
>你必须在你想要配置区域的授权 DNS 服务器执行这些步骤。 在会员**DnsAdmins**，或等效，需要执行以下步骤。  
  
以下部分提供了详细的配置说明进行操作。  
  
>[!IMPORTANT]
>以下部分包括包含许多参数示例值的示例 Windows PowerShell 命令。 确保你在运行以下命令之前就提供适用于你的部署的值与替换示例值在这些命令。  
  
#### <a name="bkmk_subnets"></a>创建 DNS 客户端子网  
第一步是找出子网或 IP 地址的地区，你希望将通信重定向的空间。 例如，如果你想要将通信美国和欧洲重定向，你需要确定子网或 IP 地址空间的这些区域。  
  
你可以从地理 IP 地图获取此信息。 基于这些地理 IP 分配，你必须创建"DNS 客户端个子网。" DNS 客户端子网是 IPv4 或 IPv6 个子网从中查询发送到 DNS 服务器的逻辑分组。  
  
以下 Windows PowerShell 命令用于创建 DNS 客户端个子网。  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
有关详细信息，请参阅[添加 DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
#### <a name="bkmk_zscopes"></a>创建区域范围  
将配置客户端子网后，你必须分区区域你想要将重定向到了两种不同的区域范围，其流量一个范围有配置 DNS 客户端个子网。  
  
例如，如果你想要将重定向 DNS 名称 www.contosogiftservices.com 的通信，您必须在 contosogiftservices.com 区域、美国和欧洲创建两个不同的区域范围。  
  
区域范围是唯一区域的实例。 DNS 区域有多个区域范围包含自己套 DNS 记录每个区域范围。 同一记录可能存在多个范围，使用不同的 IP 地址或同一 IP 地址。  
  
>[!NOTE]
>默认情况下，区域范围存在上 DNS 区域。 此区域范围作为区域，具有相同的名称，并传统 DNS 运营处理此范围。  
  
可以使用下面的 Windows PowerShell 命令来创建区域范围。  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
有关详细信息，请参阅[添加 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
#### <a name="bkmk_records"></a>添加到区域范围记录  
现在，你必须添加到了两个区域范围表示 web 服务器主机的记录。  
  
例如，在**SeattleZoneScope**，记录**www.contosogiftservices.com**使用 IP 地址 192.0.0.1，在西雅图 datacenter 位于添加。 同样，在**DublinZoneScope**，记录**www.contosogiftservices.com**使用 Dublin 数据中心中的 IP 地址 141.1.0.3 添加  
  
你可以使用 Windows PowerShell 以下命令添加到区域范围记录。  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
记录添加默认范围内时，不包括该区域范围区域参数。 这就是向标准 DNS 区域中添加记录相同。  
  
有关详细信息，请参阅[添加 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。  
  
#### <a name="bkmk_policies"></a>创建 DNS 策略  
创建子网后，分区（区域范围），并且你已添加的记录，你必须创建子网和分区中，连接的策略，以便当查询来源之一 DNS 客户端子网，查询响应从正确的范围内的区域。 没有策略所需的映射默认区域范围。  
  
配置这些 DNS 策略 DNS 服务器行为后，如下所示：  
  
1. 欧洲 DNS 客户端接收在其 DNS 查询响应 Dublin 数据中心中的 Web 服务器的 IP 地址。  
2. 美国 DNS 客户端接收在其 DNS 查询响应西雅图数据中心中的 Web 服务器的 IP 地址。  
3. 6 下午与之间 9 Dublin 中，从欧洲的客户端查询 20%在其 DNS 查询响应西雅图数据中心中收到 Web 服务器的 IP 地址。  
4. 之间 6 点，在西雅图晚上 9 来自美国的客户端查询 20%收到在其 DNS 查询响应 Dublin 数据中心中的 Web 服务器的 IP 地址。  
5. 一半从在全世界其他人查询收到西雅图 datacenter 的 IP 地址，并且另一半接收 Dublin datacenter 的 IP 地址。  
  
  
以下 Windows PowerShell 命令可用于创建 DNS 策略，该链接 DNS 客户端子网和区域范围。  
  
>[!NOTE]
>在此示例中，DNS 服务器是中 GMT 时区的区域，以便峰值小时时间段必须表达等效 GMT 时间。  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
现在重定向通信根据地理位置并一天时间才能 DNS 策略配置 DNS 服务器。  
  
当 DNS 服务器收到名称分辨率查询时，DNS 服务器评估配置 DNS 策略防御 DNS 请求中的字段。 名称分辨率请求中的源 IP 地址与匹配任何策略，如果关联的区域范围用于响应查询，并且用户定向到地理位置与他们最近的资源。  
  
你可以管理要求根据您的通信中创建数千 DNS 策略，并且无需重新启动 DNS 服务器，在传入查询动态-应用的所有新策略。


