---
title: 基于时间的 DNS 响应和 Azure 云应用服务器
description: 本主题是 Windows Server 2016 DNS 策略方案指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4307ce1512980277af819e0710e0447d8dbac8c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406192"
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>基于时间的 DNS 响应和 Azure 云应用服务器

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题来了解如何使用基于当天的时间的 DNS 策略在应用程序的不同地理位置分发应用程序流量。 

如果希望将一个时区中的流量定向到其他应用程序服务器（例如在位于其他时区的 Microsoft Azure 上托管的 Web 服务器），则此方案非常有用。 这使你可以在高峰时间段内通过流量对主服务器进行负载平衡，在高峰时段内对流量进行负载均衡。 

> [!NOTE]
> 若要了解如何在不使用 Azure 的情况下使用 DNS 策略来执行智能 DNS 响应，请参阅[根据一天中的时间将 Dns 策略用于智能 Dns 响应](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md)。 

## <a name="example-of-intelligent-dns-responses-based-on-the-time-of-day-with-azure-cloud-app-server"></a>基于 Azure 云应用服务器当天的时间的智能 DNS 响应示例

下面的示例演示了如何使用 DNS 策略根据一天中的时间来平衡应用程序流量。

此示例使用了一家虚构的公司 Contoso 礼品服务，该公司通过其网站 contosogiftservices.com 提供了全球范围内的在线赠与政策解决方案。 

Contosogiftservices.com 网站仅托管在西雅图的单个本地数据中心（使用公共 IP 192.68.30.2）。 

DNS 服务器也位于本地数据中心。 

使用最新的业务冲击，contosogiftservices.com 每天有更多的访问者，一些客户报告了服务可用性问题。 

Contoso 礼券会执行站点分析，发现每个晚上晚上6点到晚上9点，西雅图 Web 服务器的流量会出现冲击。 在这些高峰时段，Web 服务器无法缩放以处理增加的流量，导致拒绝向客户发送服务。 

为了确保 contosogiftservices.com 客户获得网站的响应体验，Contoso 礼品服务决定在这些时间段内，会租借 Microsoft Azure 上的虚拟机 \(VM @ no__t，以便托管其 Web 服务器的副本。  

Contoso 礼券从 Azure 获取 VM 的公共 IP 地址（192.68.31.44），并在每天下午5-10，开发自动化，以便每天在 Azure 上的 Azure 上部署 Web 服务器，从而允许一小时的紧急情况。

> [!NOTE]
> 有关 Azure Vm 的详细信息，请参阅[虚拟机文档](https://azure.microsoft.com/documentation/services/virtual-machines/) 

DNS 服务器配置了区域作用域和 DNS 策略，以便每天 5-9 PM，将 30% 的查询发送到运行在 Azure 中的 Web 服务器的实例。

下图描述了此方案。

![当天时间响应的 DNS 策略](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)  

## <a name="how-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server-works"></a>如何根据一天中的时间 Azure 应用服务器的智能 DNS 响应工作
 
本文演示如何将 DNS 服务器配置为使用两个不同的应用程序服务器 IP 地址应答 DNS 查询-一台 web 服务器在西雅图，另一台在 Azure 数据中心内。

在配置新的 DNS 策略后，如果该策略基于西雅图的高峰期6：30，则 DNS 服务器会向包含西雅图 Web 服务器的 IP 地址的客户端发送70（每秒发送的 DNS 响应），以及对客户端的 DNS 响应的30美元。nts 包含 Azure Web 服务器的 IP 地址，从而将客户端流量定向到新的 Azure Web 服务器，并阻止西雅图 Web 服务器过载。 

在一天的所有其他时间，将进行正常的查询处理，并从包含本地数据中心内 web 服务器记录的默认区域作用域发送响应。 

Azure 记录上的 TTL 10 分钟可确保从 Azure 删除 VM 之前，LDNS 缓存中的记录已过期。 此类扩展的优点之一是，你可以将你的 DNS 数据保存在本地，并根据需要随时扩展到 Azure。

## <a name="how-to-configure-dns-policy-for-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server"></a>如何根据一天中的 Azure 应用服务器的时间为智能 DNS 响应配置 DNS 策略

若要为基于应用程序负载平衡的查询响应配置 DNS 策略，必须执行以下步骤。

- [创建区域作用域](#create-the-zone-scopes)
- [将记录添加到区域作用域](#add-records-to-the-zone-scopes)
- [创建 DNS 策略](#create-the-dns-policies)

> [!NOTE]
> 您必须在对要配置的区域具有权威的 DNS 服务器上执行这些步骤。 若要执行以下过程，需要 DnsAdmins 中的成员身份或同等身份。 

以下各节提供了详细的配置说明。

> [!IMPORTANT]
> 以下各节包含示例 Windows PowerShell 命令，其中包含许多参数的示例值。 在运行这些命令之前，请确保将这些命令中的示例值替换为适用于你的部署的值。 


### <a name="create-the-zone-scopes"></a>创建区域作用域

区域作用域是区域的唯一实例。 DNS 区域可以有多个区域作用域，每个区域作用域包含其自己的一组 DNS 记录。 同一条记录可以出现在多个作用域中，具有不同的 IP 地址或相同的 IP 地址。 

> [!NOTE]
> 默认情况下，在 DNS 区域中存在区域作用域。 此区域作用域具有与区域相同的名称，并且旧式 DNS 操作在此作用域上起作用。 

你可以使用以下示例命令创建用于托管 Azure 记录的区域作用域。

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

有关详细信息，请参阅[DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="add-records-to-the-zone-scopes"></a>将记录添加到区域作用域
下一步是将代表 Web 服务器主机的记录添加到区域作用域中。 

在 AzureZoneScope 中，将在 Azure 公有云中添加记录 www.contosogiftservices.com 和 IP 地址192.68.31.44。 

同样，在默认区域作用域 \(contosogiftservices @ no__t-1 中，将使用在西雅图本地数据中心内运行的 Web 服务器的 IP 地址 no__t 添加记录 \(www @ 192.68.30.2。

在下面的第二个 cmdlet 中，不包括– ZoneScope 参数。 因此，会将记录添加到默认 ZoneScope。 

此外，Azure Vm 的记录 TTL 将保留在600s （10分钟），以便 LDNS 不会将其缓存较长的时间，这会干扰负载平衡。 此外，Azure Vm 还提供了1个额外的时间作为应急措施，以确保甚至包含缓存记录的客户端都能够解决。

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

有关详细信息，请参阅[DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。  

### <a name="create-the-dns-policies"></a>创建 DNS 策略 
创建区域作用域后，可以创建在这些范围内分发传入查询的 DNS 策略，以便发生以下情况。

1. 每日下午6：30，客户端在 Azure 数据中心收到 DNS 响应中 Web 服务器的 IP 地址，而 70% 的客户端接收西雅图本地 Web 服务器的 IP 地址。
2. 在所有其他情况下，所有客户端都接收西雅图本地 Web 服务器的 IP 地址。

当天的时间必须以 DNS 服务器的本地时间表示。

你可以使用以下示例命令创建 DNS 策略。

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

有关详细信息，请参阅[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
现在 DNS 服务器配置了所需的 DNS 策略，以根据一天的时间将流量重定向到 Azure Web 服务器。 

请注意表达式：

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” 
`

此表达式将 DNS 服务器配置为 ZoneScope 和权重组合，该组合指示 DNS 服务器在每个时间段的时间发送西雅图 Web server 70 的 IP 地址，同时每秒发送 Azure Web 服务器的 IP 地址。

你可以根据流量管理要求创建数千个 DNS 策略，并动态应用所有新策略，而无需重新启动 DNS 服务器的传入查询。
