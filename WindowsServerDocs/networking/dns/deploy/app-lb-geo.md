---
title: 使用应用程序使用负载平衡地理位置感知 DNS 策略
description: 本主题介绍 DNS 策略方案指南为 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7637d927c7b22b83053e7f9100b07581c11bafc0
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>使用应用程序使用负载平衡地理位置感知 DNS 策略

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解如何配置 DNS 策略使用地理位置感知加载余额应用程序。

本指南，以前主题[使用 DNS 策略应用程序负载平衡](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb)、使用它的提供联机赠送虚构公司-Contoso 礼品服务的一个示例服务，和这已网站命名 contosogiftservices.com。Contoso 礼品服务负载平衡之间服务器位于华盛顿西雅图，芝加哥，IL，，达拉斯州奥斯汀北美中心他们在线 Web 应用程序。

>[!NOTE]
>建议熟悉主题[使用 DNS 策略应用程序负载平衡](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb)再执行此方案中的说明进行操作。

本主题以此为基础使用同一虚构公司和网络的基础结构的新的部署示例，其中包括地理位置感知。

在此示例中，在全球范围内情况下，成功扩展 Contoso 礼品服务其存在。

北美类似公司现在具有托管欧洲数据中心中的 web 服务器。

Contoso 礼品服务 DNS 管理员想要配置应用程序负载平衡欧洲数据中心在美国，DNS 策略实现类似的方式，通过应用程序交通分布在爱尔兰都柏林，阿姆斯特丹、 荷兰、 中和其他地方位于的 Web 服务器。

DNS 管理员还想所有查询来自世界均匀地分布在之间所有他们的数据中心中的其他位置。

在下一步部分可学会如何实现类似的目标到这些自己网络上的 Contoso DNS 管理员。

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>如何配置应用程序使用负载平衡地理位置感知

以下各部分将向您展示如何配置应用程序使用负载平衡地理位置感知 DNS 策略。

>[!IMPORTANT]
>以下部分包括包含许多参数示例值的示例 Windows PowerShell 命令。 确保你在运行以下命令之前就提供适用于你的部署的值与替换示例值在这些命令。

###<a name="bkmk_clientsubnets"></a>创建 DNS 客户端子网

首先，你必须识别子网或 IP 地址的北美和欧洲地区的空间。

你可以从地理 IP 地图获取此信息。 基于这些地理 IP 分配，你必须创建 DNS 客户端个子网。

DNS 客户端子网是 IPv4 或 IPv6 个子网从中查询发送到 DNS 服务器的逻辑分组。

以下 Windows PowerShell 命令用于创建 DNS 客户端个子网。 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
有关详细信息，请参阅[添加 DnsServerClientSubnet](https://technet.microsoft.com/library/mt126261.aspx)。

###<a name="bkmk_zscopes2"></a>创建区域范围

客户端子网也扣入到位后，必须将区域 contosogiftservices.com 划分为不同的区域范围，每次都数据中心。

区域范围是唯一区域的实例。 DNS 区域有多个区域范围包含自己套 DNS 记录每个区域范围。 同一记录可能存在多个范围，使用不同的 IP 地址或同一 IP 地址。

>[!NOTE]
>默认情况下，区域范围存在上 DNS 区域。 此区域范围作为区域，具有相同的名称，并传统 DNS 运营处理此范围。

在应用程序负载平衡以前方案演示如何北美配置的数据中心中的三个区域范围。

使用以下命令，则可以创建两个更多区域范围，分别对应 Dublin 和阿姆斯特丹数据中心。 

你可以添加到同一区域范围三个现有北美区域的这些区域范围，无需进行任何更改。 此外，你创建的这些区域范围后，你不需要重启 DNS 服务器。

可以使用下面的 Windows PowerShell 命令来创建区域范围。

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

有关详细信息，请参阅[添加 DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

###<a name="bkmk_records2"></a>添加到区域范围记录

现在，你必须添加到了区域范围表示 web 服务器主机的记录。

在以前的情况下添加美国数据中心的记录。 可以使用下面的 Windows PowerShell 命令添加到区域范围欧洲数据中心记录。
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

有关详细信息，请参阅[添加 DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx)。

###<a name="bkmk_policies2"></a>创建 DNS 策略

你已创建分区 （区域范围） 并添加了记录后，你必须创建 DNS 传入查询分布这些范围的策略。

例如，跨不同数据中心中的应用程序服务器查询 distribution 满足以下条件。

1. 当 DNS 查询收到北美的客户端子网中, 来源 DNS 响应 50%指向西雅图数据中心中的响应 25%指向芝加哥数据中心中，并的响应其余 25%指向达拉斯数据中心。
2. 在欧洲的客户端子网来源收到 DNS 查询时，50 %dns 响应指向 Dublin 数据中心中，和 DNS 响应 50%指向阿姆斯特丹数据中心。
3. 如果查询来自其他任何地方世界中，所有五数据中心分布 DNS 响应。

可以使用下面的 Windows PowerShell 命令来实现这些 DNS 策略。

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx)。

现在，你已成功创建之间位于在多个大洲五个不同的数据中心中的 Web 服务器提供应用程序负载平衡 DNS 策略。

你可以管理要求根据您的通信中创建数千 DNS 策略，并且无需重新启动 DNS 服务器，在传入查询动态-应用的所有新策略。
