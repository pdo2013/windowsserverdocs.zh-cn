---
title: 基于时间的 DNS 响应和 Azure 云应用服务器
description: 本主题是 DNS 策略方案指南 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 68f30973ef58b64006181990425e6ca84c39c059
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812036"
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>基于时间的 DNS 响应和 Azure 云应用服务器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题，了解如何通过使用基于一天的时间的 DNS 策略将应用程序流量分配到不同的地理位置分散的应用程序实例。 

此方案是在你想要将流量定向到备用的应用程序服务器，如 Web 服务器托管在 Microsoft Azure 位于其他时区中的一个时区的情况下很有用。 这使您流量进行负载平衡应用程序实例在高峰时间的段时主服务器超载的流量。 

> [!NOTE]
> 若要了解如何使用 DNS 策略的智能 DNS 响应而无需使用 Azure，请参阅[智能 DNS 响应的基于的日期时间使用 DNS 策略](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md)。 

## <a name="example-of-intelligent-dns-responses-based-on-the-time-of-day-with-azure-cloud-app-server"></a>基于 Azure 云应用程序服务器的一天的时间智能 DNS 响应的示例

下面是举例说明如何使用 DNS 策略应用到应用程序传入流量进行均衡基于一天的时间。

此示例使用一个虚构公司 Contoso 礼品服务，从而通过其网站在全球范围内提供联机赠与解决方案，contosogiftservices.com。 

仅在西雅图 （使用公共 IP 192.68.30.2) 的单个本地数据中心内托管 contosogiftservices.com web 站点。 

DNS 服务器也位于本地数据中心。 

使用最新业务激增，contosogiftservices.com 数值还高的访问者的每一天和某些客户报告的服务可用性问题。 

Contoso 礼品服务执行的站点分析，并发现每天下午 6 点和晚上 9 点本地时间之间存在到西雅图 Web 服务器流量是剧增。 Web 服务器不能缩放以处理增加的流量在这些高峰时间，从而导致拒绝服务的客户。 

若要确保 contosogiftservices.com 客户网站上获得一种响应体验，Contoso 礼品服务决定，在这些小时数期间它将租用虚拟机\(VM\)上托管的 Web 服务器的复制的 Microsoft Azure.  

Contoso 礼品服务从 Azure VM (192.68.31.44) 中获取的公共 IP 地址和开发自动化部署 Web 服务器之间 5-晚上 10 点，允许为一小时应变期间在 Azure 上每一天。

> [!NOTE]
> 有关 Azure Vm 的详细信息，请参阅[虚拟机文档](https://azure.microsoft.com/documentation/services/virtual-machines/) 

DNS 服务器配置区域作用域和 DNS 策略，以便之间 5-9 PM 每一天，30%的查询发送到在 Azure 中运行的 Web 服务器的实例。

下图描绘了此方案。

![一天的响应时间的 DNS 策略](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)  

## <a name="how-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server-works"></a>如何智能 DNS 响应基于 Azure 的一天的时间应用程序服务器的工作
 
本文演示如何配置 DNS 服务器来应答 DNS 查询具有两个不同的应用程序服务器 IP 地址-一台 web 服务器是在西雅图和另一种是在 Azure 数据中心。

后开始算起的峰值时间的下午 6 点到晚上 9 点在西雅图的新 DNS 策略的配置，DNS 服务器将发送到客户端包含西雅图 Web 服务器的 IP 地址对 DNS 响应的 70%和 30%的客户端的 DNS 响应nts 包含 Azure Web 服务器的 IP 地址，从而客户端将流量定向到新的 Azure Web 服务器，并防止西雅图 Web 服务器过载。 

一天中的所有其他时间正常查询处理将会发生，并从默认区域作用域，其中包含的记录中的本地数据中心的 web 服务器发送响应。 

10 分钟内在 Azure 记录的 TTL 可确保从 Azure 中删除 VM 之前，从 LDNS 缓存过期记录。 此类扩展的优势之一是您可以将保留在 DNS 数据在本地，并保持向外扩展到 Azure 根据需要。

## <a name="how-to-configure-dns-policy-for-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server"></a>如何为基于 Azure 的应用程序服务器的一天的时间智能 DNS 响应中配置 DNS 策略

若要配置基于时间的一天的应用程序负载平衡的查询响应的 DNS 策略，必须执行以下步骤。

- [创建区域作用域](#create-the-zone-scopes)
- [将记录添加到区域作用域](#add-records-to-the-zone-scopes)
- [创建 DNS 策略](#create-the-dns-policies)

> [!NOTE]
> 你想要配置的区域具有权威的 DNS 服务器上，必须执行这些步骤。 DnsAdmins 或等效身份的成员身份才能执行以下过程。 

以下部分提供详细的配置说明。

> [!IMPORTANT]
> 以下部分包含示例 Windows PowerShell 命令包含多个参数的示例值。 请确保将这些命令中的示例值替换之前运行这些命令适用于你的部署的值为。 


### <a name="create-the-zone-scopes"></a>创建区域作用域

区域作用域是该区域的唯一实例。 DNS 区域可以有多个区域作用域，包含其自己的 DNS 记录集的每个区域作用域。 同一条记录可出现在多个作用域，使用不同的 IP 地址或相同的 IP 地址。 

> [!NOTE]
> 默认情况下，区域作用域的 DNS 区域上存在。 此区域作用域为该区域具有相同的名称和旧的 DNS 操作适用于此作用域。 

以下示例命令可用于创建要托管的 Azure 记录的区域作用域。

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

有关详细信息，请参阅[添加 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="add-records-to-the-zone-scopes"></a>将记录添加到区域作用域
下一步是添加到区域作用域表示 Web 服务器主机的记录。 

在 AzureZoneScope，记录 www.contosogiftservices.com 添加 IP 地址 192.68.31.44，位于 Azure 公有云。 

同样，在默认区域作用域\(contosogiftservices.com\)，一条记录\(www.contosogiftservices.com\)添加 IP 地址 192.68.30.2 在西雅图本地运行的 Web 服务器数据中心。

在以下第二个 cmdlet，则不包含区域范围 – 区域参数。 因此，在默认区域范围区域中添加记录。 

此外，Azure Vm 的记录的 TTL 保留在 600s （10 分钟），以便 LDNS 较长的时间-这会干扰负载平衡不缓存它。 此外，Azure Vm 可作为应急以确保即使已缓存记录客户端能够解析 1 小时。

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

有关详细信息，请参阅[添加 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。  

### <a name="create-the-dns-policies"></a>创建 DNS 策略 
创建区域作用域后，您可以创建跨相应范围分发传入的查询，将发生以下情况的 DNS 策略。

1. 从每日晚上 9 点到下午 6 点，30%的客户端 Web 服务器的 IP 地址在 DNS 响应中的 Azure 数据中心时接收 70%的客户端收到的西雅图的本地 Web 服务器的 IP 地址。
2. 在所有其他情况下，所有客户端接收西雅图的本地 Web 服务器的 IP 地址。

在一天的时间必须在 DNS 服务器的本地时间表示。

您可以使用下面的示例命令创建 DNS 策略。

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
现在已具有所需的 DNS 策略，以将流量重定向到 Azure Web 服务器上的时间基于配置的 DNS 服务器。 

请注意表达式：

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” 
`

此表达式使用的区域范围区域和权重的组合，用于指示 DNS 服务器发送 70%的时间，同时发送 30%的时间的 Azure Web 服务器的 IP 地址的西雅图 Web 服务器的 IP 地址配置 DNS 服务器。

您可以管理要求，根据你的流量中创建数千个 DNS 策略且无需重新启动 DNS 服务器-在传入的查询上动态-应用所有新策略。
