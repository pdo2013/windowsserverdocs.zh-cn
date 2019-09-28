---
title: 使用 DNS 策略执行应用程序负载平衡
description: 本主题是 Windows Server 2016 DNS 策略方案指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 356c61c2cc5b60f43a69f17966c97f3c69d05cda
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356036"
---
# <a name="use-dns-policy-for-application-load-balancing"></a>使用 DNS 策略执行应用程序负载平衡

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题来了解如何配置 DNS 策略以执行应用程序负载平衡。

以前版本的 Windows Server DNS 仅提供使用轮循机制响应的负载平衡;但对于 Windows Server 2016 中的 DNS，你可以配置用于应用程序负载平衡的 DNS 策略。

部署了应用程序的多个实例后，可以使用 DNS 策略来平衡不同应用程序实例之间的流量负载，从而动态分配应用程序的流量负载。

## <a name="example-of-application-load-balancing"></a>应用程序负载均衡的示例

以下示例演示了如何使用 DNS 策略来实现应用程序负载平衡。

此示例使用一个虚构的公司 Contoso 礼券，该服务提供联机 gifing 服务，并且有一个名为**contosogiftservices.com**的网站。

Contosogiftservices.com 网站托管在多个数据中心中，每个数据中心都有不同的 IP 地址。

在北美（Contoso 礼券的主要市场）中，网站托管在三个数据中心：芝加哥，IL，达拉斯，TX 和西雅图，华盛顿州。

西雅图 Web 服务器具有最佳的硬件配置，并可处理两倍于其他两个站点的负载。 Contoso 礼券需要按以下方式定向应用程序流量。

- 因为西雅图 Web 服务器包括更多资源，所以，应用程序的一半会定向到此服务器
- 将应用程序客户端的四分之一定向到达拉斯，TX datacenter
- 将应用程序客户端的一个季度定向到芝加哥，IL，datacenter

下图描述了此方案。

![DNS 应用程序负载平衡与 DNS 策略](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>应用程序负载平衡的工作方式

使用此示例方案为使用 DNS 策略的 DNS 服务器配置了应用程序负载平衡后，DNS 服务器将在 50% 的时间使用西雅图 Web 服务器地址，25% 的时间为达拉斯 Web 服务器地址，25% 的时间为芝加哥 Web 服务器地址。

因此，对于每个 DNS 服务器收到的四个查询，它会响应两个针对西雅图的响应，一个用于达拉斯和芝加哥。

使用 DNS 策略的负载平衡的一个可能问题是 DNS 客户端和解析程序/LDNS 缓存 DNS 记录，这可能会干扰负载平衡，因为客户端或解析程序不会向 DNS 服务器发送查询。

你可以通过对应进行负载平衡的 DNS 记录使用 low Time @ no__t-0to @ no__t-1Live \(TTL @ no__t 值来缓解此行为的影响。

### <a name="how-to-configure-application-load-balancing"></a>如何配置应用程序负载平衡

以下部分介绍了如何配置用于应用程序负载平衡的 DNS 策略。

#### <a name="create-the-zone-scopes"></a>创建区域作用域

必须首先为托管这些区域的数据中心创建区域 contosogiftservices.com 的作用域。

区域作用域是区域的唯一实例。 DNS 区域可以有多个区域作用域，每个区域作用域包含其自己的一组 DNS 记录。 同一条记录可以出现在多个作用域中，具有不同的 IP 地址或相同的 IP 地址。

>[!NOTE]
>默认情况下，在 DNS 区域中存在区域作用域。 此区域作用域具有与区域相同的名称，并且旧式 DNS 操作在此作用域上起作用。

你可以使用以下 Windows PowerShell 命令创建区域作用域。
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

有关详细信息，请参阅[DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

#### <a name="bkmk_records"></a>将记录添加到区域作用域

现在，必须将表示 web 服务器主机的记录添加到区域作用域中。

在**SeattleZoneScope**中，你可以添加具有位于西雅图 datacenter 中的 IP 地址192.0.0.1 的记录 www.contosogiftservices.com。

在**ChicagoZoneScope**中，可以将同一记录添加到芝加哥数据中心包含 IP 地址182.0.0.1 的 contosogiftservices @ no__t-2 @no__t。

同样，在**DallasZoneScope**中，可以使用 IP 地址162.0.0.1 在芝加哥 datacenter 中添加记录 @no__t contosogiftservices @ no__t。

你可以使用以下 Windows PowerShell 命令将记录添加到区域作用域。
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

有关详细信息，请参阅[DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

#### <a name="bkmk_policies"></a>创建 DNS 策略

创建分区（区域作用域）并添加记录后，必须创建在这些范围内分发传入查询的 DNS 策略，以便使用 Web IP 地址对 contosogiftservices.com 的 50% 的查询进行响应西雅图和达拉斯数据中心之间平均分布了西雅图数据中心的服务器。

你可以使用以下 Windows PowerShell 命令创建在这三个数据中心之间平衡应用程序流量的 DNS 策略。

>[!NOTE]
>在下面的示例命令中，expression – ZoneScope "SeattleZoneScope，2;ChicagoZoneScope，1;DallasZoneScope，1 "使用包含参数组合 @no__t 0ZoneScope @ no__t，\<weight @ no__t; 的数组配置 DNS 服务器。
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

有关详细信息，请参阅[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  

现在，你已成功创建了跨三个不同数据中心中的 Web 服务器提供应用程序负载平衡的 DNS 策略。

你可以根据流量管理要求创建数千个 DNS 策略，并动态应用所有新策略，而无需重新启动 DNS 服务器的传入查询。
