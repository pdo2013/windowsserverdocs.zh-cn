---
title: 基于时间的 Azure 的一天中的 DNS 响应云应用服务器
description: 本主题介绍 DNS 策略方案指南为 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3255d3fe29f6a7dda821183fa4352964cc230a5f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>基于时间的 Azure 的一天中的 DNS 响应云应用服务器

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解如何使用的基于当天的时间 DNS 策略应用程序交通分散不同的应用程序的地理位置分布式实例。 

这种情况下是在你想要一个时区备用应用程序的服务器，如 Web 服务器位于 Microsoft Azure 都位于另一台时区的区域中直接通信的情况下有用。 这允许你以高峰跨应用程序实例加载余额通信时间的段交通时过你的主服务器。 

>[!NOTE]
>若要了解如何使用的智能 DNS 响应 DNS 策略，而无需使用 Azure，请参阅[使用 DNS 策略智能 DNS 响应基于一天时间](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md)。 

## <a name="bkmk_azureexample"></a>基于使用 Azure 云应用服务器的一天的时间的智能 DNS 响应它的示例

以下是一种方式，你可以使用 DNS 策略于余额应用程序流量基于当天的时间。

此示例虚构公司，提供联机赠送解决方案通过他们的网站 contosogiftservices.com 全球范围内的 Contoso 礼品服务。 

仅在西雅图（使用公用 IP 192.68.30.2) 一个本地 datacenter 托管 contosogiftservices.com 网站。 

在本地 datacenter 也位于 DNS 服务器。 

在企业中最近电涌，contosogiftservices.com 有更多的访客每天，并且某些客户报告服务可用性问题。 

Contoso 礼品服务执行站点分析，，将会发现之间 6 下午与 9 本地时间，每个晚上电涌到西雅图 Web 服务器流量。 Web 服务器无法扩展以在高峰时段，处理增加的交通向客户导致拒绝服务。 

若要确保 contosogiftservices.com 客户能够从网站响应体验，Contoso 礼品服务决定，在以下时间内，它将租赁虚拟计算机上对其 Web 服务器的副本主机的 Microsoft Azure \(VM\) 中。  

Contoso 礼品服务获取公共的 IP 地址从 Azure 虚拟机 (192.68.31.44），并开发自动化部署每一天都会在 Azure 上之间 5-10 下午，允许一小时应急期间的 Web 服务器。

>[!NOTE]
>有关 Azure 虚拟机的功能的详细信息，请参阅[虚拟机文档](https://azure.microsoft.com/documentation/services/virtual-machines/) 

DNS 服务器，以便在 5 9 下午每天，之间 30%查询发送对在 Azure 上运行的 Web 服务器实例区域范围和 DNS 策略配置。

下图显示了这种情况。

![DNS 策略的一天响应时间](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)  

## <a name="bkmk_azurehow"></a>如何智能 DNS 响应基于使用 Azure 的一天的时间应用服务器的工作原理
 
本文说明如何配置 DNS 服务器回答 DNS 查询的两个不同的应用程序 server IP 地址的一个 web 服务器在西雅图并且另一种是 Azure 数据中心中。

基于高峰时段的 6 9 下午至西雅图新 DNS 策略配置之后, DNS 服务器发送七十 %DNS 响应包含 IP 地址西雅图 Web 服务器的客户端和三十 %到包含 Azure Web 服务器的 IP 地址的客户端 DNS 响应从而定向到新的 Azure Web 服务器的客户端路况和变得重载阻止西雅图 Web 服务器。 

一天，在所有其他情况下正常查询处理发生和响应，从默认区域范围，其中包含本地数据中心中的 web 服务器记录发送。 

在 Azure 记录 10 分钟 TTL 确保 VM 从 Azure 删除之前，从 LDNS 缓存过期记录。 此类缩放的优势是你可以保留你 DNS 数据本地，并查看保留缩放根据需要 Azure。

## <a name="bkmk_azureconfigure"></a>如何为智能 DNS 响应基于与 Azure 应用服务器的一天时间配置 DNS 策略
若要配置基于一天应用程序负载平衡时间查询响应 DNS 策略，你必须执行以下步骤。


- [创建区域范围](#bkmk_zscopes)
- [添加到区域范围记录](#bkmk_records)
- [创建 DNS 策略](#bkmk_policies)


>[!NOTE]
>你必须在你想要配置区域的授权 DNS 服务器执行这些步骤。 会员 DnsAdmins，或等效，需要执行以下步骤。 

以下部分提供了详细的配置说明进行操作。

>[!IMPORTANT]
>以下部分包括包含许多参数示例值的示例 Windows PowerShell 命令。 确保你在运行以下命令之前就提供适用于你的部署的值与替换示例值在这些命令。 


### <a name="bkmk_zscopes"></a>创建区域范围
区域范围是唯一区域的实例。 DNS 区域有多个区域范围包含自己套 DNS 记录每个区域范围。 同一记录可能存在多个范围，使用不同的 IP 地址或同一 IP 地址。 

>[!NOTE]
>默认情况下，区域范围存在上 DNS 区域。 此区域范围作为区域，具有相同的名称，并传统 DNS 运营处理此范围。 

可以使用下面的示例命令创建 Azure 记录的宿主区域范围。

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

有关详细信息，请参阅[添加 DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

### <a name="bkmk_records"></a>添加到区域范围记录
下一步是添加到区域范围表示 Web 服务器主机的记录。 

在 AzureZoneScope，记录 www.contosogiftservices.com IP 地址 192.68.31.44，位于公共 Azure 云添加。 

同样，在默认区域范围 \(contosogiftservices.com\)，记录 \(www.contosogiftservices.com\) 添加 192.68.30.2 运行西雅图本地数据中心中的 Web 服务器的 IP 地址。

下面的第二个 cmdlet，不包括该区域范围 – 区域参数。 出于此原因，默认区域范围区域中添加记录。 

此外，Azure 虚拟机的功能的记录 TTL 保留在 600s（10 分钟），以便 LDNS 较长的时间-这会干扰负载平衡不缓存它。 此外，作为紧急以确保缓存记录与甚至客户端无法解决 1 额外的 1 小时 Azure 虚拟机的功能都可用。

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

有关详细信息，请参阅[添加 DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx)。  

### <a name="bkmk_policies"></a>创建 DNS 策略 
在创建区域范围后，你可以创建在这些范围分发传入查询，以便将发生下列情况的 DNS 策略。

1. 从 6 每日晚上 9 到的项目经理的客户端 30%接收 IP 地址的 Web 服务器的在 Azure 数据中心中的 DNS 响应时 70%的客户端接收西雅图本地 Web 服务器的 IP 地址。
2. 在所有其他情况下的客户端接收西雅图本地 Web 服务器的 IP 地址。

一天的时间必须表达中的 DNS 服务器的本地时间。

你可以使用下面的示例命令创建 DNS 策略。

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW –-ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx)。  
  
现在 DNS 服务器具有所需的 DNS 策略重定向到 Azure Web 服务器基于当天的时间通信配置。 

注意 expression:

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” 
`

此 expression 指示 DNS 服务器发送西雅图 Web 服务器的 IP 地址七十 %的时间，同时发送 Azure Web 服务器的 IP 地址的情况下三十 %的区域范围区域和粗细组合使用来配置 DNS 服务器。

你可以管理要求根据您的通信中创建数千 DNS 策略，并且无需重新启动 DNS 服务器，在传入查询动态-应用的所有新策略。
