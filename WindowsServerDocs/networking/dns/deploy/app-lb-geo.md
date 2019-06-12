---
title: 使用 DNS 策略通过地理位置感知执行应用程序负载平衡
description: 本主题是 DNS 策略方案指南 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9f76163e6b064ac3225ab4d755afd548e1cb720b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446410"
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>使用 DNS 策略通过地理位置感知执行应用程序负载平衡

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题以了解如何配置 DNS 策略通过地理位置感知负载平衡应用程序。

本指南中的上一个主题[使用 DNS 策略的应用程序负载均衡](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb)、 使用举例说明提供了联机赠与一家虚构公司 Contoso 礼品服务的服务，并且具有一个名为网站contosogiftservices.com。 Contoso 礼品 Services 负载平衡其位于西雅图，华盛顿州、 伊利诺伊州和德克萨斯州达拉斯市的北美数据中心中的服务器之间的联机 Web 应用程序。

>[!NOTE]
>建议熟悉本主题[使用 DNS 策略的应用程序负载均衡](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb)才能执行此方案中的说明进行操作。

本主题为基础使用的同一个虚构的公司和网络基础结构有关新的示例部署，包括地理位置感知。

在此示例中，Contoso 礼品服务正在成功地在全球范围内扩展它们的存在。

类似于北美，该公司现在具有在欧洲数据中心中托管的 web 服务器。

Contoso 礼品服务的 DNS 管理员想要配置应用程序中的负载平衡的欧洲数据中心类似于在美国的 DNS 策略实现的方式与应用程序流量都位于 Web 服务器之间分配爱尔兰的都柏林、 阿姆斯特丹、 荷兰、 和其他位置。

DNS 管理员也想在所有其数据中心之间平均分布在世界其他位置中的所有查询。

在接下来的部分可以了解如何实现类似的 Contoso DNS 管理员自己的网络上的目标。

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>如何配置应用程序负载平衡和地理位置感知

以下各节演示如何配置应用程序负载平衡和地理位置感知的 DNS 策略。

>[!IMPORTANT]
>以下部分包含示例 Windows PowerShell 命令包含多个参数的示例值。 请确保将这些命令中的示例值替换之前运行这些命令适用于你的部署的值为。

### <a name="bkmk_clientsubnets"></a>创建 DNS 客户端子网

首先必须确定子网或 IP 地址空间的北美和欧洲区域。

可以从地理 IP 映射获取此信息。 根据这些地理 IP 发行版，必须创建 DNS 客户端子网。

DNS 客户端子网是从中查询发送到 DNS 服务器的 IPv4 或 IPv6 子网的逻辑分组。

可以使用以下 Windows PowerShell 命令创建 DNS 客户端子网。 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
有关详细信息，请参阅[添加 DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。

### <a name="bkmk_zscopes2"></a>创建区域作用域

客户端子网就位后，必须将区域 contosogiftservices.com 划分为不同的区域作用域，每个数据中心。

区域作用域是该区域的唯一实例。 DNS 区域可以有多个区域作用域，包含其自己的 DNS 记录集的每个区域作用域。 同一条记录可出现在多个作用域，使用不同的 IP 地址或相同的 IP 地址。

>[!NOTE]
>默认情况下，区域作用域的 DNS 区域上存在。 此区域作用域为该区域具有相同的名称和旧的 DNS 操作适用于此作用域。

应用程序负载平衡前面的方案演示如何在北美配置的数据中心的三个区域作用域。

使用以下命令中，您可以创建两个更多区域作用域，每个用于在 Dublin 和阿姆斯特丹数据中心。 

可以将无需更改任何这些区域作用域添加到三个现有北美区域中的作用域相同的区域。 此外，在创建这些区域作用域后，你不必重新启动 DNS 服务器。

可以使用以下 Windows PowerShell 命令以创建区域作用域。

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

有关详细信息，请参阅[添加 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records2"></a>将记录添加到区域作用域

现在，必须添加到区域作用域表示 web 服务器主机的记录。

在前面的方案中添加了美国数据中心的记录。 可以使用以下 Windows PowerShell 命令将记录添加到欧洲数据中心的区域作用域。
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

有关详细信息，请参阅[添加 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

### <a name="bkmk_policies2"></a>创建 DNS 策略

已创建的分区 （区域作用域），并且已将记录添加后，您必须创建跨这些作用域中分发传入的查询的 DNS 策略。

对于本例，请查询分布在不同数据中心中的应用程序服务器满足以下条件。

1. 收到 DNS 查询后的源中的北美的客户端子网，50%的 DNS 响应指向西雅图数据中心，25%的响应指向芝加哥数据中心，剩下的 25%的响应指向达拉斯数据中心。
2. 从欧洲客户端子网中的源收到的 DNS 查询时，50%的 DNS 响应指向都柏林的数据中心，并对 DNS 响应的 50%指向阿姆斯特丹数据中心。
3. 如果查询来自的其他任何位置的世界中，DNS 响应分布在所有五个数据中心。

可以使用以下 Windows PowerShell 命令来实现这些 DNS 策略。

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。

现在已成功创建位于多个洲的五个不同数据中心内的 Web 服务器之间提供应用程序负载平衡的 DNS 策略。

您可以管理要求，根据你的流量中创建数千个 DNS 策略且无需重新启动 DNS 服务器-在传入的查询上动态-应用所有新策略。
