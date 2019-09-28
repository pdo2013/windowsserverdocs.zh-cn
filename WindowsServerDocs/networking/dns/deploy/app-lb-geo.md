---
title: 使用 DNS 策略通过地理位置感知执行应用程序负载平衡
description: 本主题是 Windows Server 2016 DNS 策略方案指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ea3f959612de0f2bc56a887ba73aba47f1d3f141
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406218"
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>使用 DNS 策略通过地理位置感知执行应用程序负载平衡

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用本主题来了解如何配置 DNS 策略，以便对应用程序进行负载平衡，并使用地理位置识别。

本指南中的上一个主题[使用 DNS 策略来实现应用程序负载平衡](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb)，其中使用了虚构的公司-Contoso 礼券的示例，该服务提供联机赠与政策服务，并具有名为 contosogiftservices.com 的网站。 Contoso 礼券负载会在位于西雅图、WA、芝加哥、IL 和达拉斯，德克萨斯州的北美数据中心内的服务器之间对其联机 Web 应用程序进行平衡。

>[!NOTE]
>建议在执行此方案中的说明之前，先熟悉主题 "[将 DNS 策略用于应用程序负载均衡](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb)"。

本主题使用同一虚构公司和网络基础结构作为新的示例部署的基础，其中包括地理位置感知。

在此示例中，Contoso 礼品服务成功地在全球范围内扩展其状态。

与北美类似，公司现在已有在欧洲数据中心托管的 web 服务器。

Contoso 礼券 DNS 管理员希望采用与美国中的 DNS 策略实现类似的方式为欧洲数据中心配置应用程序负载平衡，以及在都柏林、爱尔兰、阿姆斯特丹、Holland 和其他地方。

DNS 管理员还希望世界上其他位置的所有查询在其所有数据中心之间平均分布。

在接下来的部分中，你将了解如何实现与你自己的网络上的 Contoso DNS 管理员相同的目标。

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>如何配置针对地理位置感知的应用程序负载平衡

以下部分说明了如何为应用程序负载平衡配置 DNS 策略，并提供地理位置感知功能。

>[!IMPORTANT]
>以下各节包含示例 Windows PowerShell 命令，其中包含许多参数的示例值。 在运行这些命令之前，请确保将这些命令中的示例值替换为适用于你的部署的值。

### <a name="bkmk_clientsubnets"></a>创建 DNS 客户端子网

必须首先识别北美和欧洲区域的子网或 IP 地址空间。

你可以从地理 IP 映射获取此信息。 根据这些地域 IP 分发，必须创建 DNS 客户端子网。

DNS 客户端子网是将查询发送到 DNS 服务器的 IPv4 或 IPv6 子网的逻辑分组。

你可以使用以下 Windows PowerShell 命令来创建 DNS 客户端子网。 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
有关详细信息，请参阅[DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。

### <a name="bkmk_zscopes2"></a>创建区域作用域

准备好客户端子网之后，必须将区域 contosogiftservices.com 分区到不同的区域作用域中，每个区域都适用于数据中心。

区域作用域是区域的唯一实例。 DNS 区域可以有多个区域作用域，每个区域作用域包含其自己的一组 DNS 记录。 同一条记录可以出现在多个作用域中，具有不同的 IP 地址或相同的 IP 地址。

>[!NOTE]
>默认情况下，在 DNS 区域中存在区域作用域。 此区域作用域具有与区域相同的名称，并且旧式 DNS 操作在此作用域上起作用。

应用程序负载平衡的前一方案演示了如何在北美中为数据中心配置三个区域作用域。

在下面的命令中，可以创建两个以上的区域作用域，每个区域用于都柏林和阿姆斯特丹数据中心。 

可以添加这些区域作用域，而无需对同一区域中的三个现有北美区域作用域进行任何更改。 此外，在创建这些区域作用域后，无需重新启动 DNS 服务器。

你可以使用以下 Windows PowerShell 命令创建区域作用域。

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

有关详细信息，请参阅[DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records2"></a>将记录添加到区域作用域

现在，必须将表示 web 服务器主机的记录添加到区域作用域中。

在前面的方案中添加了美国数据中心的记录。 你可以使用以下 Windows PowerShell 命令将记录添加到欧洲数据中心的区域作用域。
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

有关详细信息，请参阅[DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

### <a name="bkmk_policies2"></a>创建 DNS 策略

创建分区（区域作用域）并添加记录后，必须创建在这些范围内分发传入查询的 DNS 策略。

在此示例中，跨不同数据中心内的应用程序服务器的查询分布满足以下条件。

1. 从北美客户端子网上的源接收 DNS 查询时，50% 的 DNS 响应点指向西雅图数据中心，25% 的响应指向芝加哥数据中心，其余 25% 的响应指向达拉斯 datacenter。
2. 从欧洲客户端子网中的源接收 DNS 查询时，50% 的 DNS 响应点将指向都柏林数据中心，50% 的 DNS 响应点将指向阿姆斯特丹数据中心。
3. 如果查询来自世界各地的任何其他地方，则 DNS 响应分布在所有五个数据中心。

你可以使用以下 Windows PowerShell 命令来实现这些 DNS 策略。

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

有关详细信息，请参阅[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。

现在，你已成功创建了一个 DNS 策略，该策略可跨多个洲的五个不同数据中心内的 Web 服务器提供应用程序负载平衡。

你可以根据流量管理要求创建数千个 DNS 策略，并动态应用所有新策略，而无需重新启动 DNS 服务器的传入查询。
