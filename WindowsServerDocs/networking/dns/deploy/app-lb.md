---
title: 使用 DNS 策略执行应用程序负载平衡
description: 本主题是 DNS 策略方案指南 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dca60fc0e216b1b873bd4f94dd1b01174d80fc14
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446440"
---
# <a name="use-dns-policy-for-application-load-balancing"></a>使用 DNS 策略执行应用程序负载平衡

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题以了解如何配置 DNS 策略执行应用程序负载均衡。

以前版本的 Windows Server DNS 仅提供负载平衡使用轮循机制响应;但通过 Windows Server 2016 中的 DNS，您可以配置 DNS 策略的应用程序负载均衡。

当已部署应用程序的多个实例时，可以使用 DNS 策略来平衡不同应用程序实例，从而动态分配的应用程序的流量负载之间的流量负载。

## <a name="example-of-application-load-balancing"></a>应用程序负载平衡的示例

下面是举例说明如何使用 DNS 策略的应用程序负载均衡。

此示例使用一个虚构公司 Contoso 礼品服务-提供联机 gifing 服务，并且具有一个名为网站**contosogiftservices.com**。

Contosogiftservices.com 网站托管在多个数据中心，每个包含不同的 IP 地址。

在北美，这是主 Contoso 礼品服务市场，在三个数据中心中托管网站：伊利诺伊州、 德克萨斯州达拉斯市和华盛顿州西雅图。

西雅图 Web 服务器具有最佳的硬件配置，并且可以作为其他两个站点处理倍负载。 Contoso 礼品服务想按以下方式定向的应用程序流量。

- 由于西雅图 Web 服务器包含更多资源，一半的应用程序的客户端将定向到此服务器
- 一个季度的应用程序的客户端定向到德克萨斯州达拉斯市数据中心
- 一个季度的应用程序的客户端定向到伊利诺伊州，数据中心

下图描绘了此方案。

![DNS 应用程序负载均衡使用 DNS 策略](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>如何应用程序负载平衡的工作原理

配置后的应用程序的 DNS 策略的 DNS 服务器加载平衡使用此示例方案中，DNS 服务器做出响应的 50%的西雅图 Web 服务器地址，25%的时间与 Dallas Web 服务器地址，并使用时间的 25%的时间芝加哥 Web 服务器地址。

因此对于每个 DNS 服务器收到的四个查询，它响应与两个响应西雅图和两个分别用于 Dallas 和芝加哥。

一个负载平衡和 DNS 策略可能出现问题是由 DNS 客户端和冲突解决程序/LDNS，这会干扰负载均衡，因为客户端或冲突解决程序不执行向 DNS 服务器发送查询的 DNS 记录的缓存。

可以通过使用较低的时间来缓解这种行为的影响\-到\-Live \(TTL\) DNS 记录，应进行负载均衡的值。

### <a name="how-to-configure-application-load-balancing"></a>如何配置应用程序负载均衡

以下各节演示如何配置应用程序负载均衡的 DNS 策略。

#### <a name="create-the-zone-scopes"></a>创建区域作用域

必须先创建的范围区域 contosogiftservices.com 为数据中心托管位置。

区域作用域是该区域的唯一实例。 DNS 区域可以有多个区域作用域，包含其自己的 DNS 记录集的每个区域作用域。 同一条记录可出现在多个作用域，使用不同的 IP 地址或相同的 IP 地址。

>[!NOTE]
>默认情况下，区域作用域的 DNS 区域上存在。 此区域作用域为该区域具有相同的名称和旧的 DNS 操作适用于此作用域。

可以使用以下 Windows PowerShell 命令以创建区域作用域。
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

有关详细信息，请参阅[添加 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

#### <a name="bkmk_records"></a>将记录添加到区域作用域

现在，必须添加到区域作用域表示 web 服务器主机的记录。

在中**SeattleZoneScope**，可以添加 www.contosogiftservices.com 记录 IP 地址为 192.0.0.1，它位于西雅图数据中心。

在中**ChicagoZoneScope**，可以添加同一条记录\(www.contosogiftservices.com\)具有 IP 地址 182.0.0.1 芝加哥数据中心内的。

同样在**DallasZoneScope**，可以添加一条记录\(www.contosogiftservices.com\)具有 IP 地址 162.0.0.1 芝加哥数据中心内的。

可以使用以下 Windows PowerShell 命令将记录添加到区域作用域。
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

有关详细信息，请参阅[添加 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

#### <a name="bkmk_policies"></a>创建 DNS 策略

已创建的分区 （区域作用域），并且已将记录添加后，您必须创建跨相应范围分发传入的查询，使其 50%的 contosogiftservices.com 的查询响应的 IP 地址与 web 的 DNS 策略在芝加哥和达拉斯数据中心之间平均分发西雅图数据中心和其余部分中的服务器时。

可以使用以下 Windows PowerShell 命令来创建应用程序流量均衡分配给这些三个数据中心的 DNS 策略。

>[!NOTE]
>在下面的表达式的示例命令中 – 区域范围区域"SeattleZoneScope，2;ChicagoZoneScope，1;DallasZoneScope，1"配置 DNS 服务器使用一个数组，其中包括参数组合\<区域范围区域\>，\<权重\>。
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

有关详细信息，请参阅[添加 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  

现在已成功创建三个不同数据中心中的 Web 服务器之间提供应用程序负载平衡的 DNS 策略。

您可以管理要求，根据你的流量中创建数千个 DNS 策略且无需重新启动 DNS 服务器-在传入的查询上动态-应用所有新策略。
